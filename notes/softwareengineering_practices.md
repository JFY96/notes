# Clean and Modular Code

*Production code*: Software running on production servers to handle live users and data of the intended audience. 

This is different from *production-quality code*, which describes code that meets expectations for production in reliability, efficiency, and other aspects. Ideally, all code in production meets these expectations, but this is not always the case.

*Clean code*: Code that is readable, simple, and concise. Clean production-quality code is crucial for collaboration and maintainability in software development.

*Modular* code: Code that is logically broken up into functions and modules. Modular production-quality code that makes your code more organized, efficient, and reusable.

*Module*: A file. Modules allow code to be reused by encapsulating them into files that can be imported into other files.

*Refactoring*: Restructuring your code to improve its internal structure without changing its external functionality. This gives you a chance to clean and modularize your program after you've got it working.

# Writing clean code

Use meaningful names:
- Be descriptive and imply type
	- e.g. prefix with `is` or `has` for booleans
	- e.g. verbs for functions and nouns for variables
- Be consistent but clearly differenciate
- Avoid abbreviations and single letters (exceptions include counters and common math variables like `x`, `y`)
- Be descriptive but only with relevant information
	- Too long names could have a bad effect

Use whitespace properly
- Consistent indentation
- Seperate sections with blank lines to keep code organised and readable
- Try to limit characters on each line

# Writing modular code

DRY principle (Don't Repeat Yourself)
- Generalize and consolidate repeated code in functions or loops

Abstract out logic to improve readability
- Abstract out code into a function to make it less repetitive, improve readability
- It is possible to over-engineer this and have way too many modules, so use suitable judgement

Minimize the number of entities (functions, classes, modules)
- Don't have an unnecessary amount of functions and modules, as it will make it hard to view implementation details for something

Functions should do one thing
- Each function should be focused on doing one thing only. Single Responsibility Principle.

Arbitrary variable names can be more effective in certain functions (e.g. functions that calculate a value based on input)

Try to use fewer than three arguments per function
- Not a hard rule but in many cases its more effective this way

# Efficient code

Optimizing code to be more efficient can mean making it:
- Execute faster
- Take up less space in memory/storage

# Documentation

Additional text or illustrated information that comes with or is embedded in the code of software.

Several types of documentation can be added at different levels of your program:
- Inline comments - line level
- Docstrings - module and function level
- Project documentation - project level

## Inline Comments

- Comments often document the major steps in complex code, so it is easier to follow and without completely understanding the code
	- Some may argue this is using comments to justify bad code and that refactoring may be needed instead
- Comments are valuable for explaining where code cannot
	- e.g. history on why a method was implemented a certain way
	- sometimes an unconventional approach may be applied because of some obscure external variable or side effects, and these things are difficult to explain with just code

## Documentation strings

- Pieces of documentation that explain the functionality of any function or module in your code
- Ideally, every function should always have a docstring
- one-line docstrings may be enough but if the function is complicated enough, add a  more thorough paragraph and list arguments (state types, purpose) and the output (function return).

## Project documentation

- Essential for getting others to understand why and how your code is relevant to them, whether potential users or your project or developers that may contribute to it.
- at a minimum, a README file should be written for each project. This should include:
	- explain what it does
	- list of dependencies
	- detailed instructions on how to use it

# Logging

Logging is the process of recording messages to describe events that have occurred while running your software.

Logging properly is valuable for events and especially errors that occur when running a program.

Log messages should be:
- professional and clear
- concise with normal capitalization
- have the appropriate level for logging
	- e.g. `Debug`, `Error`, `Info` level (this depends on the language etc)

# Code reviews

Benefits
- Catch errors
- Ensure readability
- Check standards are met
- Share knowledge among teams

Some questions to ask yourself when conducting a code review
- Is the code clean and modular?
- Is the code efficient?
- Is the documentation/comments effective?
- Is the code well tested?
- Is the logging effective?

Tips
- Use a code linter
- Explain issues and make suggestions
- Keep your comments objective
	- try to avoid "you" or "I"
- Provide code examples