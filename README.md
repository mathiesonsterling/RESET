# RESET
Framework to provide an EventSourced basis for REST services.  Conventions allow easy discovery of entities, event types, and other niceities

#Basic Concepts
RESET follows Domain Driven Design principles

#Entities - any object that maintains state is an Entity.  An entity can be related to another object as a sub entity.  Entities which do not have a parent are known as AggregateRoots, and represent the main objects a system deals with.  An entity is the sum of all events that have been performed on that, and is identical to a Stream in EventSourcing terminology.

Each entity has  
- Id - unique identifier for the entity
- LastModified - datetime of when the last event of the entity
#Value Objects - objects that do not maintain state (unit values, currencies, etc) are Value Objects
#Repositories - objects are stored in a repository, which abstracts out storage.  Objects should only be created through a repository
#Events - every change to an entity state is reflected in an event.  This allows recreation, auditing, and retroactive changes after business logic modifications or bug fixes.  Events are always targeted and handled by an Aggregate Root

#Standard endpoints
- /entitytypes - lists the entities supported by the sytem.  This lists only the AggregateRoots of the system, with subtypes listed below
- /events/entitytype - lists the events that an AggregateRoot can consume, and the fields of each 
- /{aggregateroot}/ - lists all aggregate roots a user has access to
- /{aggregateroot}/{Id} - lists the individual item
- /{aggregateroot}/{Id}/events - provides an endpoint to POST events to the specified entity
