# Architecture Design

## Overview

```mermaid
flowchart LR
    Customer(Customer<br/>customerId<br/>name)
    Cargo(Cargo<br/>trackingId)
    DeliveryHistory(DeliveryHistory)
    DeliverySpecification(DeliverySpecification<br/>arrivalTime)
    HandlingEvent(HandlingEvent<br/>completionTime<br/>type)
    CarrierMovement(CarrierMovement<br/>scheduleId)
    Location(Location<br/>portCode)

    Cargo -->|1..*| Customer
    Cargo -->|goal| DeliverySpecification
    Cargo -.-> DeliveryHistory
    HandlingEvent -->|1| Cargo
    DeliveryHistory -->|1..*| HandlingEvent
    DeliverySpecification -->|destination| Location
    CarrierMovement -->|from| Location
    CarrierMovement -->|to| Location
    HandlingEvent -->|0..1| CarrierMovement
```

## Basic Features

1. Track key handling of customer cargo
2. Book cargo in advance
3. Send invoices to customers automatically when the cargo reaches some point in its handling

## Applications

### Tracking Query

A *Tracking Query* that can access past and present handling of a particular *Cargo*

### Booking Application

A *Booking Application* that allows a new *Cargo* to be registered and prepares the system for it.

### Incident Logging Application

An *Incident Logging Application* that can record each handling of the *Cargo* (providing the information that is found by the *Tracking Query*).

## Entities and Value Objects

### Entities

#### Customer

A *Customer* object represents a person or a company, an entity in the usual sense of the word. The *Customer* object clearly has identity that matters to the user, so it is an ENTITY in the model. How to track it? Tax ID might be appropriate in some cases, but an international company could not use that. This question calls for consultation with a domain expert. We discuss the problem with a businessperson in the shipping company, and we discover that the company already has a customer database in which each *Customer* is assigned an ID Number at first sales contact. This ID is already used throughout the company; using the number in our software will establish continuity of identity between those systems. It will initially be a manual entry.

- **Type:** Entity
- **Identity:** Customer ID Number
- **Note:** Assigned at first sales contact from existing company database

#### Cargo

Two identical crates must be distinguishable, so *Cargo* objects are ENTITIES. In practice, all shipping companies assign tracking IDs to each piece of cargo. This ID will be automatically generated, visible to the user, and in this case, probably conveyed to the customer at booking time.

- **Type:** Entity
- **Identity:** Tracking ID
- **Note:** Automatically generated, visible to user, conveyed to customer at booking

#### Handling Event

We care about such individual incidents because they allow us to keep track of what is going on. They reflect real-world events, which are not usually interchangeable, so they are ENTITIES.

- **Type:** Entity
- **Identity:** Combination of Cargo ID, completion time, and type
- **Note:** Same Cargo cannot be both loaded and unloaded at the same time

#### Carrier Movement

Each *Carrier Movement* will be identified by a code obtained from a shipping schedule.

- **Type:** Entity
- **Identity:** Code from shipping schedule
- **Note:** Real-world events not usually interchangeable

#### Location

Two places with the same name are not the same. Latitude and longitude could provide a unique key, but probably not a very practical one, since those measurements are not of interest to most purposes of this system, and they would be fairly complicated. More likely the *Location* will be part of a geographical model of some kind that will relate places according to shipping lanes and other domain-specific concerns. So an arbitrary, internal, automatically generated identifier will suffice.

- **Type:** Entity
- **Identity:** Arbitrary, internal, automatically generated identifier

#### Delivery History

This is a tricky one. *Delivery Histories* are not interchangeable, so, they are ENTITIES. But a *Delivery History* has one-to-one relationship with its *Cargo*, so it doesn't really have an identity of its own. Its identity is borrowed from the *Cargo* that owns it. This will become clearer when we model the AGGREGATES.

- **Type:** Entity (identity borrowed from Cargo)
- **Identity:** Borrowed from associated Cargo
- **Note:** One-to-one relationship with Cargo

### Value Objects

#### Delivery Specification

Although it represents the goal of a *Cargo*, this abstraction does not depend on *Cargo*. It really expresses a hypothetical state of some *Delivery History*. We hope that the *Delivery History* attached to our *Cargo* will eventually satisfy the *Delivery Specification* attached to our *Cargo*. If we had two *Cargoes* going to the same place, they could share the same *Delivery Specification*, but they could not share the same *Delivery History*, even though the histories start out the same (empty), *Delivery Specifications* are VALUE OBJECTS.

- **Type:** Value Object
- **Note:** Does not depend on Cargo. Can be shared between Cargoes going to the same destination.

