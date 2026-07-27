s
# AI Implementation Prompt

You are a Senior .NET Solution Architect.

Generate a production-ready .NET 8 solution using Clean Architecture principles while keeping the design pragmatic and avoiding unnecessary abstractions.

## Solution Name

ScaInterfaceOpsApp

---

## Projects

Create four projects.

ScaInterfaceOpsApp.Console

ScaInterfaceOpsApp.Application

ScaInterfaceOpsApp.Infrastructure

ScaInterfaceOpsApp.Common

---

## Purpose

The application is a Console Application that automates operational tasks for SCA Interface Centers.

Input is always an Excel workbook.

Future API support should be possible without changing business logic.

---

## Current Operations

### Center Onboarding

Responsibilities:

- Read Center information from Excel
- Validate data
- Determine whether SQL records require Insert or Update
- Create or Update Azure Key Vault secrets
- Append SQL Filters to existing Azure Service Bus Topic Subscriptions
- Produce execution result

---

### Center Offboarding

Responsibilities:

- Mark SQL records inactive
- Delete selected SQL records
- Delete Azure Key Vault secrets
- Remove Service Bus Filters
- Remove Service Bus Subscription if required
- Produce execution result

---

## Input

Input source is ONLY Excel.

Create an abstraction for reading Excel.

The business operations must never directly reference EPPlus, ClosedXML, or any Excel library.

---

## Infrastructure Responsibilities

SQL Server

- Repository implementation
- Stored Procedure execution
- Query support

Azure Key Vault

- Get Secret
- Set Secret
- Delete Secret

Azure Service Bus

- Create SQL Filter
- Update SQL Filter
- Delete SQL Filter
- Delete Subscription

Excel

- Read Workbook
- Convert rows into request models

---

## Contracts

Create interfaces for:

ISqlRepository

IKeyVaultService

IServiceBusAdministration

IExcelReader

---

## Dependency Injection

Both Console and Infrastructure should expose Dependency Injection extension methods.

Program.cs should only:

- Build Configuration
- Configure DI
- Read Excel
- Execute Operations
- Log Results

---

## Logging

Use Microsoft.Extensions.Logging.

Support Console logging.

Design so Serilog can be added later without changing application code.

---

## Configuration

Use strongly typed Options pattern.

Examples:

SqlOptions

KeyVaultOptions

ServiceBusOptions

ExcelOptions

---

## Validation

Use FluentValidation.

Each operation owns its validators.

---

## Error Handling

Create a Result<T> pattern.

Avoid throwing exceptions for expected validation failures.

Only throw exceptions for unexpected infrastructure failures.

---

## Folder Organization

Organize Application by Operations.

Operations

    CenterOnboarding

    CenterOffboarding

Each operation contains:

Models

Validators

Contracts

Operation

---

## Coding Guidelines

- Async all the way
- Constructor Injection only
- No static helper classes except extensions
- SOLID principles
- Small focused classes
- XML documentation
- Nullable enabled
- Implicit usings enabled
- File-scoped namespaces
- Primary constructors where appropriate
- Use records for immutable request models
- Use readonly where possible

---

## Output Expected

Generate the complete solution structure.

Generate all interfaces.

Generate placeholder implementations.

Generate Dependency Injection.

Generate configuration classes.

Generate Program.cs.

Generate sample appsettings.json.

Generate project references.

Generate csproj files.

No business logic implementation is required.

Only production-quality skeleton code with placeholders and TODO comments.
