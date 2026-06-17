# Configuration & Externalized Settings Inventory

The EStore application is configured through a small set of XML-based `.config` files (ASP.NET `Web.config` and MSBuild transformation files), with two build configurations (Debug/Release) and no runtime profile system or external configuration service.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|--------|------|--------------|-------|
| `Web.config` | Primary application config | `EStore.WebUI/Web.config` | Connection strings, appSettings, Forms Authentication, EF provider registration |
| `Web.Debug.config` | Build transform (Debug) | `EStore.WebUI/Web.Debug.config` | No active overrides — template only |
| `Web.Release.config` | Build transform (Release) | `EStore.WebUI/Web.Release.config` | Removes `debug="true"` compilation attribute |
| `Views/Web.config` | Razor view engine config | `EStore.WebUI/Views/Web.config` | Locks down direct view access; declares Razor host and namespaces |
| `App.config` | Domain project config | `EStore.Domain/App.config` | EF provider registration for Domain class library (used during test/design-time) |
| No external config server | — | — | No Spring Cloud Config, Azure App Configuration, AWS AppConfig, or Consul |
| No secret store integration | — | — | No HashiCorp Vault, Azure Key Vault, or AWS Secrets Manager |

## Build Profiles

| Profile | Activation | Purpose | Key Changes |
|---------|-----------|---------|------------|
| Debug | Default in Visual Studio; `-c Debug` in MSBuild | Development build with debug symbols | `compilation debug="true"` in `Web.config`; full PDB output |
| Release | Manual (`-c Release` or publish wizard) | Production-ready build | `Web.Release.config` transform removes `debug="true"` from `<compilation>`; optimized output |

No additional MSBuild profiles, conditional compilation symbols, or custom build targets are defined.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---------|-----------------|-------------|--------------|
| (single environment) | No runtime profile system | `Web.config` only | All environments share the same config; no `appsettings.{Environment}.json` or `application-{profile}.properties` equivalent |

> Note: ASP.NET MVC 5 on .NET Framework 4.5.1 does not use `ASPNETCORE_ENVIRONMENT` or the `IConfiguration` profile system. Environment-specific values would require manual `Web.config` transformation or external tooling.

## Properties Inventory

### EStore.WebUI — `Web.config` appSettings

| Property Key | Default Value | Profiles | Source |
|-------------|--------------|---------|--------|
| `webpages:Version` | `3.2.2.0` | All | `Web.config` |
| `webpages:Enabled` | `false` | All | `Web.config` |
| `ClientValidationEnabled` | `true` | All | `Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | All | `Web.config` |
| `Email.WriteAsFile` | `true` | All | `Web.config` (read by `NinjectWebCommon` to configure `EmailSettings.WriteAsFile`) |

### EStore.WebUI — Connection Strings

| Name | Value | Profiles | Source |
|------|-------|---------|--------|
| `EFDbContext` | `Data Source=(localdb)\v11.0;Initial Catalog=EStore;Integrated Security=True` | All | `Web.config` |

### EStore.WebUI — Authentication

| Setting | Value | Profiles | Source |
|---------|-------|---------|--------|
| `authentication mode` | `Forms` | All | `Web.config` `<system.web>` |
| `forms loginUrl` | `~/Account/Login` | All | `Web.config` |
| `forms timeout` | `2880` minutes (48 hours) | All | `Web.config` |
| `credentials passwordFormat` | `Clear` (plaintext) | All | `Web.config` |
| Admin username | `admin` | All | `Web.config` `<credentials>` (hardcoded) |
| Admin password | `password` | All | `Web.config` `<credentials>` (hardcoded plaintext) |

### EStore.WebUI — Compilation

| Setting | Debug | Release | Source |
|---------|-------|---------|--------|
| `compilation debug` | `true` | `false` (via transform) | `Web.config` / `Web.Release.config` |
| `targetFramework` | `4.5.1` | `4.5.1` | `Web.config` |

### EStore.Domain — App.config

| Setting | Value | Notes |
|---------|-------|-------|
| EF default connection factory | `System.Data.Entity.Infrastructure.SqlConnectionFactory` | Used during design-time/test |
| EF SQL provider | `System.Data.Entity.SqlServer.SqlProviderServices` | SQL Server provider registration |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | CPU | Instance Count |
|---------|---------------|--------|-----|---------------|
| EStore.WebUI (IIS) | No explicit JVM or CLR startup flags configured | Not specified | Not specified | 1 (single IIS application) |

No Docker, Kubernetes, or cloud deployment configuration is present. The application is intended to run on a local IIS or IIS Express instance.

## Startup Dependency Chain

```
SQL Server LocalDB (v11.0)  ← must be running before first DB request
        ↓
EStore.WebUI (IIS / IIS Express)
```

There is no wait mechanism, health check probe, or readiness gate. The application starts unconditionally; a missing or unavailable LocalDB instance will surface as an unhandled `SqlException` on the first database call (lazy EF initialization). No config server dependency exists.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage | Status |
|-----------------|------|---------|--------|
| `EFDbContext` connection string | Database connection | Plaintext in `Web.config` | Uses Integrated Security (no password in string) |
| Admin `password` | Admin credentials | Plaintext in `Web.config` `<credentials>` | **Critical risk** — hardcoded cleartext password |
| `EmailSettings.UserName` | SMTP credential | Hardcoded default in `EmailSettings` class | Placeholder value `"MySmtpUserName"` — not a real secret |
| `EmailSettings.Password` | SMTP credential | Hardcoded default in `EmailSettings` class | Placeholder value `"MySmtpPassword"` — not a real secret |

> **Warning**: The admin password (`password`) is stored in cleartext in `Web.config` under `<credentials passwordFormat="Clear">`. This is a critical security vulnerability. No encryption, DPAPI, Azure Key Vault, or external secret store is in use.

### Secrets Provisioning Workflow

There is no automated secrets provisioning workflow. All secrets are statically embedded in `Web.config`:

1. **Admin credentials** are defined inline in `Web.config` using ASP.NET Forms Authentication `<credentials>` with `passwordFormat="Clear"`.
2. **Database access** uses Windows Integrated Security (`Integrated Security=True`) — no password required in the connection string, but the application process must run under an account with LocalDB access.
3. **No secrets rotation, injection from environment variables, CI/CD secret injection, or secret store integration** is implemented.

## Feature Flags

| Flag | Default | Controlled By | Behavior |
|------|---------|--------------|---------|
| `Email.WriteAsFile` | `true` | `Web.config` appSettings | When `true`, `EmailOrderProcessor` writes `.eml` files to `c:\sports_store_emails` instead of sending real SMTP email |
| `ClientValidationEnabled` | `true` | `Web.config` appSettings | Enables client-side jQuery validation in MVC forms |
| `UnobtrusiveJavaScriptEnabled` | `true` | `Web.config` appSettings | Enables unobtrusive JavaScript for MVC HTML helpers |

No feature flag framework (LaunchDarkly, Unleash, .NET `Microsoft.FeatureManagement`) is in use. The `Email.WriteAsFile` flag serves as the only runtime toggle.

## Framework & Runtime Versions

| Component | Version | Source |
|-----------|---------|--------|
| Target Runtime | .NET Framework 4.5.1 | `EStore.WebUI.csproj` `<TargetFrameworkVersion>` |
| ASP.NET MVC | 5.2.3 | `packages.config` |
| ASP.NET Razor | 3.2.3 | `packages.config` |
| ASP.NET WebPages | 3.2.3 | `packages.config` |
| Entity Framework | 6.1.2 | `packages.config` |
| Ninject | 3.2.2.0 | `packages.config` |
| Ninject.MVC3 | 3.2.1.0 | `packages.config` |
| Ninject.Web.Common | 3.2.3.0 | `packages.config` |
| WebActivatorEx | 2.0.6 | `packages.config` |
| Bootstrap | 3.3.2 | `packages.config` |
| jQuery | 2.1.3 | `packages.config` |
| jQuery.Validation | 1.13.1 | `packages.config` |
| Microsoft.jQuery.Unobtrusive.Validation | 3.0.0 | `packages.config` |
| Moq (test) | 4.2.1502.0911 | `packages.config` |
| Build Tool | MSBuild (via Visual Studio 2013 solution format) | `EStore.sln` |
| Package Manager | NuGet (packages.config format) | `packages.config` files |
| Visual Studio Solution Format | 12.00 (VS 2013) | `EStore.sln` header |
