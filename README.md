# RESET
Framework to provide an EventSourced basis for REST services.  Conventions allow easy discovery of entities, event types, and other niceities

#Basic Concepts
RESET follows Domain Driven Design principles

#Entities - any object that maintains state is an Entity.  An entity can be related to another object as a sub entity.  Entities which do not have a parent are known as AggregateRoots, and represent the main objects a system deals with.  An entity is the sum of all events that have been performed on that, and is identical to a Stream in EventSourcing terminology.

Each entity has  
- Id - unique identifier for the entity
- LastModified - datetime of when the last event of the entity
- Type - type of the entity (may be provided by class name)
- Description - describes what the entity does, and is listed through the API


#Value Objects - objects that do not maintain state (unit values, currencies, etc) are Value Objects


#Repositories - objects are stored in a repository, which abstracts out storage.  Objects should only be created through a repository


#Events - every change to an entity state is reflected in an event.  This allows recreation, auditing, and retroactive changes after business logic modifications or bug fixes.  Events are always targeted and handled by an Aggregate Root
Each event has
- EntityId - id of the entity this event should modify
- EventId - unique ID for the event
- UserId - user who generated the event, nullable
- Timestamp - when the event was generated, down to the clock tick
- Description - text of what the event does
- Type - type of the event (may be provided by class name)

#Standard endpoints
- /entitytypes - lists the entities supported by the sytem.  This lists only the AggregateRoots of the system
- /events - list all possible events in the system
- /events/entitytype - lists the events that an AggregateRoot can consume, and the fields of each 
- /{entitytype}/ - lists all entities of the specified type a user has access to
- /{entitytype}/{Id} - lists the individual item
- /{entitytype}/{Id}/events - provides an endpoint to POST events to the specified entity

- /metrics - Prometheus metrics page
- /swagger - provide Swagger documentation of the API

#Design Guidelines
- keep the number of aggregate roots small if possible, as events push towards them and this handles keeping streams intact.  For example, in an ecommerce system, an Order is a good aggregate root.  An OrderItem, while useful as a subitem, is not a good entity root because it constantly needs the context of an order.  In general, if you find yourself frequently using a group of items in code, it's a clear sign that you should define that group as an entity root instead.
