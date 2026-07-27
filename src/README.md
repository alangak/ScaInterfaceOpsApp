# ScaInterfaceOpsApp - Implementation Specification Prompt

You are a Senior .NET 8 Solution Architect and Developer.

Generate a production-quality .NET 8 solution based on the architecture defined below.

Do not introduce additional architectural patterns unless explicitly requested.

---

# Solution Name

ScaInterfaceOpsApp

---

# Architecture Decision

Implement:

- Simplified Clean Architecture
- Vertical Slice Architecture
- Dependency Injection
- Interface-based abstraction
- Options Pattern
- FluentValidation
- Result Pattern

Do NOT implement:

- CQRS
- MediatR
- Domain Driven Design
- Generic Repository
- Unit Of Work
- Event Sourcing
- Pipeline Behaviors
- Repository per Entity

The application is an operational workflow application, not a generic framework.

---

# Solution Structure

Create the following projects:

```
src/

ScaInterfaceOpsApp.Console
    Type: Console Application

ScaInterfaceOpsApp.Application
    Type: Class Library

ScaInterfaceOpsApp.Infrastructure
    Type: Class Library

ScaInterfaceOpsApp.Shared
    Type: Class Library


tests/

ScaInterfaceOpsApp.Tests
    Type: xUnit Test Project
```

---

# Project Dependency Rules

Allowed dependencies:

```
Console
    |
    +--> Application
    |
    +--> Infrastructure
    |
    +--> Shared


Infrastructure
    |
    +--> Application
    |
    +--> Shared


Application
    |
    +--> Shared


Tests
    |
    +--> Application
    |
    +--> Shared
```

Rules:

- Application must not reference Infrastructure.
- Application must not reference Azure SDKs.
- Application must not reference SQL libraries.
- Business logic must remain independent of external systems.

---

# Business Scope

The application manages SCA Interface Center lifecycle operations.

Current supported operations:

1. Center Onboarding
2. Center Offboarding

No other operations are required.

---

# Input

Input source is fixed.

The application receives an Excel workbook.

Excel is the only input source.

Do not design API input models.

Do not design database-driven input.

The Excel reader must be abstracted.

Flow:

```
Excel Workbook

        |

Excel Reader

        |

Request Models

        |

Operation Execution
```

---

# Center Onboarding Operation

Purpose:

Add new centers or update existing centers.

The operation internally determines whether the action is:

- Insert
- Update


Workflow:

```
Excel Data

    |

Validation

    |

Check Existing Center

    |

SQL Insert / Update

    |

Azure Key Vault Add / Update

    |

Azure Service Bus Filter Append

    |

Execution Result
```

---

# Center Offboarding Operation

Purpose:

Disable and cleanup existing centers.

Workflow:

```
Excel Data

    |

Validation

    |

SQL Flag / Delete

    |

Key Vault Cleanup

    |

Service Bus Filter Cleanup

    |

Execution Result
```

---

# Application Project Structure

Use vertical slice organization.

```
Application

    Abstractions

        Data

        Excel

        Messaging

        Secrets


    Operations

        CenterOnboarding

            Models

            Contracts

            Validators

            CenterOnboardingOperation.cs

            ICenterOnboardingOperation.cs


        CenterOffboarding

            Models

            Contracts

            Validators

            CenterOffboardingOperation.cs

            ICenterOffboardingOperation.cs
```

---

# Infrastructure Structure

```
Infrastructure

    SqlServer

        SqlRepository.cs


    KeyVault

        AzureKeyVaultService.cs


    ServiceBus

        AzureServiceBusAdministration.cs


    Excel

        ExcelReader.cs


    Configuration

        SqlOptions.cs

        KeyVaultOptions.cs

        ServiceBusOptions.cs

        ExcelOptions.cs
```

---

# Required Application Interfaces

Create:

```
ISqlRepository

IKeyVaultService

IServiceBusAdministration

IExcelReader
```

Interfaces belong in Application.

Implementations belong in Infrastructure.

---

# SQL Requirements

Provide placeholder implementation for:

- Execute stored procedure
- Query single record
- Query collection

Use:

- Dapper
- Microsoft.Data.SqlClient

No Entity Framework.

---

# Azure Key Vault Requirements

Provide placeholder implementation for:

- Get Secret
- Create Secret
- Update Secret
- Delete Secret

Use:

- Azure.Identity
- Azure.Security.KeyVault.Secrets

---

# Azure Service Bus Requirements

Provide placeholder implementation for:

- Add Subscription Filter
- Update Filter
- Remove Filter
- Remove Subscription

Use:

- Azure Service Bus Administration Client

Important:

For Center Onboarding:

Service Bus subscriptions already exist.

The operation only appends filters.

---

# Excel Requirements

Create abstraction for Excel reading.

The implementation may use:

- ClosedXML

The Application layer must only see:

- Excel DTO
- Request Models

---

# Configuration

Use strongly typed options:

```
SqlOptions

KeyVaultOptions

ServiceBusOptions

ExcelOptions
```

Configuration source:

appsettings.json

---

# Logging

Use:

Microsoft.Extensions.Logging

Console logging is sufficient.

Design should allow Serilog addition later.

---

# Validation

Use FluentValidation.

Each operation owns its validators.

Examples:

CenterOnboardingValidator

CenterOffboardingValidator

---

# Error Handling

Implement:

Result<T>

Expected validation failures should return Result failures.

Unexpected infrastructure failures may throw exceptions.

---

# Testing

Create:

```
ScaInterfaceOpsApp.Tests
```

Use:

- xUnit
- FluentAssertions
- Moq

Tests should cover:

- Center onboarding validation
- Center onboarding workflow decisions
- Center offboarding validation
- Center offboarding workflow decisions

External dependencies must be mocked.

No real Azure or SQL dependency required for unit tests.

---

# Coding Standards

Use:

- .NET 8
- Nullable enabled
- Implicit usings enabled
- File scoped namespaces
- Async methods
- Constructor dependency injection
- Records for immutable request models
- XML documentation
- Clean naming conventions

---

# Deliverables

Generate:

1. Solution file
2. Project files
3. Folder structure
4. Project references
5. Dependency injection setup
6. Configuration classes
7. Interfaces
8. Placeholder implementations
9. Sample Program.cs
10. Sample appsettings.json
11. Test project skeleton

Do not generate unnecessary business implementation.

Focus on a clean production-ready foundation.
