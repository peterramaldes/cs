# Modules

```mermaid
classDiagram
    namespace Customer {
        class Customer
        class Contact
        class CustomerAgreement
    }
    namespace Billing {
        class Invoice
        class Money
        class Currency
        class PricingModel
    }
    namespace Shipping {
        class Cargo
        class RouteSpecification
        class Itinerary
        class Leg
        class Location
        class TransportSchedule
        class Router
        class HandlingStep
        class Equipment
        class EquipmentInventory
        class BillOfLading
    }

    Customer --> CustomerAgreement
    Customer --> Contact
    Invoice --> Money
    Money --> Currency
    Cargo --> RouteSpecification
    RouteSpecification ..> Itinerary : "0..1"
    Itinerary --> Leg
    Leg --> Location
    TransportSchedule --> Leg
    Router --> Leg
    Itinerary --> HandlingStep
    HandlingStep --> Equipment
    Equipment --> EquipmentInventory
    Cargo --> Itinerary
    Cargo --> BillOfLading
    CustomerAgreement --> Billing.PricingModel
    CustomerAgreement --> Shipping.RouteSpecification : "may constrain"
```
