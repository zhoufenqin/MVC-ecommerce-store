# EStore.Domain

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 24 |
| Mandatory Blockers | 11 |
| Potential Issues | 7 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | .NETFramework,Version=v4.5.1 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Hardcoded local or network paths detected | Mandatory | 1 | [1](#Hardcoded_local_or_network_paths_detected) |
| Windows authentication detected | Mandatory | 3 | [1](#Windows_authentication_detected) |
| SMTP connections detected | Potential | 3 | [5](#SMTP_connections_detected) |
| Old .NET Framework dependency detected | Potential | 3 | [3](#Old_NET_Framework_dependency_detected) |
| Explicit password-based credentials usage is detected | Potential | 3 | [2](#Explicit_password-based_credentials_usage_is_detected) |
| SQL database connection detected | Potential | 3 | [1](#SQL_database_connection_detected) |
| Hardcoded sensitive data detected | Optional | 3 | [4](#Hardcoded_sensitive_data_detected) |
| Connection strings without configuration builders detected | Optional | 3 | [2](#Connection_strings_without_configuration_builders_detected) |
| Static content detected | Optional | 3 | [1](#Static_content_detected) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |

### Issue Details

<details id="Hardcoded_local_or_network_paths_detected">
<summary><b>Hardcoded local or network paths detected</b> — affected files</summary>

- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 94)`

</details>

<details id="Windows_authentication_detected">
<summary><b>Windows authentication detected</b> — affected files</summary>

- `EStore.WebUI\Web.config`

</details>

<details id="SMTP_connections_detected">
<summary><b>SMTP connections detected</b> — affected files</summary>

- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 72)`
- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 72)`
- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 37)`
- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 26)`
- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 26)`

</details>

<details id="Old_NET_Framework_dependency_detected">
<summary><b>Old .NET Framework dependency detected</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.UnitTests\EStore.UnitTests.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="Explicit_password-based_credentials_usage_is_detected">
<summary><b>Explicit password-based credentials usage is detected</b> — affected files</summary>

- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 32)`
- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 32)`

</details>

<details id="SQL_database_connection_detected">
<summary><b>SQL database connection detected</b> — affected files</summary>

- `EStore.WebUI\Web.config`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `EStore.Domain\Concrete\EmailOrderProcessor.cs (line 90)`
- `EStore.UnitTests\AdminSecurityTests.cs (line 24)`
- `EStore.UnitTests\AdminSecurityTests.cs (line 18)`
- `EStore.WebUI\Controllers\AccountController.cs (line 39)`

</details>

<details id="Connection_strings_without_configuration_builders_detected">
<summary><b>Connection strings without configuration builders detected</b> — affected files</summary>

- `EStore.WebUI\Web.config`
- `EStore.WebUI\Web.config`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `EStore.WebUI\Web.config`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| NuGet package is incompatible | Mandatory | 1 | [14](#NuGet_package_is_incompatible) |
| NuGet package functionality is included with framework reference | Mandatory | 1 | [8](#NuGet_package_functionality_is_included_with_framework_reference) |
| Binary incompatible for selected .NET version | Mandatory | 1 | [8](#Binary_incompatible_for_selected_NET_version) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [3](#Project_file_needs_to_be_converted_to_SDK-style) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [3](#Project_s_target_framework_s_needs_to_be_changed) |
| Routes registration via RouteCollection is not supported in .NET Core and needs to be converted to the route mappings on the application object | Mandatory | 1 | [2](#Routes_registration_via_RouteCollection_is_not_supported_in_NET_Core_and_needs_to_be_converted_to_the_route_mappings_on_the_application_object) |
| Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs | Mandatory | 1 | [1](#Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs) |
| Legacy Configuration System | Mandatory | 2 | 0 |
| ASP.NET Framework (System.Web) | Mandatory | 4 | 0 |
| Source incompatible for selected .NET version | Potential | 1 | [4](#Source_incompatible_for_selected_NET_version) |
| NuGet package upgrade is recommended | Potential | 1 | [2](#NuGet_package_upgrade_is_recommended) |
| AutoGenerateBindingRedirects not set and no manual redirects | Potential | 1 | [2](#AutoGenerateBindingRedirects_not_set_and_no_manual_redirects) |
| NuGet package contains security vulnerability | Optional | 1 | [3](#NuGet_package_contains_security_vulnerability) |
| NuGet package is deprecated | Optional | 1 | [2](#NuGet_package_is_deprecated) |

### Issue Details

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.UnitTests\EStore.UnitTests.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="NuGet_package_functionality_is_included_with_framework_reference">
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `EStore.UnitTests\EStore.UnitTests.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="Binary_incompatible_for_selected_NET_version">
<summary><b>Binary incompatible for selected .NET version</b> — affected files</summary>

- `EStore.WebUI\Infrastructure\Concrete\FormsAuthProvider.cs (line 16, col 16)`
- `EStore.WebUI\Infrastructure\Concrete\FormsAuthProvider.cs (line 13, col 12)`
- `EStore.WebUI\Global.asax.cs (line 16, col 12)`
- `EStore.WebUI\App_Start\RouteConfig.cs (line 11, col 8)`

</details>

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.UnitTests\EStore.UnitTests.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.UnitTests\EStore.UnitTests.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="Routes_registration_via_RouteCollection_is_not_supported_in_NET_Core_and_needs_to_be_converted_to_the_route_mappings_on_the_application_object">
<summary><b>Routes registration via RouteCollection is not supported in .NET Core and needs to be converted to the route mappings on the application object</b> — affected files</summary>

- `EStore.WebUI\Global.asax.cs`
- `EStore.WebUI\App_Start\RouteConfig.cs`

</details>

<details id="Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs">
<summary><b>Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs</b> — affected files</summary>

- `EStore.WebUI\Global.asax.cs`

</details>

<details id="Source_incompatible_for_selected_NET_version">
<summary><b>Source incompatible for selected .NET version</b> — affected files</summary>

- `EStore.WebUI\Infrastructure\NinjectDependencyResolver.cs (line 58, col 12)`
- `EStore.WebUI\Global.asax.cs (line 11, col 45)`

</details>

<details id="NuGet_package_upgrade_is_recommended">
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="AutoGenerateBindingRedirects_not_set_and_no_manual_redirects">
<summary><b>AutoGenerateBindingRedirects not set and no manual redirects</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.UnitTests\EStore.UnitTests.csproj`

</details>

<details id="NuGet_package_contains_security_vulnerability">
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `EStore.WebUI\EStore.WebUI.csproj`

</details>

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `EStore.Domain\EStore.Domain.csproj`
- `EStore.WebUI\EStore.WebUI.csproj`

</details>

---

## Codebase Insights

> **Note:** These documents are generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

1. **[Architecture Diagram](facts/architecture-diagram.md)** — Understand the big picture: system layers and component relationships
2. **[Dependency Map](facts/dependency-map.md)** — Know what the project depends on and where the risks are
3. **[API & Service Contracts](facts/api-service-contracts.md)** — See how services communicate and what contracts they expose
4. **[Data Architecture](facts/data-architecture.md)** — Explore data models, storage, and data flow patterns
5. **[Configuration Inventory](facts/configuration-inventory.md)** — Review how the application is configured across environments
6. **[Business Workflows](facts/business-workflows.md)** — Trace end-to-end business processes and domain logic

[Share feedback](https://aka.ms/ghcp-appmod/feedback)
