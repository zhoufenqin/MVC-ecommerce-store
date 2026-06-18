# Dependency Map

This document summarizes declared external dependencies for EStore across its .NET projects (16 primary runtime/build dependencies excluding test-only categorization).

## Dependencies

```mermaid
flowchart LR
    App["EStore Solution"]

    subgraph Web["Web Frameworks"]
        AspMvc["Microsoft.AspNet.Mvc 5.2.3"]
        Razor["Microsoft.AspNet.Razor 3.2.3"]
        WebPages["Microsoft.AspNet.WebPages 3.2.3"]
    end
    subgraph DB["Database / ORM"]
        EF["EntityFramework 6.1.2"]
    end
    subgraph Log["Logging"]
        LogNode["No dedicated logging package"]
    end
    subgraph Sec["Security"]
        Ninject["Ninject 3.2.2.0"]
        NinjectMvc["Ninject.MVC3 3.2.1.0"]
        NinjectWeb["Ninject.Web.Common 3.2.3.0"]
    end
    subgraph Util["Utilities"]
        WebInfra["Microsoft.Web.Infrastructure 1.0.0.0"]
        JQuery["jQuery 2.1.3"]
        JQueryM["jQuery.Migrate 1.2.1"]
        JQueryVal["jQuery.Validation 1.13.1"]
        JQueryUnob["Microsoft.jQuery.Unobtrusive.Validation 3.0.0"]
        Bootstrap["bootstrap 3.3.2"]
        WebActivator["WebActivatorEx 2.0.6"]
    end

    App -->|"web"| Web
    App -->|"persistence"| DB
    App -->|"security and DI"| Sec
    App -->|"utilities and UI libs"| Util
    App -->|"logging"| Log
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 3 | Microsoft.AspNet.Mvc, Razor, WebPages | Legacy ASP.NET MVC 5 stack |
| Database / ORM | 1 | EntityFramework 6.1.2 | EF6 on .NET Framework |
| Security | 3 | Ninject, Ninject.MVC3, Ninject.Web.Common | Primarily DI wiring |
| Utilities | 8 | jQuery, Bootstrap, WebActivatorEx | Mix of UI and ASP.NET support packages |
| Logging | 0 | N/A | Uses framework defaults only |

### Version & Compatibility Risks

The solution targets .NET Framework 4.5.1 and uses older packages (for example EF 6.1.2 and ASP.NET MVC 5.2.3), which are maintainable but outdated for modern .NET targets. Migration to net10.0 will require replacing legacy System.Web-based framework dependencies.

### Notable Observations

- Dependency model uses `packages.config` instead of newer PackageReference format.
- No dedicated structured logging library is declared.
- Ninject MVC3 integration is legacy and may need replacement during upgrade.
- Moq is present in both WebUI and UnitTests package lists but treated as test support dependency.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| MSTest (Microsoft.VisualStudio.QualityTools.UnitTestFramework) | Visual Studio test framework | Referenced directly in test project |
| Moq | 4.2.1502.0911 | Mocking library used in unit tests |

Total test-scope dependencies: 2

Test infrastructure exists through MSTest with Moq; it is based on older Visual Studio tooling and may need modernization when upgrading target framework.
