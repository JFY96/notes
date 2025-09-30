# Onion Architecture with DDD and CQRS

# Domain Driven Design (DDD)

## Value objects

- Express a value
- Define allowed values
- Are Immutable (can't update once created)
- Are easy to test (no state change - very little setup, no external dependencies, just need to check can construct in certain situations)

- Object that represents a value in the domain, but also constrains the possible values that we can have in memory in our system
- Value objects are part of our strategy to make the incorrect inexpressible
- e.g. Name object - throw error if not valid

## Entities

- contains value objects, e.g. an attendee object may have a name, email
- Have identity
- Are more than their attributes
- Evolve over time
- Are slightly harder to test (can change, have dependencies on value objects)

- rules are checked by value objects, and the entities consistency is guaranteed by that

## Services

- Have no identity or attributes (are not a "thing")
- Tackle cross-concern operations
- Are harder to test (May contain multiple entities)

## Repositories

- Are a collection of (almost) all objects of a certain type (what you ask for when you want entities)

# CQRS

Command Query Responsibility Segregration

- Command is a method that mutates state
- Query is a method that returns state
- A method generally does not