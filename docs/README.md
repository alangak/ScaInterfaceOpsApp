# ScaInterfaceOpsApp

## Purpose

ScaInterfaceOpsApp is a .NET operational console application responsible for managing the lifecycle of SCA Interface Centers.

The application is designed with future extensibility in mind. While today it is implemented as a Console Application, the Application layer is independent of the host, allowing an ASP.NET API or other host to be added later without changing the business logic.

---

## Input

The application accepts a predefined Excel workbook.

The Excel workbook is the single source of truth for execution.

No API input or manual data entry is considered in the current scope.

---

## Current Operations

### Center Onboarding

Responsible for adding new centers or updating existing centers.

Tasks include:

- SQL Server Insert / Update
- Azure Key Vault Secret Create / Update
- Azure Service Bus Subscription Filter Append

The Service Bus Subscription already exists.

Only filters are appended or updated.

---

### Center Offboarding

Responsible for disabling an existing center.

Tasks include:

- Flag records inactive in SQL
- Delete selected SQL records
- Delete Azure Key Vault secrets
- Remove Azure Service Bus filters
- Remove Service Bus subscriptions when required

---

## Architecture

The solution follows a simplified Clean Architecture.

```
Console
      │
      ▼
Application
      │
      ▼
Contracts
      │
      ▼
Infrastructure
```

Business logic never references Azure SDKs or SQL implementations directly.

Infrastructure implements the contracts defined by the Application layer.

---

## Project Responsibilities

### ScaInterfaceOpsApp.Console

- Read Excel
- Configure Dependency Injection
- Read Configuration
- Invoke Operations
- Display Progress
- Exit Codes

---

### ScaInterfaceOpsApp.Application

Contains business operations.

Current Operations:

- CenterOnboarding
- CenterOffboarding

Contains no Azure SDK code.

Contains no SQL implementation.

---

### ScaInterfaceOpsApp.Infrastructure

Contains implementations for:

- SQL Server
- Azure Key Vault
- Azure Service Bus
- Excel Reader

---

### ScaInterfaceOpsApp.Common

Contains reusable models and utilities.

Examples:

- Options
- Helpers
- Extensions
- Constants

---

## Design Principles

- Single Responsibility
- Dependency Injection
- Interface-based Design
- Feature-Oriented Organization
- Separation of Business Logic and Infrastructure

---

## Future Extension

The Console Host can later be replaced or complemented by:

- ASP.NET Web API
- Azure Function
- Windows Service

without changing the Application layer.
