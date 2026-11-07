# Modularisation

## What is a module?

- Modules exist to fulfill a purpose.
- They are a tool in the toolbox of your application.
- The more flexible in usage that tool is, the better
- Because modules have a purpose, it means they have a use-case, a reason to exist.
    -  Solid form and shape, allows us to separate it into a distinct concept
- While a module exists to fulfill a purpose, there are aspects of the wider world that it must interact iwth to accomplish that purpose, but which are not its own responsibility to fulfill
    - Each module therefore has certain expectations of 

In summary a module is
- A Purpose
- A boundary
- An Interface
- Connections

## Why modularise?

- The Purpose
    - Tells us exactly what we are trying to achieve
- The Boundary
    - Tells us exactly waht is within our world
- The Interface
    - Tells us exactly waht the outside world expects of us
- The Connections
    Tells us exactly waht we expect of the outside world

- The module itself can be viewed from an external vantage point, and from an internal vantage point
    - Both must be considered to get a full picture
    - External = Software Architect
    - Internal = Software Engineer

- Reduced cognitive load
- Information hiding
- Braod brush architecture
- Navigability

A specification defines functionality that is available, and it defines the behaviour in terms of inputs/outputs.

Defining a specification at the module level, allows for understanding and agreement on the interface - while allowing ignorance of the implementation.

The interface + specification forms the basis for verification

We can write a single set of verification tests and apply them to any implementation of that module.


In summary
- Makes the intangible physical and explicit
- Unifies Internal and External worlds (architect and engineer level)
- Filters irrelevant information depending on level 
- Purpose is fulfilled through specification
- Implementations verified through specification testing

Overall, everything pulls in the same direction to bring physicality to a non-trivial portion of the codebase. So that

## Summary
- The application combines modules to achieve a goal
- Modules must be clear in purpose and generic in usage
- A module is
    - A Purpose
    - A boundary
    - An Interface
    - Connections
- Make sure your application architecture is just a visual representation of your application layer
- Everyone should agree what concepts exist within the application - regardless of engineer or architect
- Operate at the correct level (application or module) and view it from both externally and internally
- 
