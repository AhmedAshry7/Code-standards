<!-- Heading -->
# PHP code Standards

## Introduction
PHP coding standards are guidelines and best practices for writing clean, readable, and maintainable PHP code. Adhering to these standards helps ensure consistency across different projects and teams, making it easier for developers to understand and work on the codebase.

## General Standards
### Opening and Closing PHP tags
When embedding multi-line PHP snippets within an HTML block, the PHP open and close tags must be on a line by themselves as the example here.
```php
function foo() {
    ?>
    <div>
        <?php
        echo esc_html(
            bar(
                $baz,
                $bat
            )
        );
        ?>
    </div>
    <?php
} 
```

This is for single line
```php
<input name="<?php echo esc_attr( $name ); ?>" />
```
### No Shorthand PHP Tags
<!-- strong -->
**Important:** You should always use full PHP tags not shorthand PHP start tags.
Correct:
```php
<?php ... ?>
<?php echo esc_html( $var ); ?>
```
Incorrect:
```php
<? ... ?>
<?= esc_html( $var ) ?>
```
### Single and Double Quotes
You should alternate your quating style so that if you are going to use single quotes in the string it self so wrap it in double quotes as this
```php
echo "<a href='{$escaped_link}'>text with a ' single quote</a>";
```

### Writing include/require statements
Because `include[_once]` and `require[_once]` are language constructs, they do not need parentheses around the path, so those shouldn’t be used. There should only be one space between the path and the include/require keywords.

It is strongly recommended to use `require[_once]` for unconditional includes. When using `include[_once]`, PHP will throw a warning when the file is not found but will continue execution, which will almost certainly lead to other errors/warnings/notices being thrown if your application depends on the file loaded, potentially leading to security leaks. 

## Naming
### Naming Conventions
You should use lowercase letters in variable, filter, and function names (never use camelCase). Separate words via underscores. Don’t abbreviate variable names unnecessarily so that the code can be unambiguous and self-documenting.

Example:
```php
function some_name( $some_variable ) {}
```

For function parameter names, it is strongly recommended to avoid reserved keywords as names, as it leads to hard to read and confusing code when using the PHP 8.0 “named parameters in function calls” feature.
Also keep in mind that renaming a function parameter should be considered a breaking change since PHP 8.0, so name function parameters with due care!

Class, trait, interface and enum names should use capitalized words separated by underscores. Any acronyms should be all upper case.

Example:

```php
class Walker_Category extends Walker {}
class WP_HTTP {}

interface Mailer_Interface {}
trait Forbid_Dynamic_Properties {}
enum Post_Status {}
```

Constants should be in all upper-case with underscores separating words.

Example:

```php
define( 'DOING_AJAX', true );
```

Files should be named descriptively using lowercase letters. Hyphens should separate words.

Example: `my-plugin-name.php`

Class file names should be based on the class name with class- prepended and the underscores in the class name replaced with hyphens.

Example: `class-wp-error.php`

This file-naming standard is for all current and new files with classes, except test classes. For files containing test classes, the file name should reflect the class name exactly, as per PSR4.

### Interpolation for Naming Dynamic Hooks
Dynamic hooks should be named using interpolation rather than concatenation for readability and discoverability purposes.

Dynamic hooks are hooks that include dynamic values in their tag name, e.g. `{$new_status}_{$post->post_type}`.

Variables used in hook tags should be wrapped in curly braces `{` and `}`, with the complete outer tag name wrapped in double quotes. This is to ensure PHP can correctly parse the given variables’ types within the interpolated string.

```php
do_action( "{$new_status}_{$post->post_type}", $post->ID, $post );
```

## Whitespace
### Space Usage
Always put spaces after commas, and on both sides of logical, arithmetic, comparison, string, the opening and closing parentheses of control structure blocks, and assignment operators.

```php
SOME_CONST === 23;
foo() && bar();
! $foo;
array( 1, 2, 3 );
$baz . '-5';
$term .= 'X';
if ( $object instanceof Post_Type_Interface ) {}
$result = 2 ** 3; // 8.
Put spaces on both sides of .

foreach ( $foo as $bar ) { ...
```

When defining a function, do it like so:

```php
function my_function( $param1 = 'foo', $param2 = 'bar' ) { ...
function my_other_function() { ...
```

When calling a function, do it like so:

```php
my_function( $param1, func_param( $param2 ) );
my_other_function();
```

When performing logical comparisons, do it like so:
```php
if ( ! $foo ) { ...
```

[Type casts](https://www.php.net/manual/en/language.types.type-juggling.php#language.types.typecasting) must be lowercase. Always prefer the short form of type casts, `(int)` instead of `(integer)` and `(bool)` rather than `(boolean)`. For float casts use `(float)`, not `(real)` which is deprecated in PHP 7.4, and removed in PHP 8:

Example:

```php
foreach ( (array) $foo as $bar ) { ...

$foo = (bool) $bar;
```

When referring to array items, only include a space around the index if it is a variable

Example:

```php
$x = $foo['bar']; // Correct.
$x = $foo[ 'bar' ]; // Incorrect.

$x = $foo[0]; // Correct.
$x = $foo[ 0 ]; // Incorrect.

$x = $foo[ $bar ]; // Correct.
$x = $foo[$bar]; // Incorrect.
```

In a switch block, there must be no space between the case condition and the colon.

Example:

```php
switch ( $foo ) {
    case 'bar': // Correct.
    case 'ba' : // Incorrect.
}
```

Unless otherwise specified, parentheses should have spaces inside them.

Example:

```php
if ( $foo && ( $bar || $baz ) ) { ...

my_function( ( $x - 1 ) * 5, $y );
```

When using increment (`++`) or decrement (`--`) operators, there should be no spaces between the operator and the variable it applies to.

Correct:
```php
for ( $i = 0; $i < 10; $i++ ) {}
```

Incorrect:
```php
for ( $i = 0; $i < 10; $i ++ ) {}
++   $b; // Multiple spaces.
```

### Indentation
Your indentation should always reflect logical structure. Use real tabs, not spaces, as this allows the most flexibility across clients.

Exception: if you have a block of code that would be more readable if things are aligned, use spaces.

Example:

```php
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

For associative arrays, each item should start on a new line when the array contains more than one item.

Example:

```php
$query = new WP_Query( array( 'ID' => 123 ) );
$args = array(
[tab]'post_type'   => 'page',
[tab]'post_author' => 123,
[tab]'post_status' => 'publish',
);

$query = new WP_Query( $args );
```

Note the comma after the last array item: this is recommended because it makes it easier to change the order of the array, and makes for cleaner diffs when new items are added.

Example:

```php
$my_array = array(
[tab]'foo'   => 'somevalue',
[tab]'foo2'  => 'somevalue2',
[tab]'foo3'  => 'somevalue3',
[tab]'foo34' => 'somevalue3',
);
```

For `switch` control structures, `case` statements should be indented one tab from the `switch` statement and the contents of the `case` should be indented one tab from the `case` condition statement.

Example:

```php
switch ( $type ) {
[tab]case 'foo':
[tab][tab]some_function();
[tab][tab]break;
[tab]case 'bar':
[tab][tab]some_function();
[tab][tab]break;
}
```

**Rule of thumb:** Tabs should be used at the beginning of the line for indentation, while spaces can be used mid-line for alignment.

### Remove Trailing Spaces
Remove trailing whitespace at the end of each line. Omitting the closing PHP tag at the end of a file is preferred. If you use the tag, make sure you remove trailing whitespace.

There should be no trailing blank lines at the end of a function body.

## Formatting
### Brace Style
Braces shall be used for all blocks in the style shown here:

```php
if ( condition ) {
    action1();
    action2();
} elseif ( condition2 && condition3 ) {
    action3();
    action4();
} else {
    defaultaction();
}
```

If you have a really long block, consider whether it can be broken into two or more shorter blocks, functions, or methods, to reduce complexity, improve ease of testing, and increase readability.

Braces should always be used, even when they are not required:

```php
if ( condition ) {
    action0();
}

if ( condition ) {
    action1();
} elseif ( condition2 ) {
    action2a();
    action2b();
}

foreach ( $items as $item ) {
    process_item( $item );
}
```

Note that requiring the use of braces means that single-statement inline control structures are prohibited. You are free to use the alternative syntax for control structures (e.g. if/endif, while/endwhile)—especially in templates where PHP code is embedded within HTML, for instance:

```php
<?php if ( have_posts() ) : ?>
    <div class="hfeed">
        <?php while ( have_posts() ) : the_post(); ?>
            <article id="<?php echo esc_attr( 'post-' . get_the_ID() ); ?>" class="<?php echo esc_attr( get_post_class() ); ?>">
                <!-- ... -->
            </article>
        <?php endwhile; ?>
    </div>
<?php endif; ?>
```

### Declaring Arrays
Using long array syntax `( array( 1, 2, 3 ) )` for declaring arrays is generally more readable than short array syntax `( [ 1, 2, 3 ] )`, particularly for those with vision difficulties. Additionally, it’s much more descriptive for beginners.

Arrays must be declared using long array syntax.

### Multiline Function Calls
When splitting a function call over multiple lines, each parameter must be on a separate line. Single line inline comments can take up their own line.

Each parameter must take up no more than a single line. Multi-line parameter values must be assigned to a variable and then that variable should be passed to the function call.

```php
$bar = array(
    'use_this' => true,
    'meta_key' => 'field_name',
);
$baz = sprintf(
    /* translators: %s: Friend's name */
    __( 'Hello, %s!', 'yourtextdomain' ),
    $friend_name
);

$a = foo(
    $bar,
    $baz,
    /* translators: %s: cat */
    sprintf( __( 'The best pet is a %s.' ), 'cat' )
);
```

### Type declarations
Type declarations must have exactly one space before and after the type. The nullability operator (`?`) is regarded as part of the type declaration and there should be no space between this operator and the actual type. Class/interface/enum name based type declarations should use the case of the class/interface/enum name as declared, while the keyword-based type declarations should be lowercased.

Return type declarations should have no space between the closing parenthesis of the function declaration and the colon starting a return type.

Correct:

```php
function foo( Class_Name $parameter, callable $callable, int $number_of_things = 0 ) {
    // Do something.
}

function bar(
    Interface_Name&Concrete_Class $param_a,
    string|int $param_b,
    callable $param_c = 'default_callable'
): User|false {
    // Do something.
}
```

Incorrect:

```php
function baz(Class_Name $param_a, String$param_b,      CALLABLE     $param_c )   :   ?   iterable   {
    // Do something.
}
```

The function signature of any function (method) which can be overloaded by plugins or themes should not be touched.
This leaves, for now, only unconditionally declared functions in the global namespace, private class methods, and code new to Core, as candidates for adding type declarations.

Note: Using the array keyword in type declarations is strongly discouraged for now, as most often, it would be better to use iterable to allow for more flexibility in the implementation and that keyword is not yet available for use in WordPress Core until the minimum requirements are raised to PHP 7.1.

### Magic constants
The [PHP native `__*__` magic constants](https://www.php.net/manual/en/language.constants.magic.php), like `__CLASS__` and `__DIR__`, should be written in uppercase when used.

When using the `::class` constant for class name resolution, the class keyword should be in lowercase and there should be no spaces around the `::` operator.

Correct:

```php
add_action( 'action_name', array( __CLASS__, 'method_name' ) );
add_action( 'action_name', array( My_Class::class, 'method_name' ) );
```

Incorrect:

```php
require_once __dIr__ . '/relative-path/file-name.php';
add_action( 'action_name', array( My_Class :: CLASS, 'method_name' ) );
```

### Spread operator …
When using the spread operator, there should be one space or a new line with the appropriate indentation before the spread operator. There should be no spaces between the spread operator and the variable/function call it applies to. When combining the spread operator with the reference operator (&), there should be no spaces between them.

Correct:
```php
function foo( &...$spread ) {
    bar( ...$spread );

    bar(
        array( ...$foo ),
        ...array_values( $keyed_array )
    );
}
```

Incorrect:

```php
function fool( &   ... $spread ) {
    bar(...
             $spread );

    bar(
        [...  $foo ],... array_values( $keyed_array )
    );
}
```

## Declare Statements, Namespace, and Import Statements
### Namespace declarations
Each part of a namespace name should consist of capitalized words separated by underscores.

Namespace declarations should have exactly one blank line before the declaration and at least one blank line after.

Correct:

```php
namespace Prefix\Admin\Domain_URL\Sub_Domain\Event;
```

There should be only one namespace declaration per file, and it should be at the top of the file. Namespace declarations using curly brace syntax are not allowed. Explicit global namespace declaration (namespace declaration without name) are also not allowed.

Incorrect namespace declaration using curly brace syntax:

```php
namespace Foo {
    // Code.
}
```

Incorrect namespace declaration for the global namespace:

```php
namespace {
    // Code.
}
```

### Using import use statements
Using import `use` statements allows you to refer to constants, functions, classes, interfaces, namespaces, enums and traits that live outside of the current namespace.

Import `use` statements should be at the top of the file and follow the (optional) `namespace` declaration. They should follow a specific order based on the type of the import:

1. `use` statements for namespaces, classes, interfaces, traits and enums
2. `use` statements for functions
3. `use` statements for constants
Aliases can be used to prevent name collisions (two classes in different namespaces using the same class name).
When using aliases, make sure the aliases follow the WordPress naming convention and are unique.

The following examples showcase the correct and incorrect usage of import `use` statements regarding things like spacing, groupings, leading backslashes, etc.

Correct:

```php
namespace Project_Name\Feature;

use Project_Name\Sub_Feature\Class_A;
use Project_Name\Sub_Feature\Class_C as Aliased_Class_C;
use Project_Name\Sub_Feature\{
    Class_D,
    Class_E as Aliased_Class_E,
}

use function Project_Name\Sub_Feature\function_a;
use function Project_Name\Sub_Feature\function_b as aliased_function;

use const Project_Name\Sub_Feature\CONSTANT_A;
use const Project_Name\Sub_Feature\CONSTANT_D as ALIASED_CONSTANT;

// Rest of the code.
```

Incorrect:

```php
namespace Project_Name\Feature;

use   const   Project_Name\Sub_Feature\CONSTANT_A; // Superfluous whitespace after the "use" and the "const" keywords.
use function Project_Name\Sub_Feature\function_a; // Function import after constant import.
use \Project_Name\Sub_Feature\Class_C as aliased_class_c; // Leading backslash shouldn't be used, alias doesn't comply with naming conventions.
use Project_Name\Sub_Feature\{Class_D, Class_E   as   Aliased_Class_E} // Extra spaces around the "as" keyword, incorrect whitespace use inside the brace opener and closer.
use Vendor\Package\{ function function_a, function function_b,
     Class_C,
        const CONSTANT_NAME}; // Combining different types of imports in one use statement, incorrect whitespace use within group use statement.

class Foo {
    // Code.
}

use const \Project_Name\Sub_Feature\CONSTANT_D as Aliased_constant; // Import after class definition, leading backslash, naming conventions violation.
use function Project_Name\Sub_Feature\function_b as Aliased_Function; // Import after class definition, naming conventions violation.

// Rest of the code.
```

## Refrences

[WordPress](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)
