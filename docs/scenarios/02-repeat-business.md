# Sample Application Feature: Repeat Business

The users say that repeated bookings from the same *Customers* tend to be similar, so they want to use old *Cargoes* as prototypes for new ones. The application will allow them to find a *Cargo* in the REPOSITORY and then select a command to create a new *Cargo* based on the selected one. We'll design this using the PROTOTYPE pattern (Gamma et al. 1995).

*Cargo* is an ENTITY and is the root of an AGGREGATE. Therefore, it must be copied carefully; we need to consider what should happen to each object or attribute enclosed by its AGGREGATE boundary. Let's go over each one:

- *Delivery History:* We should create a new, empty one, because the history of the old one doesn't apply. This is the usual case with ENTITIES inside the AGGREGATE boundary.
- *Customer Roles:* We should copy the *Map* (or other collection) that holds the keyed references to *Customers*, including the keys, because they are likely to play the same roles in the new shipment. But we have to be careful *not* to copy the *Customer* objects themselves. We must end up with references to the same *Customer* objects as the old *Cargo* object referenced, because they are ENTITIES outside the AGGREGATE boundary.
- *Tracking ID:* We must provide a new *Tracking ID* from the same source as we would when creating a new *Cargo* from scratch.

Notice that we have copied everything inside the *Cargo* AGGREGATE boundary, we have made some modifications to the copy, but we have *affected nothing outside the AGGREGATE boundary* at all.
