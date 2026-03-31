# Aggregates

*Customer*, *Location*, and *Carrier Movement* have their own identities, and are shared by many *Cargoes*, so they must be the roots of their own AGGREGATES, which contain their attribute and possibly other objects below the level of detail of this discussion.

## Customer Aggregate

*Customer* has its own identity (Customer ID Number) and is shared by many Cargoes. It is an AGGREGATE ROOT.

- **Aggregate Root:** Customer

## Location Aggregate

*Location* has its own identity (arbitrary, automatically generated identifier). It is shared by many Cargoes and Carrier Movements. It is an AGGREGATE ROOT.

- **Aggregate Root:** Location

## Carrier Movement Aggregate

*Carrier Movement* has its own identity (code from shipping schedule). It is an AGGREGATE ROOT.

- **Aggregate Root:** Carrier Movement

## Cargo Aggregate

The *Cargo* AGGREGATE sweeps in everything that would not exist but for a particular *Cargo*, which would include the *Delivery History*, the *Delivery Specification*, and the *Handling Events*.

*Delivery History* fits inside *Cargo's* boundary. No one would look up a *Delivery History* directly without wanting the *Cargo* itself. With no need for direct global access, and with an identity that is really just derived from the *Cargo*, the *Delivery History* fits nicely inside *Cargo's* boundary, and it does not need to be a root.

The *Delivery Specification* is a VALUE OBJECT, so there are no complications from including it in the *Cargo* AGGREGATE.

- **Aggregate Root:** Cargo
- **Contained:** Delivery History, Delivery Specification (Value Object)

## Handling Event Aggregate

The *Handling Event* should be the root of its own AGGREGATE. The activity for handling the Cargo has some meaning even when considered apart from the Cargo itself. It can be queried to find all the operations to load and prepare for a particular *Carrier Movement*.

- **Aggregate Root:** HandlingEvent
