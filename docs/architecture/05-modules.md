# Modules

```mermaid
graph TB
    subgraph Customer
        Customer
    end

    subgraph Billing
        Invoice
    end

    subgraph Shipping
        Cargo
        HandlingEvent
        CarrierMovement
        Location
    end

    Customer --> Cargo
    Invoice --> Cargo
    HandlingEvent --> Cargo
    Cargo --> Location
    Cargo --> CarrierMovement
```
