## Features

* Custom delimiters
    * Defaults
        * `{{` `}}` to output an expression
        * `{%` `%}` to execute a statement
        * `{#` `#}` to add a comment
* Loading templates
* Rendering templates
* Error handling
* Template inheritance
* Expressions
    * Assignment (e.g. `a = 1`, `b = "some text"`, etc)
    * Binary (e.g. `a + b`, `a > b`, etc)
    * Grouping (e.g. `(a + b) * c`)
    * Literal (e.g. `1`, `2.0`, `"some text"`, `true`, `false`, etc)
    * Logical (e.g. `&&` and `||`)
    * Range (e.g. `0..<3` and `1...4`)
    * Ternary (e.g. `a > b ? "a is greater" : "a is not greater"`)
    * Unary (e.g. `!a` and `-b`)
    * Variable (e.g. `var a = 1`, `var b = 2.0`, `var c = "some text"`, etc)
* Statements
    * `block`
    * `extend`
    * `for` loop
    * `if`, `elseif`, and `else` conditions
    * `include`
    * `super`
    * `while` loop
* Memory caching with `NSCache`
