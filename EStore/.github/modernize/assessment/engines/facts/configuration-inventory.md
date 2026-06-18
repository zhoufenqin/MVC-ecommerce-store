# Configuration & Externalized Settings Inventory

This document inventories EStore's configuration sources and externalized settings across build/runtime contexts, including sensitive configuration locations.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | .NET web runtime config | `EStore.WebUI/Web.config` | Connection string, appSettings, auth, EF provider entries |
| App.config | .NET library config | `EStore.Domain/App.config` | EF provider setup for domain project |
| packages.config | NuGet package declarations | `EStore.*/*/packages.config` | Declares package versions per project |
| Solution file | Build orchestration | `EStore/EStore.sln` | Defines project composition and build configurations |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Build configuration | Local development and debugging | Standard MSBuild C# targets |
| Release | Build configuration | Optimized production build artifacts | Standard MSBuild C# targets |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default (IIS/ASP.NET) | Web application startup | `Web.config` | Forms authentication, EF connection string, appSettings |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| EFDbContext (connection string) | LocalDB EStore database | Default | Web.config connectionStrings |
| webpages:Version | 3.2.2.0 | Default | Web.config appSettings |
| ClientValidationEnabled | true | Default | Web.config appSettings |
| UnobtrusiveJavaScriptEnabled | true | Default | Web.config appSettings |
| Email.WriteAsFile | true | Default | Web.config appSettings |
| forms loginUrl | `~/Account/Login` | Default | Web.config system.web/authentication |
| forms timeout | 2880 | Default | Web.config system.web/authentication |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| EStore.WebUI | ASP.NET on .NET Framework 4.5.1 (IIS/IIS Express) | Not explicitly configured | 1 (local/default) |
| EStore.Domain | In-process class library | Inherited from host process | N/A |

## Startup Dependency Chain

1. `EStore.WebUI` starts and registers MVC routes plus cart model binder.
2. Ninject bootstraps dependency resolution during application start.
3. Domain services become available in-process.
4. SQL Server LocalDB and SMTP are required when corresponding operations execute.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Forms auth credential (admin/password) | Authentication credential | Web.config (stored in clear text, value masked here) |
| SMTP username/password defaults | Mail credential | Domain `EmailSettings` code defaults (masked) |

### Secrets Provisioning Workflow

Secrets are currently sourced from static configuration/code defaults rather than external secret stores. Authentication credentials are read by ASP.NET Forms Authentication from Web.config, and SMTP credentials are loaded from `EmailSettings` during order processing. No managed identity, vault integration, or automated secret rotation workflow was detected.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| Email.WriteAsFile | true | Web.config appSetting |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.5.1 | Project files (`TargetFrameworkVersion`) |
| ASP.NET MVC | 5.2.3 | packages.config |
| Entity Framework | 6.1.2 | packages.config |
| Ninject | 3.2.2.0 / 3.2.3.0 | packages.config |
| jQuery | 2.1.3 | packages.config |
| Bootstrap | 3.3.2 | packages.config |
