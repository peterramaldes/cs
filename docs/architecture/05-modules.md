# Modules

```mermaid
classDiagram

%% ======================
%% Customer Module
%% ======================
namespace Customer {
  class Customer
  class Contact
  class CustomerAgreement

  Customer --> Contact
  Customer --> CustomerAgreement
}

%% ======================
%% Billing Module
%% ======================
namespace Billing {
  class Invoice
  class Money
  class Currency
  class PricingModel

  Invoice --> Money
  Money --> Currency
}

%% ======================
%% Shipping Module
%% ======================
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

  Cargo --> RouteSpecification
  RouteSpecification --> Itinerary : "0..1"
  Itinerary --> "*" Leg
  Leg --> Location : from/to

  TransportSchedule --> "*" Leg
  Router --> "*" Leg

  Itinerary --> "*" HandlingStep
  HandlingStep --> Equipment
  Equipment --> EquipmentInventory

  Cargo --> Itinerary
  Cargo --> BillOfLading
}

%% ======================
%% Cross-module relations
%% ======================
Customer.CustomerAgreement --> Billing.PricingModel
Customer.CustomerAgreement --> Shipping.RouteSpecification : "may constrain"
```
