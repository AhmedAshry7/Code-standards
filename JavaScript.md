<!-- Heading -->
# JavaScript code Standards

## Introduction
JavaScript coding standards are guidelines and best practices for writing clean, readable, and maintainable PHP code. Adhering to these standards helps ensure consistency across different projects and teams, making it easier for developers to understand and work on the codebase.

## Spacing
Use spaces liberally throughout your code. “When in doubt, space it out.”

These rules encourage liberal spacing for improved developer readability. The minification process creates a file that is optimized for browsers to read and process.

* Indentation with tabs.
* No whitespace at the end of line or on blank lines.
* Lines should usually be no longer than 80 characters, and should not exceed 100 (counting tabs as 4 spaces). This is a “soft” rule, but long lines generally indicate unreadable or disorganized code.
* `if`/`else`/`for`/`while`/`try` blocks should always use braces, and always go on multiple lines.
* Unary special-character operators (e.g., `++`, `--`) must not have space next to their operand.
* Any `,` and ; must not have preceding space.
* Any `;` used as a statement terminator must be at the end of the line.
* Any `:` after a property name in an object definition must not have preceding space.
* The `?` and `:` in a ternary conditional must have space on both sides.
* No filler spaces in empty constructs (e.g., `{}`, `[]`, `fn()`).
* There should be a new line at the end of each file.
* Any `!` negation operator should have a following space.
* All function bodies are indented by one tab, even if the entire file is wrapped in a closure.
* Spaces may align code within documentation blocks or within a line, but only tabs should be used at the start of a line.

Whitespace can easily accumulate at the end of a line – avoid this, as trailing whitespace is caught as an error in JSHint. One way to catch whitespace buildup is enabling visible whitespace characters within your text editor.

### Object Declarations
Object declarations can be made on a single line if they are short (remember the line length guidelines). When an object declaration is too long to fit on one line, there must be one property per line and each line ended by a comma. Property names only need to be quoted if they are reserved words or contain special characters:

Arrays can be declared on a single line if they are short (remember the line length guidelines). When an array is too long to fit on one line, each member must be placed on its own line and each line ended by a comma.

```js
// Preferred
var obj = {
    ready: 9,
    when: 4,
    'you are': 15,
};
var arr = [
    9,
    4,
    15,
];

// Acceptable for small objects and arrays
var obj = { ready: 9, when: 4, 'you are': 15 };
var arr = [ 9, 4, 15 ];

// Bad
var obj = { ready: 9,
    when: 4, 'you are': 15 };
var arr = [ 9,
    4, 15 ];
```

### Arrays and Function Calls
Always include extra spaces around elements and arguments:

```js
array = [ a, b ];

foo( arg );

foo( 'string', object );

foo( options, object[ property ] );

foo( node, 'property', 2 );

prop = object[ 'default' ];

firstArrayElement = arr[ 0 ];
```
Examples of Good Spacing:

```js
var i;

if ( condition ) {
    doSomething( 'with a string' );
} else if ( otherCondition ) {
    otherThing( {
        key: value,
        otherKey: otherValue
    } );
} else {
    somethingElse( true );
}

// Unlike jQuery, WordPress prefers a space after the ! negation operator.
// This is also done to conform to our PHP standards.
while ( ! condition ) {
    iterating++;
}

for ( i = 0; i < 100; i++ ) {
    object[ array[ i ] ] = someFn( i );
    $( '.container' ).val( array[ i ] );
}

try {
    // Expressions
} catch ( e ) {
    // Expressions
}
```

## Semicolons
Use them. Never rely on Automatic Semicolon Insertion (ASI).

## Indentation and Line Breaks
Indentation and line breaks add readability to complex statements.

Tabs should be used for indentation. Even if the entire file is contained in a closure (i.e., an immediately invoked function), the contents of that function should be indented by one tab:

```js
( function ( $ ) {
    // Expressions indented

    function doSomething() {
        // Expressions indented
    }
} )( jQuery );
```

### Blocks and Curly Braces
`if` , `else` , `for`, `while`, and try blocks should always use braces, and always go on multiple lines. The opening brace should be on the same line as the function definition, the conditional, or the loop. The closing brace should be on the line directly following the last statement of the block.

```js
var a, b, c;

if ( myFunction() ) {
    // Expressions
} else if ( ( a && b ) || c ) {
    // Expressions
} else {
    // Expressions
}
```

### Multi-line Statements
When a statement is too long to fit on one line, line breaks must occur after an operator.

Correct:
```js
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c +
    ' is ' + ( a + b + c ) + '</p>';
```

Incorrect:

```js
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c
    + ' is ' + ( a + b + c ) + '</p>';
```

Lines should be broken into logical groups if it improves readability, such as splitting each expression of a ternary operator onto its own line, even if both will fit on a single line.

Acceptable:

```js
var baz = ( true === conditionalStatement() ) ? 'thing 1' : 'thing 2';
```

Better:

```js
var baz = firstCondition( foo ) && secondCondition( bar ) ?
    qux( foo, bar ) :
    foo;
```

When a conditional is too long to fit on one line, each operand of a logical operator in the boolean expression must appear on its own line, indented one extra level from the opening and closing parentheses.

```js
if (
    firstCondition() &&
    secondCondition() &&
    thirdCondition()
) {
    doStuff();
}
```

### Chained Method Calls

When a chain of method calls is too long to fit on one line, there must be one call per line, with the first call on a separate line from the object the methods are called on. If the method changes the context, an extra level of indentation must be used.

```js
elements
    .addClass( 'foo' )
    .children()
        .html( 'hello' )
    .end()
    .appendTo( 'body' );
```

## Assignments and Globals

### Declaring Variables with const and let

For code written using ES2015 or newer, `const` and `let` should always be used in place of `var`. A declaration should use `const` unless its value will be reassigned, in which case `let` is appropriate.

Unlike `var`, it is not necessary to declare all variables at the top of a function. Instead, they are to be declared at the point at which they are first used.

### Declaring Variables With var
Each function should begin with a single comma-delimited `var` statement that declares any local variables necessary. If a function does not declare a variable using `var`, that variable can leak into an outer scope (which is frequently the global scope, a worst-case scenario), and can unwittingly refer to and modify that data.

Assignments within the `var` statement should be listed on individual lines, while declarations can be grouped on a single line. Any additional lines should be indented with an additional tab. Objects and functions that occupy more than a handful of lines should be assigned outside of the `var` statement, to avoid over-indentation.

Correct:

```js
var k, m, length,
    // Indent subsequent lines by one tab
    value = 'WordPress';
```

Incorrect:

```js
var foo = true;
var bar = false;
var a;
var b;
var c;
```

### Globals
In the past, WordPress core made heavier use of global variables. Since core JavaScript files are sometimes used within plugins, existing globals should not be removed.

All globals used within a file should be documented at the top of that file. Multiple globals can be comma-separated.

This example would make `passwordStrength` an allowed global variable within that file:

```js
/* global passwordStrength:true */
```

The “true” after `passwordStrength` means that this global is being defined within this file. If you are accessing a global which is defined elsewhere, omit :true to designate the global as read-only.

## Naming Conventions
Variable and function names should be full words, using camel case with a lowercase first letter.

Names should be descriptive, but not excessively so. Exceptions are allowed for iterators, such as the use of `i` to represent the index in a loop.

### Abbreviations and Acronyms
[Acronyms](https://en.wikipedia.org/wiki/Acronym) must be written with each of its composing letters capitalized. This is intended to reflect that each letter of the acronym is a proper word in its expanded form.

All other abbreviations must be written as camel case, with an initial capitalized letter followed by lowercase letters.

If an abbreviation or an acronym occurs at the start of a variable name, it must be written to respect the camelcase naming rules covering the first letter of a variable or class definition. For variable assignment, this means writing the abbreviation entirely as lowercase. For class definitions, its initial letter should be capitalized.

```js
// "Id" is an abbreviation of "Identifier":
const userId = 1;

// "DOM" is an acronym of "Document Object Model":
const currentDOMDocument = window.document;

// Acronyms and abbreviations at the start of a variable name are consistent
// with camelcase rules covering the first letter of a variable or class.
const domDocument = window.document;
class DOMDocument {}
class IdCollection {}
```

### Class Definitions

Constructors intended for use with new should have a capital first letter (UpperCamelCase).

A class definition must use the UpperCamelCase convention, regardless of whether it is intended to be used with new construction.

```js
class Earth {
    static addHuman( human ) {
        Earth.humans.push( human );
    }

    static getHumans() {
        return Earth.humans;
    }
}

Earth.humans = [];
```

### Constants
An exception to camel case is made for constant values which are never intended to be reassigned or mutated. Such variables must use the SCREAMING_SNAKE_CASE convention.

In almost all cases, a constant should be defined in the top-most scope of a file. It is important to note that JavaScript’s const assignment is conceptually more limited than what is implied here, where a value assigned by const in JavaScript can in-fact be mutated, and is only protected against reassignment. A constant as defined in these coding guidelines applies only to values which are expected to never change, and is a strategy for developers to communicate intent moreso than it is a technical restriction.

## Comments
Comments come before the code to which they refer, and should always be preceded by a blank line. Capitalize the first letter of the comment, and include a period at the end when writing full sentences. There must be a single space between the comment token (`//`) and the comment text.

```js
someStatement();

// Explanation of something complex on the next line
$( 'p' ).doSomething();

// This is a comment that is long enough to warrant being stretched
// over the span of multiple lines.
```

JSDoc comments should use the `/**` multi-line comment opening. Refer to the [JavaScript Documentation Standards](https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/#multi-line-comments) for more information.

Inline comments are allowed as an exception when used to annotate special arguments in formal parameter lists:

```js
function foo( types, selector, data, fn, /* INTERNAL */ one ) {
    // Do stuff
}
```

## Equality
Strict equality checks (`===`) must be used in favor of abstract equality checks (`==`).

## Type Checks
These are the preferred ways of checking the type of an object:

* String: `typeof object === 'string'`
* Number: `typeof object === 'number'`
* Boolean: `typeof object === 'boolean'`
* Object: `typeof object === 'object' or _.isObject( object )`
* Plain Object: `jQuery.isPlainObject( object )`
* Function: `_.isFunction( object ) or jQuery.isFunction( object )`
* Array: `_.isArray( object ) or jQuery.isArray( object )`
* Element: `object.nodeType or _.isElement( object )`
* null: `object === null`
* null or undefined: `object == null`
* undefined:
  * Global Variables: `typeof variable === 'undefined'`
  * Local Variables: `variable === undefined`
  * Properties: `object.prop === undefined`
  * Any of the above:` _.isUndefined( object )`

## Strings
Use single-quotes for string literals:

```js
var myStr = 'strings should be contained in single quotes';
```

When a string contains single quotes, they need to be escaped with a backslash (`\)`. Escape single quotes within strings:

```js
'Note the backslash before the \'single quotes\'';
```

## Switch Statements
The usage of `switch` statements is generally discouraged, but can be useful when there are a large number of cases – especially when multiple cases can be handled by the same block, or fall-through logic (the default case) can be leveraged.

When using `switch` statements:

* Use a `break` for each case other than `default`. When allowing statements to “fall through,” note that explicitly.
* Indent case statements one tab within the `switch`.

```js
switch ( event.keyCode ) {
    // ENTER and SPACE both trigger x()
    case $.ui.keyCode.ENTER:
    case $.ui.keyCode.SPACE:
        x();
        break;
    case $.ui.keyCode.ESCAPE:
        y();
        break;
    default:
        z();
}
```

It is not recommended to return a value from within a switch statement: use the case blocks to set values, then return those values at the end.

```js
function getKeyCode( keyCode ) {
    var result;

    switch ( event.keyCode ) {
        case $.ui.keyCode.ENTER:
        case $.ui.keyCode.SPACE:
            result = 'commit';
            break;
        case $.ui.keyCode.ESCAPE:
            result = 'exit';
            break;
        default:
            result = 'default';
    }

    return result;
}
```

## Best Practices
### Arrays
Creating arrays in JavaScript should be done using the shorthand `[]` constructor rather than the new `Array()` notation.

```js
var myArray = [];
```

You can initialize an array during construction:

```js
var myArray = [ 1, 'WordPress', 2, 'Blog' ];
```

In JavaScript, associative arrays are defined as objects.

### Objects
There are many ways to create objects in JavaScript. Object literal notation, `{}`, is both the most performant, and also the easiest to read.

```js
var myObj = {};
```

Object literal notation should be used unless the object requires a specific prototype, in which case the object should be created by calling a constructor function with `new`.

```js
var myObj = new ConstructorMethod();
```

Object properties should be accessed via dot notation, unless the key is a variable or a string that would not be a valid identifier:

```js
prop = object.propertyName;
prop = object[ variableKey ];
prop = object['key-with-hyphens'];
```

### Iteration
When iterating over a large collection using a `for` loop, it is recommended to store the loop’s max value as a variable rather than re-computing the maximum every time:

Correct:

```js
var i, max;

// getItemCount() gets called once
for ( i = 0, max = getItemCount(); i < max; i++ ) {
    // Do stuff
}
```
Incorrect:

```js
// getItemCount() gets called every time
for ( i = 0; i < getItemCount(); i++ ) {
    // Do stuff
}
```
### Underscore.js Collection Functions
Learn and understand Underscore’s collection and array methods. These functions, including `_.each`, `_.map`, and `_.reduce`, allow for efficient, readable transformations of large data sets.

Underscore also permits jQuery-style chaining with regular JavaScript objects:

```js
var obj = {
    first: 'thing 1',
    second: 'thing 2',
    third: 'lox'
};

var arr = _.chain( obj )
    .keys()
    .map( function ( key ) {
        return key + ' comes ' + obj[ key ];
    } )
    // Exit the chain
    .value();

// arr === [ 'first comes thing 1', 'second comes thing 2', 'third comes lox' ]
```

### Iterating Over jQuery Collections
The only time jQuery should be used for iteration is when iterating over a collection of jQuery objects:

```js
$tabs.each( function ( index, element ) {
    var $element = $( element );

    // Do stuff to $element
} );
```

Never use jQuery to iterate over raw data or vanilla JavaScript objects.

## Refrences

[WordPress](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)