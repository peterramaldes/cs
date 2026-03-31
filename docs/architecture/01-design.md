# Architecture Design

## Overview

DDD Chapter 7 - Cargo Shipping System

Domain: Shipping/Logistics
Purpose: Learning Domain-Driven Design through Cargo Shipping domain

## Basic Features

1. **Track key handling of customer cargo**
   - Monitor cargo status through its lifecycle

2. **Book cargo in advance**
   - Register new cargo before shipping

3. **Send invoices to customers automatically**
   - Trigger invoice generation when cargo reaches certain handling points

## Applications

### Tracking Query

- Access past and present handling of a particular Cargo
- Provides cargo status history

### Booking Application

- Register new Cargo in the system
- Prepare system for new cargo processing

### Incident Logging Application

- Record each handling of the Cargo
- Provide information for Tracking Query

## Entities and Value Objects

### Entities

#### Customer

- **Type:** Entity
- **Identity:** Customer ID Number (assigned at first sales contact, from existing company database)
- **Description:** Represents a person or company. Identity matters to users.
- **Note:** Tax ID not suitable for international companies. Domain expert consultation required.

#### Cargo

- **Type:** Entity
- **Identity:** Tracking ID (automatically generated, visible to user, conveyed to customer at booking)
- **Description:** Two identical crates must be distinguishable. All shipping companies assign tracking IDs.
- **Note:** ID will be automatically generated.

#### Handling Event

- **Type:** Entity
- **Identity:** Combination of Cargo ID, completion time, and type
- **Description:** Individual incidents tracking what is going on with cargo
- **Note:** Same Cargo cannot be both loaded and unloaded at the same time. Uniquely identified by (Cargo ID, completion time, type).

#### Carrier Movement

- **Type:** Entity
- **Identity:** Code obtained from shipping schedule
- **Description:** Real-world events not usually interchangeable
- **Note:** Identified by code from shipping schedule.

#### Location

- **Type:** Entity
- **Identity:** Arbitrary, internal, automatically generated identifier
- **Description:** Two places with the same name are not the same. Latitude/longitude not practical for most purposes.
- **Note:** Will be part of a geographical model relating places by shipping lanes and domain-specific concerns.

#### Delivery History

- **Type:** Entity (identity borrowed from Cargo)
- **Identity:** Borrowed from associated Cargo (one-to-one relationship)
- **Description:** Not interchangeable, but identity is tied to Cargo that owns it
- **Note:** Will become clearer when modeling AGGREGATES.

### Value Objects

#### Delivery Specification

- **Type:** Value Object
- **Description:** Represents the goal of a Cargo. Expresses a hypothetical state of some Delivery History.
- **Note:** Does not depend on Cargo. If two Cargoes go to the same place, they could share the same Delivery Specification, but not the same Delivery History (even though histories start out empty).

## Bounded Contexts

(TBD - to be defined together)

## Key Aggregates

(TBD - based on Entities analysis above)

## Technology Stack

- Java: 21 (LTS)
- Build: Maven 3.9.11
- Framework: Spring Boot 3.4.x
