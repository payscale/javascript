# PayScale JavaScript Style Guide

This is a set of coding conventions and rules for use in JavaScript programming. It is heavily based on [Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html), by Douglas Crockford, along with the [Airbnb](https://github.com/airbnb/javascript) and [jQuery](http://contribute.jquery.org/style-guide/js/) JavaScript style guides.

The long-term value of software to an organization is in direct proportion to the quality of the codebase. Over its lifetime, a program will be handled by many pairs of hands and eyes. If a program is able to clearly communicate its structure and characteristics, it is less likely that it will break when modified in the never-too-distant future.

Code conventions can help in reducing the brittleness of programs.

All of our JavaScript code is sent directly to the public. It should always be of publication quality.

Neatness counts.

## Table of Contents

1. [JavaScript Files](#javascript-files)
1. [Line Length](#line-length)
1. [Comments](#comments)
1. [Comment Tokens](#comment-tokens)
1. [Quotes](#quotes)
1. [Variable Declarations](#variable-declarations)
1. [Naming Conventions](#naming-conventions)
1. [Function Declarations](#function-declarations)
1. [Properties](#properties)
1. [Statements](#statements)
1. [Equality](#equality)
1. [Spacing](#spacing)
1. [Objects](#objects)
1. [Arrays and Function Calls](#arrays-and-function-calls)
1. [Best Practices and Patterns](#best-practices-and-patterns)
1. [jQuery-Specific](#jquery-specific)
1. [Parting Words](#parting-words)
 
## JavaScript Files

JavaScript programs should be stored in and delivered as .js files. Filenames should use camel case `likeThisFilename.js`.

JavaScript code should not be embedded in HTML files unless the code is specific to a single session. Code in HTML adds significantly to pageweight with no opportunity for mitigation by caching and compression.

`<script src="filename.js">` tags should be placed as late in the body as possible. This reduces the effects of delays imposed by script loading on other page components. There is no need to use the language or type attributes. It is the server, not the script tag, that determines the MIME type.


## Indentation

The unit of indentation is four spaces. Use of tabs should be avoided because (as of this writing in the 21st Century) there still is not a standard for the placement of tabstops. The use of spaces can produce a larger filesize, but the size is not significant over local networks, and the difference is eliminated by minification.


## Line Length

Avoid lines longer than 80 characters. When a statement will not fit on a single line, it may be necessary to break it. Place the break after an operator, ideally after a comma. A break after an operator decreases the likelihood that a copy-paste error will be masked by semicolon insertion. The next line should be indented 8 spaces, or alternatively vertically align parameters if that is more readable.

When a statement is too long to fit on one line, line breaks must occur after an operator.

Bad:
```JavaScript
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c
    + ' is ' + (a + b + c);
``` 

Good:
```JavaScript
var html = '<p>The sum of ' + a + ' and ' + b + ' plus ' + c +
        ' is ' + (a + b + c);
```

Lines should be broken into logical groups if it improves readability, such as splitting each expression of a ternary operator onto its own line even if both will fit on a single line.
```JavaScript
var baz = firstCondition(foo) && secondCondition(bar) ?
        qux(foo, bar) :
        foo;
        
// Also good
myFunc(alpha, 
       beta,
       gamma,
       delta,
       epsilon);
```

## Comments

Be generous with comments. It is useful to leave information that will be read at a later time by people (possibly yourself) who will need to understand what you have done. The comments should be well-written and clear, just like the code they are annotating. An occasional nugget of humor might be appreciated. Frustrations and resentments will not.

It is important that comments be kept up-to-date. Erroneous comments can make programs even harder to read and understand.

Make comments meaningful. Focus on what is not immediately visible. Don't waste the reader's time with stuff like
```JavaScript
i = 0; // Set i to zero.
```

Generally use line comments `//`. Save block comments for formal documentation.

Comments start with a capital first letter, but don't require a period at the end, unless you're writing full sentences. There must be a single space between the comment token and the comment text.

Good:
```JavaScript
var alpha = 'beta'; // Quick comment here  

// We need an explicit "bar", because later in the code foo is checked.
var foo = 'bar';
 
// Even long comments that span
// multiple lines use the single
// line comment form.
```

### Comment Tokens

Avoid using TODO comments. It may not be clear whether the TODO work has been performed, or the code may have changed since the TODO was written, and it exposes issues within the code to the public. 

If a TODO really must be used, open a bug or work item with a description of the issue and include the id along with the TODO. If the TODO is part of a work-in-progress, include your alias so it is easy to search the code to find the remaining TODOs to finish.

Bad:
```JavaScript
// TODO: refactor this code
```

Acceptable:
```JavaScript
// TODO:PSP-1234 Some description
// TODO:alexd:2014-07-25 Another description
```


## Quotes
Use single quotes `''` for strings.
```JavaScript
var single = 'I am wrapped in single quotes';
```

Strings that require inner quoting must use single outside and double inside.
```JavaScript    
var html = '<div id="myId"></div>';
```


## Variable Declarations

All variables should be declared before used. JavaScript does not require this, but doing so makes the program easier to read and makes it easier to detect undeclared variables that may become implied globals. Implied global variables should never be used. Use of global variables should be minimized.

It is preferred that each variable be given its own line (and comment if necessary).
```JavaScript
var currentEntry; // currently selected table entry
var level;        // indentation level
var size;         // size of table
```


## Naming Conventions

Variable and function names should be full words, using camel case with a lowercase first letter. Names should be descriptive but not excessively so. Exceptions are allowed for iterators, such as the use of i to represent the index in a loop. 

Names should be formed from the 26 upper and lower case letters (A .. Z, a .. z), the 10 digits (0 .. 9), $, and _ (underscore).

Do not use _ (underscore) as the first or last character of a name. It is sometimes intended to indicate privacy, but it does not actually provide privacy.

Most variables and functions should start with a lower case letter.

Constructor functions that must be used with the new prefix should start with a capital letter. JavaScript issues neither a compile-time warning nor a run-time warning if a required new is omitted. Bad things can happen if new is not used, so the capitalization convention is the only defense we have.

Examples:
```JavaScript
functionNamesLikeThis, variableNamesLikeThis, ClassNamesLikeThis, 
EnumNamesLikeThis, CONSTANT_VALUES_LIKE_THIS
```

If the name contains an acronym or abbreviation, the first letter only will be capitalized. 

Bad:
```JavaScript   
elementID
userHTML
```    

Good:
```JavaScript   
elementId
userHtml
```

Global variables, if absolutely necessary, should be attached to the `window` object. This clearly indicates what is happening anyway.

Bad:
```JavaScript
myGlobalVariable = 1;
var myOtherGlobalVariable = 2;
```

Good:
```JavaScript
window.myGlobalVariable = 1;
window.myOtherGlobalVariable = 2;
```

## Function Declarations

All functions should be declared before they are used. Inner functions should follow the var statement. This helps make it clear what variables are included in its scope.

There should be no space between the name of a function and the `(` (left parenthesis) of its parameter list. There should be one space between the `)` (right parenthesis) and the `{` (left curly brace) that begins the statement body. The body itself is indented four spaces. The `}` (right curly brace) is aligned with the line containing the beginning of the declaration of the function.
```JavaScript
function outer(c, d) {
    var e = c * d;

    function inner(a, b) {
        return (e * a) + b;
    }

    return inner(0, 1);
}
```

This convention works well with JavaScript because in JavaScript, functions and object literals can be placed anywhere that an expression is allowed. It provides the best readability with inline functions and complex structures.
```JavaScript
function getElementsByClassName(className) {
    var results = [];
    walkTheDom(document.body, function (node) {
        var array;                // array of class names
        var ncn = node.className; // the node's classname

        // If the node has a class name, then split it into a list of simple names.
        // If any of them match the requested name, then append the node to the list of results.
        if (ncn && ncn.split(' ').indexOf(className) >= 0) {
            results.push(node);
        }
    });
    return results;
}
```

If a function literal is anonymous, there should be one space between the word function and the `(` (left parenthesis). If the space is omited, then it can appear that the function's name is function, which is an incorrect reading.
```JavaScript
div.onclick = function (e) {
    e.preventDefault();
};

that = {
    method: function () {
        return this.datum;
    },
    datum: 0
};
```    
Use of global functions should be minimized.

When a function is to be invoked immediately, the entire invocation expression should be wrapped in parens so that it is clear that the value being produced is the result of the function and not the function itself. (This technique is sometimes referred to as an [IIFE](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) (immediately invoked function execution)).
```JavaScript
var collection = (function () {
    var keys = [];
    var values = [];

    return {
        get: function (key) {
            var at = keys.indexOf(key);
            if (at >= 0) {
                return values[at];
            }
        },
        set: function (key, value) {
            var at = keys.indexOf(key);
            if (at < 0) {
                at = keys.length;
            }
            keys[at] = key;
            values[at] = value;
        },
        remove: function (key) {
            var at = keys.indexOf(key);
            if (at >= 0) {
                keys.splice(at, 1);
                values.splice(at, 1);
            }
        }
    };
}());
```

## Properties
Use dot notation when accessing properties.
```JavaScript
var luke = {
    jedi: true,
    age: 28
};

// bad
var isJedi = luke['jedi'];

// good
var isJedi = luke.jedi;
```

Use subscript notation `[]` when accessing properties with a variable.
```JavaScript
var luke = {
    jedi: true,
    age: 28
};

function getProp(prop) {
    return luke[prop];
}

var isJedi = getProp('jedi');
```


## Statements

### Simple Statements
Each line should contain at most one statement. Put a `;` (semicolon) at the end of every simple statement. Note that an assignment statement that is assigning a function literal or object literal is still an assignment statement and must end with a semicolon.


### Compound Statements

Compound statements are statements that contain lists of statements enclosed in `{ }` (curly braces).

- The enclosed statements should be indented four more spaces.
- The `{` (left curly brace) should be at the end of the line that begins the compound statement.
- The `}` (right curly brace) should begin a line and be indented to align with the beginning of the line containing the matching { (left curly brace).
- Braces should be used around **all** statements, even single statements, when they are part of a control structure, such as an if or for statement. This makes it easier to add statements without accidentally introducing bugs.

    Bad:
    ```JavaScript
    if (condition) statement;
    ```

    Good:
    ```JavaScript
    if (condition) {
        statement;
    }
    ```


### `return` Statement

A `return` statement with a value should not use ( ) (parentheses) around the value. The return value expression must start on the same line as the `return` keyword in order to avoid semicolon insertion.


### `if` Statement

The `if` class of statements should have the following form:
```JavaScript
if (condition) {
    statements
}

if (condition) {
    statements
} else {
    statements
}

if (condition) {
    statements
} else if (condition) {
    statements
} else {
    statements
}
```


### `for` Statement

A `for` class of statements should have the following form:
```JavaScript
for (initialization; condition; update) {
    statements
}

for (variable in object) {
    if (filter) {
        statements
    } 
}
```

The first form should be used with arrays and with loops of a predeterminable number of iterations.

The second form should be used with objects. Be aware that members that are added to the prototype of the object will be included in the enumeration. It is wise to program defensively by using the `hasOwnProperty` method to distinguish the true members of the object:
```JavaScript
for (variable in object) {
    if (object.hasOwnProperty(variable)) {
        statements
    } 
}
```


### `while` Statement

A `while` statement should have the following form:
```JavaScript
while (condition) {
    statements
}
```


### `do` Statement

A `do` statement should have the following form:
```JavaScript
do {
    statements
} while (condition);
```

Unlike the other compound statements, the `do` statement always ends with a `;` (semicolon).


### `switch` Statement

A `switch` statement should have the following form:
```JavaScript
switch (expression) {
    case expression:
        statements
    default:
        statements
}
```

Each group of statements (except the default) should end with `break`, `return`, or `throw`. Do not fall through.


### `try` Statement

The `try` class of statements should have the following form:
```JavaScript
try {
    statements
} catch (variable) {
    statements
}

try {
    statements
} catch (variable) {
    statements
} finally {
    statements
}
```


### `continue` Statement

Avoid use of the `continue` statement. It tends to obscure the control flow of the function.


### `with` Statement

The `with` statement should not be used.


## Equality

Use the `===` and `!==` identity operators. The `==` and `!=` equality operators do type coercion and should not be used.

Aside: What is type coercion? A brief example via [Stack Overflow](http://stackoverflow.com/questions/359494/does-it-matter-which-equals-operator-vs-i-use-in-javascript-comparisons?rq=1):
```
Using the == operator (Equality)
-------------------------------------------
true == 1; // true, because 'true' is converted to 1 and then compared
"2" == 2;  // true, because 2 is converted to "2" and then compared

Using the === operator (Identity)
-------------------------------------------
true === 1; // false
"2" === 2;  // false

This is because the equality operator == does type coercion, meaning that the interpreter
implicitly tries to convert the values before comparing.

On the other hand, the identity operator === does not do type coercion, and thus does not
convert the values when comparing.
```

The *only* exception is when checking for `undefined` and `null` by way of `null`.

```JavaScript
// Check for both undefined and null values, for some important reason.
undefOrNull == null;
```


## Spacing

Blank lines improve readability by setting off sections of code that are logically related.

Blank spaces should be used in the following circumstances:

- A keyword followed by ( (left parenthesis) should be separated by a space.

    ```JavaScript
    while (true) {
    ```
- A blank space should not be used between a function value and its ( (left parenthesis). This helps to distinguish between keywords and function invocations. Good: `add(1, 2);`
- All binary operators except . (period) and ( (left parenthesis) and [ (left bracket) should be separated from their operands by a space. Good: `a.b`, `a[b]`, `a + b`.
- No space should separate a unary operator and its operand except when the operator is a word such as typeof. Good: `i++`, `!condition`, `typeof someVar`.
- Each ; (semicolon) in the control part of a for statement should be followed with a space.
- Whitespace should follow every , (comma).
- The ? and : in a ternary conditional must have space on both sides. Good: `a ? b : c`.
- No filler spaces in empty constructs. Good: `{}`, `[]`, `fn()`.
- No leading commas.
 
    Bad:
    ```JavaScript
    if(condition) doSomething();
    while(!condition) iterating++;
    for(var i=0;i<100;i++) object[array[i]] = someFn(i);
    var leadingCommaHere = {
        a: 'b'
      , c: 'd'
    };  
    ```

    Good:
    ```JavaScript 
    if (condition) {
        doSomething();
    } else if (otherCondition) {
        somethingElse();
    } else {
        otherThing();
    }
     
    while (!condition) {
        iterating++;
    }
     
    for (var i = 0; i < 100; i++) {
        object[array[i]] = someFn(i);
    }

    var trailingCommaInstead = {
        a: 'b',
        c: 'd'
    };  
    ```

## Objects

- Use the literal syntax for object creation.

    Bad:
    ```JavaScript
    var item = new Object();
    ```

    Good:
    ```JavaScript
    var item = {};
    ```

Object declarations can be made on a single line if they are short (remember the line length limits). When an object declaration is too long to fit on one line, there must be one property per line. Property names only need to be quoted if they are reserved words or contain special characters:

- Bad:
    ```JavaScript
    var map = { ready: 9,
            when: 4, 'you are': 15 };
    ```
 
- Good:
    ```JavaScript
    var map = { ready: 9, when: 4, 'you are': 15 };
     
    // Good as well
    var map = {
        ready: 9,
        when: 4,
        'you are': 15
    };
    ```


## Arrays and Function Calls

- Use the literal syntax for array creation.

    Bad:
    ```JavaScript
    var items = new Array();
    ```

    Good:
    ```JavaScript
    var items = [];
    ```

- Never include extra spaces around elements and arguments:
    ```JavaScript   
    array = ['*'];

    array = [a, b];
     
    foo(arg);
     
    foo('string', object);
     
    foo(options, object[property]);
     
    foo(node, 'property', 2);

    // Function with a callback, object, or array as the sole argument:
    foo({
        a: 'alpha',
        b: 'beta'
    });
     
    // Function with a callback, object, or array as the first argument:
    foo(function() {
        // Do stuff
    }, options);
     
    // Function with a callback, object, or array as the last argument:
    foo(data, function() {
        // Do stuff
    });
    ```

## Best Practices and Patterns
### Enums
A simple approach to simulating an enum is to use an object literal to define a set of named constants. This is done as a convenience but note it does not get type safety or truly constant values.
```JavaScript
var Color = { red: 0, green: 1, blue: 2 };
var selectedColor = Color.red;
```


## jQuery-Specific
### Variables
Prefix jQuery object variables with a `$`. If the jQuery call returns anything else (such as a string or DOM node) this is not needed. 

Bad:
```JavaScript    
var sidebar = $('.sidebar');            // Returns a jQuery object
var $username = $('.username').text();  // Returns a string like "Esteban"
```

Good:
```JavaScript
var $sidebar = $('.sidebar');
var username = $('.username').text();
```


### Chained Method Calls
When a chain of method calls is too long to fit on one line, there must be one call per line, with the first call on a separate line from the object the methods are called on. If the method changes the context, an extra level of indentation must be used.
```JavaScript
elements
    .addClass('foo')
    .children()
        .html('hello')
    .end()
    .appendTo('body');
```


### Cache jQuery Lookups

Bad:
```JavaScript
var id = $('.sidebar').attr('id');
// ... stuff ...
$('.sidebar').hide();
```

Good:
```JavaScript
var $sidebar = $('.sidebar');
var id = $sidebar.attr('id'); 
// ... stuff ...
$sidebar.hide();
```


## Parting Words
Perhaps Google sums it up best:

>*BE CONSISTENT.*

>If you're editing code, take a few minutes to look at the code around you and determine its style. If they use spaces around all their arithmetic operators, you should too. If their comments have little boxes of hash marks around them, make your comments have little boxes of hash marks around them too.

>The point of having style guidelines is to have a common vocabulary of coding so people can concentrate on what you're saying rather than on how you're saying it. We present global style rules here so people know the vocabulary, but local style is also important. If code you add to a file looks drastically different from the existing code around it, it throws readers out of their rhythm when they go to read it. Avoid this.

## References
 - [Code Conventions for the JavaScript Programming Language](http://javascript.crockford.com/code.html)
 - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
 - [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
 - [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
