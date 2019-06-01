# Writing maintainable (and beautiful) JavaScript code

Last modified 2019/06/01 by [No Dependencies](https://github.com/no-deps)

***Credits**: This article is based on and inspired by a 2012 [blog post](https://guh.me/writing-maintainable-and-beautiful-javascript-code) written by Gustavo Henrique. I think was a great article but it also deserved some updates and polishing as years went by and JavaScript evolved.*

***Sidenote**: This article is intended to be a living document that evoles over time, in contrast to most articles written that are merely a snapshot in time but get outdated very fast in our fast moving space. If you want to add or improve something, don't hesitate to create a pull request, I would be happy to consider any suggestions!*

No matter if you like JavaScript or not, these days it's hard to ignore that JavaScript rules the world and still enjoys a worse reputation than it acually deserves. Why? Because out there in the wild wild web there is so much straight out disgusting JavaScript code written by countless people who either don't care or don't know how to write it properly.

Is that JavaScript's fault because it is so forgiving when dealing with sloppy code? I would argue no, it just does everything in its power to run whatever is thrown at it, no matter how bad the code. As developers it is our responsibility (and job after all) to write code that stays maintanable for years and is enjoyable to read for others at the same time.

This article is based on my personal opinions and preferences for writing beautiful code and will most likely go against some (currently) established best-practices. So just read it, enjoy it and take from it whatever appeals to you. After all the world of code is not a dictatorship, different opinions are what can make us better developers if we embrace them!

## Apply correct indentation

Badly indented code is hard to read. Fact. I like using tabs/4 spaces for each indent level.

```
    // bad code
    input.keyup(function(e) {
    if (e.keyCode == 13) {
     submitQuery();
    }
    });
    
    // good code, properly indented
    input.keyup(function(e) {
        if (e.keyCode == 13) {
            submitQuery();
        }
    });
```

## Always use semicolons

JavaScript has a featured called ASI - Automatic Semicolon Insertion - which automatically inserts missing semicolons. It usually works in a predictable way, but sometimes it will break your program. Therefore, it’s better to code defensively and always use semicolons

```
    // broken code
    var foo = function() {
        return
            { 'bar' : 'baz' }
    }
    
    // which is parsed as
    var foo = function() {
        return;
            { 'bar' : 'baz' };
    };
```

## Don’t write long lines

Lengthy lines are harder to read, and they can lead to mistakes. I always try to wrap my code in 80 character lines, breaking the line when it reaches the limit, and indenting the next line two levels

```
    // bad, yet valid code
    var submitQuery = function() { keyword = input.val(); document.location.href = target + keyword; };
    
    // more readable code
    var submitQuery = function() {
        keyword = input.val();
        document.location.href = target + keyword;
    };
```

## Blank lines are for free

Use blank lines to separate related and unrelated code - it really makes the code more readable.

```
    // bad, at first glance you can't tell what the code does
    var checkboxes = $('input[name=message-check]:checked');
    var action = $(this).data('action');
    var ids = [];
    checkboxes.each(function () {
        ids.push($(this).data('id'));
    });
    $.post(document.location, {
        id: ids,
        action: action
    }, function(r) {
        document.location.reload();
    });
    
    // good, you can see clearly what the code does
    var checkboxes = $('input[name=message-check]:checked');
    var action = $(this).data('action');
    
    var ids = [];
    checkboxes.each(function () {
        ids.push($(this).data('id'));
    });
    
    $.post(document.location, {
        id: ids,
        action: action
    }, function(r) {
        document.location.reload();
    });
```

## Camelcase or underscores are both good

Name your variable and functions either with camel case names or with underscore separated names. The important thing is that you maintain the **only one style** in your program.

```
    // lowercase and underline
    var myaccountbalance = 10;
    var draw_cash_from_account = function() {
        ...
    };
    
    // camelcase
    var myAccountBalance = 10;
    var drawCashFromAccount = function() {
       ...
    };
```

## Constants should be constant

JavaScript has no real constants (no bitching, please), and the only way to differentiate them from regular variables is through style. The best way to define constant names is by using uppercase names separated by underlines.

```
    // bad, not readable nor different from regular variables
    var myconst = 3.14;
    
    // bad, regular variable style
    var myConst = 3.14;
    
    // bad, not readable
    var MYCONST = 3.14;
    
    // good
    var MY_CONST = 3.14;
```

## Strings

Strings should be circled by single quotes so you don’t need to escape double quotes.

```
    // escaping sucks
    var quote = "\"Better a diamond with a flaw than a pebble without.\"";
    
    // single quotes for da win :D
    var quote = '"Better a diamond with a flaw than a pebble without."';
```

## Numbers

Integers pose no threat to the code reader, but decimals can be tricky. That’s why you should always include digits before and after the decimal point.

```
    // is it 0.1 or the coder forgot something before the point?
    var dec = .1;
    
    // is it 1 or the coder forgot something after the point?
    var dec = 1.;
    
    // good, it leaves no room for doubts
    var dec = 1.0;
    var dec = 0.1;
```

## Object literals

Don’t use *new Object()*. It’s too verbose, and {} makes your code more compact and readable.

```
    // too verbose
    var myObj = new Object();
    
    // compact and readable
    var myObj = {};
```

## Array literals

Just like object literals, don’t use *new Array()*. \[\] does the same thing, is less verbose and makes your code more compact and readable.

```
    // too verbose
    var arr = new Array();
    
    // compact and readable
    var arr = [];
```

## Comments

Comments should be used when something is unclear, and must be indented according the commented block. Multiline comments should use the /\* \*/ delimiters.

```
    // Loads user settings
    settings = User.load_settings();
    
    /* Extracts API settings */
    api_settings = settings.ApiSettings;
    
    /**
     * Loads user friends via AJAX
     * @var int     User ID
     * @var string  Output type (JSON, XML)
     */
    function load_friends(user_id, output) {
    // ...
    };
```

## Statements and expressions

Braces should be used in **all** block statements - this includes one line blocks.

```
    // Good, more readable than when without braces
    if (User.has_wumpus()) {
        User.hunt();
    }
    
    // Good
    if (Wumpus.is_visible()) {
        User.show_options();
        Cave.describe();
    }
    
    // Bad
    while (Wumpus.is_alive()) Game.run();
    
    // Good
    while (Wumpus.is_alive())
    {
        Game.run();
    }
```

## Variables and functions

-   Enforce the language strict mode with “use strict”; - your code will be less error prone because the language standards will be enforced.
-   Declare all variables (when possible) at the top of a function, just like you’d do in C. Debugging and changing variables will be easier.
-   Declare functions before usage. JavaScript functions can be declared and used anywhere on the code, but declaring them before usage will make it easier for the reader to understand the logic of the program.
-   Always use the identity (=== and !==) operator instead of the equality (== and !=) operator to avoid type coercion.

## Avoid null comparisons

Do not use the identity/equality operators to find empty values, i.e., comparing with null. This is tricky and can break your programs.

```
    // Bad
    if (foo === null) {
    // ...
    }
    
    // Good
    if (typeof foo === 'undefined') {
    // ...
    }
    
    // Detecting reference values
    if (foo instanceof Foo) {
    // ...
    }
```

For now, that’s all. ;)