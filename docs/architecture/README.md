# Architecture Design

## Overview

DDD Chapter 7 - Cargo Shipping System

Domain: Shipping/Logistics
Purpose: Learning Domain-Driven Design through Cargo Shipping domain

## Basic Features

1. Track key handling of customer cargo
2. Book cargo in advance
3. Send invoices to customers automatically when the cargo reaches some point in its handling

## Domain Model Diagram

```mermaid
classDiagram
    class Customer {
        customerId
        name
    }

    class Cargo {
        trackingId
    }

    class DeliveryHistory {
    }

    class DeliverySpecification {
        arrivalTime
    }

    class HandlingEvent {
        completionTime
        type
    }

    class CarrierMovement {
        scheduleId
    }

    class Location {
        portCode
    }

    class CustomerRepository {
        +findByCustomerId(String)
        +findByName(String)
        +findByCargoTrackingId(String)
    }

    class CargoRepository {
        +findByTrackingId(String)
        +findByCustomerId(String)
    }

    class LocationRepository {
        +findByPortCode(String)
        +findByCityName(String)
    }

    class CarrierMovementRepository {
        +findByScheduleId(String)
        +findByFromTo(Location, Location)
    }

    %% Cargo Aggregate (blue)
    style Cargo fill:#cce5ff,stroke:#0000ff,stroke-width:2px
    style DeliveryHistory fill:#cce5ff,stroke:#0000ff,stroke-width:1px
    style DeliverySpecification fill:#cce5ff,stroke:#0000ff,stroke-width:1px

    %% Customer Aggregate (orange)
    style Customer fill:#ffe5cc,stroke:#ff9500,stroke-width:2px

    %% Location Aggregate (purple)
    style Location fill:#e5ccff,stroke:#9500ff,stroke-width:2px

    %% Carrier Movement Aggregate (yellow)
    style CarrierMovement fill:#ffffcc,stroke:#cccc00,stroke-width:2px

    %% Handling Event Aggregate (pink)
    style HandlingEvent fill:#ffe5f2,stroke:#ff0099,stroke-width:2px

    %% Repositories (green)
    style CustomerRepository fill:#ccffcc,stroke:#00cc00,stroke-width:2px
    style CargoRepository fill:#ccffcc,stroke:#00cc00,stroke-width:2px
    style LocationRepository fill:#ccffcc,stroke:#00cc00,stroke-width:2px
    style CarrierMovementRepository fill:#ccffcc,stroke:#00cc00,stroke-width:2px

    Cargo "1" --> "*" Customer
    Cargo --> DeliverySpecification : goal
    Cargo -- DeliveryHistory
    HandlingEvent "*" --> "1" Cargo
    DeliveryHistory "1" --> "*" HandlingEvent
    DeliverySpecification -- Location : destination
    CarrierMovement "1" --> "1" Location : from
    CarrierMovement "1" --> "1" Location : to
    HandlingEvent "*" --> "0..1" CarrierMovement
    CustomerRepository "1" --> "*" Customer
    CargoRepository "1" --> "*" Cargo
    LocationRepository "1" --> "*" Location
    CarrierMovementRepository "1" --> "*" CarrierMovement
```

## Sections

| # | Section | Description |
|---|---------|-------------|
| 01 | Applications | Tracking Query, Booking Application, Incident Logging Application |
| 02 | Entities and Value Objects | Domain entities and value objects with descriptions |
| 03 | Aggregates | Aggregate roots and their boundaries |
| 04 | Repositories | Repository selection and methods |
