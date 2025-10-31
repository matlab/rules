# MATLAB Coding Standards

This file contains comprehensive coding standards for MATLAB development. Use these rules when generating MATLAB code and for reviewing code quality and standards compliance.

## License

Based on [MATLAB Coding Guidelines](https://github.com/mathworks/MATLAB-Coding-Guidelines) repository.

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

---

## Naming Guidelines

### General Naming Principles
- Use a common language, like English, for MATLAB identifiers when writing code that will be read or used by someone whose native language is different than your own
- Prefer precise and descriptive names for elements of a programming interface including functions, classes, and methods. Do not use short names for functions or methods unless the meaning is obvious
- Avoid the use of abbreviated words in the names for elements in a programming interface whenever possible. Use whole words instead. Only use abbreviations that are unambiguous, commonly used within an organization or domain, or easily determined from context
- Avoid naming variables, functions, and classes using the name of an existing function or class on the MATLAB path. Name collisions can lead to "shadowing" which may lead to unexpected or inconsistent behavior

### Variables
- Limit variable name length to <= 32 characters
- Prefer descriptive names for variables. Short variables names are permissible when the variable's meaning can be easily determined from the context in which it is used
- Short variable names are acceptable for:
  - Mathematical expressions
  - Short blocks of contiguous code
  - Temporary variables or iterators in a loop
  - Values widely known to users in a particular domain (Mathematics: phi = golden ratio, Physics: h = Planck's constant)
- Do not mix singular and plural forms for variables (e.g., point and points). Instead, consider using a suffix for pluralization
- Avoid negated variable names like "isNot" or "notFound"
- Use lowerCamelCase for descriptive variable names consisting of multiple words
- Leading uppercase can be used for short variable names such as common mathematical symbols

### Functions and Methods
- Limit the name length to <= 32 characters
- Name functions and methods using a verb or verb phrase to convey the action performed
- Alternatively, name functions and methods using a noun or noun phrase if the noun describes the thing being created
- Use the numeral "2" in the name of a conversion function (e.g., `struct2table`)
- Use the prefix "is" or "has" for a function whose primary output is a logical value
- Use complementary names for functions with complementary operations (e.g., start/stop, create/destroy)
- Use lowerCamelCase or lowercase for function names. For function names that combine multiple words, prefer lowerCamelCase
- Method names should be either a verb phrase or a noun phrase following the same principles as function names
- Use lowerCamelCase or lowercase for method names. For method names that combine multiple words, prefer lowerCamelCase

### Name-Value Arguments
- Use UpperCamelCase for the names in Name-Value arguments

### Classes
- If a class represents a thing, use a noun or noun phrase in the name (e.g., PrintServer)
- If a class implements a set of behaviors or capabilities that other classes can obtain via inheritance, such as a mixin class, use an adjective (e.g., Copyable)
- Do not put "class" in a class name
- Do not use special attributes of the class (e.g., Abstract) in the name
- Use UpperCamelCase for the names of classes defined in a namespace
- If the class is defined in the MATLAB global namespace, use the function name casing rules above

### Properties, Events, and Namespaces
- Use a noun or noun phrase for most property names
- Use a verb phrase if a property is a logical value that indicates whether the object does something, or can do something, or has something (e.g., HasOutputPort)
- Use UpperCamelCase for property names
- Use UpperCamelCase for event names
- Use short, lowercase names for namespaces
- Do not use the namespace name in the name of a function, class, enumeration, or inner namespace

---

## Statements and Expressions Guidelines

### General Statements
- Do not put multiple statements on a single line
- Avoid using literal values in expressions especially when those values appear in multiple places. Similarly, avoid using literal values in a function call. In both cases, use a variable instead
- Write floating point literals with a digit (e.g., "0") before the decimal point

### Variables and Data
- Avoid the use of global variables. Instead, pass variables as arguments to a function
- Minimize the use of persistent variables. Caching data as a persistent variable between function calls can be used to avoid reloading or recomputing a large amount of data on each function call
- Define all fields in a struct in a single block of code. Do not add or remove fields from an existing struct outside of the function in which it was created
- Use cell arrays only to store data of varying classes and/or sizes. Do not use cell arrays to store character vectors as text data. Use a string array instead

### Syntax and Operators
- Do not use command syntax in functions or methods. Use of command syntax should be limited to the command line or in scripts
- Use parentheses in mathematical and logical expressions to improve readability
- Do not use == or ~= to compare two floating point values
- Use the fileparts, fullfile, and filesep functions to create or parse filenames in a platform independent way

### Control Flow
- Limit nesting of loop and conditional statements to 5 levels
- Avoid incrementally changing the size of an array inside a loop. Whenever possible, pre-allocate the array immediately before the loop
- Do not modify a loop iterator inside a for loop
- Minimize the use of break, continue, and return inside a for or while loop. Use break and continue only when it makes the loop more concise or more readable
- When using if-else, put the usual case in the if part and the exceptional case in the else part
- A switch statement should always have an otherwise block. If the otherwise block is empty, include a comment explaining why no other cases can occur

### Function Calls
- Use empty parentheses when calling functions or class methods with no arguments. This will make it clear that a function is being used rather than a variable. Reasonable exceptions include certain common functions like pi, true, and false and certain graphics related functions like gcf and gca
- Use the tilde character (~) to ignore unused, leading outputs from a function
- Use Name=Value syntax (R2021a) when passing Name-Value arguments to a function

### Functions to Avoid
- Avoid the use of the eval function. The eval function can lead to unexpected code execution especially when using the function with untrusted user input
- Avoid the use of functions which manipulate a workspace outside the current context. The evalin and assignin functions should not be used as a replacement for function outputs. Variables in the base workspace should not be used as if they are global variables
- Minimize the use of cd, addpath, and rmpath to modify the current folder or the MATLAB search path within a function or method. If you must use these functions, reset the current folder and path before exiting the function

---

## Formatting Guidelines

### Whitespace
- Use spaces rather than tabs to add white space
- Use 4 spaces per indent level
- Do not add spaces immediately after an opening parenthesis, square bracket, or curly brace
- Do not add spaces immediately before a closing parenthesis, square bracket, or curly brace
- Put spaces after commas or semicolons except at the end of a line
- Do not put extra whitespace at the end of lines

### Operators
- Use one space on either side of the assignment (=) operator in an assignment statement
- Do not use spaces around = when using Name=Value syntax to specify optional arguments to a function
- Use one space on either side of the relational operators (<, <=, ==, ~=, >, >=)
- Use one space on either side of the logical (&, &&, |, ||) operators
- Do not use spaces around the colon operator or in the operands on either side of the colon operator
- Do not put spaces around the multiply, divide, or exponent operators (* .* / ./ \ .\ ^ .^)
- When an expression appears on the right-hand side of an assignment statement, use spaces around the plus and minus operators that operate on the main terms of that expression. Put no spaces around plus or minus in other places, such as within grouped terms, as argument to functions, or as indexing operands
- Do not use spaces after the unary operators +, -, or ~

### Blank Lines
- Use a single blank line to separate sections of code that perform distinct tasks or are logically related
- Use one blank line to separate local function declarations
- Use one blank line to separate method declarations in a classdef file
- Use one blank line to separate method blocks in a classdef file
- Use one blank line to separate property blocks in a classdef file
- Do not put extra blank lines at the top or bottom of a script, function, or classdef file

### Line Length and Breaking
- Code and comment lines should be <= 120 characters
- Split long lines to maximize readability. When breaking a long line, consider breaking the line after a comma, after a space, or at a binary operator

---

## Code Comments Guidelines

### General Comments
- Use a common language, like English, for comments in code that will be read or used by someone whose native language is different than your own
- Use at least one space after the comment symbol "%"
- Use "%%" to define a new section

### Function Documentation
- Place the function H1 line immediately after the function declaration and before the arguments block
- The H1 line should provide a brief description of what the function does
- Help text that follows the H1 line should provide the information the user needs to use the function including the syntax, a description of inputs and outputs, and any side effects

### Comment Placement
- Comments should be placed just before the code they are meant to explain
- Short comments can be placed at the end of a line

### Indentation
- Indent H1 and help lines at the beginning of a function using the same indent level as the function declaration
- Otherwise, indent comment lines at the same level as the lines of code that immediately follow

---

## Function Authoring Guidelines

### File Organization
- A function file name should be the same as the name of the top-level function
- End all functions with the end keyword
- A function used by only one other function or script should be written as a local function in the same file
- Keep local functions simple. If a function needs to be independently tested, put it in its own file

### Function Structure
- Use caution when changing MATLAB global or system state. Restore the state when a function or method exits. If the modified state is not reset to the original values, subsequent code may behave incorrectly
- Limit the use of nested functions. Nested functions can almost always be replaced by a local function
- Keep anonymous functions simple and readable. When possible, keep the definition and use of the anonymous function together in the code
- Do not repeat blocks of code in a function. Refactor those statements into a new function or local function

### Function Arguments
- Limit the number of input arguments in a function declaration to 6. Use name-value arguments for optional information. Multiple name-value arguments can be represented as a single argument in the function declaration
- Validate input arguments for functions that are intended to be part of an external, user-facing programming interface. Use an arguments block to do validation
- Avoid the use of varargin to handle name-value arguments. Instead use an arguments block with optional arguments and name/value pairs
- Write element-wise functions so that they work with any array shape. Outputs which correspond to an input of a particular shape should have the same shape

### Function Outputs
- Limit the number of output arguments in a function declaration to 4
- Do not change the meaning of an output when the number of outputs change
- Use commas to separate outputs in a function declaration

---

## Class Authoring Guidelines

### Class Files and Structure
- A classdef file name should be the same as the name of the class
- Prefer value classes to handle classes
- Use handle classes to represent an object whose state can change without changing its identity
- Consider using handle for classes that:
  - Represent physical or unique objects like hardware devices or files
  - Represent visible objects like graphics components
  - Define objects in a relational data structure like a list or a tree

### Class Attributes
- Use the Sealed class attribute if you do not intend people to use your class as a superclass. Only leave classes unsealed when the class is designed to be extended by others
- Avoid multiple property blocks with the same attributes unless they are used to logically group related class properties
- Avoid multiple method blocks with the same attributes unless they are used to logically group related class methods

### Properties
- Make property access as restrictive as necessary to support the needs of the user of the class. This can make it easier to evolve the design of the class over time. For example, only allow set access when a property need to be set by a user of the class
- Avoid using set methods purely for validation. Use property validation syntax instead. If you have a situation where the property needs to be validated and transformed it may be more efficient to use a set method
- Use dependent properties only when one or more of the following is true:
  - Its value is computed solely from the value of other properties
  - Compatibility mandates the property be available to users of the class even if the property is no longer used in the implementation
  - A property change needs to cause a side effect on other properties
- Otherwise use a non-dependent property

### Methods
- Validate input arguments for those methods that are intended to be part of an external, user-facing programming interface (public methods). Use an arguments block, introduced in R2019b, to do validation
- Avoid writing class constructors that return more than one argument. A class constructor must return a valid object or an array of objects of the class
- Make methods private or protected unless they are intended to be called by users of the class
- Avoid the use of get methods for non-dependent properties
- Use modular indexing when creating a class with custom indexing. Avoid overloading subsref and subsasgn whenever possible

---

## Error Handling Guidelines

### Code Quality
- Fix all Code Analyzer warnings before submitting code to source control or when making code available for use by others

### Error Messages
- Write error messages that provide specific information to help the user understand the issue and what to do about it
- Error messages should take one of three forms:
  - **Problem and solution form**: The first sentence of the message states the problem. The next sentence explains ways to fix it
  - **Solution form**: The error message is a statement of what the user could do or what must be true to fix the problem
  - **Problem form**: The error message is a statement of the problem. Used when it is not possible to state a specific solution to the problem

### Exception Handling
- Reset global state or settings to their original values when handling errors. Consider using a try-catch block or an onCleanup object to reset the state
- Use try-catch blocks for error handling or to process exceptional conditions
- Do not use try-catch for normal flow control
- Include a matching catch block for every try block. If a catch block is empty, include a comment explaining why no further processing is required
- Use the MException object when a catch block tries to recover from a specific error. Do not assume which error has occurred
- Avoid the use of throwAsCaller

---

## Additional Guidelines

### String Handling
- Use " instead of ' for strings, unless ' is absolutely required

### Property Access
- Use obj.PropertyName instead of get/set methods to access properties, unless it's too awkward when working with arrays of objects
