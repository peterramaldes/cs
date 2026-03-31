# Sample Application Feature: Changing the Destination of a Cargo

Occasionally a *Customer* calls up and says, "Oh no! We said to send our cargo to Hackensack, but we really need it in Hoboken." We are here to serve, so the system is required to provide this change.

*Delivery Specification* is a VALUE OBJECT, so it would be simplest to just throw it away and get a new one, then use a setter method on *Cargo* to replace the old one with the new one.
