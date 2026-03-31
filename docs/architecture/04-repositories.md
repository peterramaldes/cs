# Repositories

There are five ENTITIES in the design that are roots of AGGREGATES, so we can limit out consideration to these, since none of the other objects is allowed to have REPOSITORIES.

To decide which of these candidates should actually have a REPOSITORY, we must go back to the application requirements. In order to take a booking through the *Booking Application*, the user need to select *Customer(s)* playing the various roles (shipper, receiver, and so on). So we need a *Customer Repository*. We also need to find a *Location* to specify as the destination for the *Cargo*, so we create a *Location Repository*.

The *Activity Logging Application* needs to allow the user to look up the *Carrier Movement* that a *Cargo* is being loaded onto, so we need a *Carrier Movement Repository*. This user must also tell the system which *Cargo* has been loaded, so we need a *Cargo Repository*.

For now there is no *Handling Event Repository*, because we decided to implement the association with *Delivery History* as a collection in the first iteration, and we have no application requirement to find out what has been loaded onto a *Carrier Movement*. Either of these reasons could change; if they did, then we would add a REPOSITORY.

## Repository Methods

| Repository | Aggregate Root | Methods |
|------------|----------------|---------|
| CustomerRepository | Customer | findByCustomerId, findByName, findByCargoTrackingId |
| CargoRepository | Cargo | findByTrackingId, findByCustomerId |
| LocationRepository | Location | findByPortCode, findByCityName |
| CarrierMovementRepository | CarrierMovement | findByScheduleId, findByFromTo |
