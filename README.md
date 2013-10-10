CoffeePHP
==================

CoffeePHP is a language makes your life easier,
it let you write less code to compile a workable PHP code.

CoffeePHP compiler is written in Go, Go is faster 10+ times than PHP, so the compilation
will be fast enough, and of course you can compile sources concurrently.

CoffeePHP takes the good stuffs from Haskell, Perl6, Go and CoffeeScript, so you can write
shorter code to generate the expected PHP code. e.g. To define a function, we use:

    getName :: -> "John"

Which is shorter than writing:

    function getName() {
        return "John";
    }

CoffeePHP aims to provide a simple optimizer to do optimizations like "dead
code elimination".. or so on.

So if you have framework with development/production, this dead code elimination 
could improve the overhead of mode checking for your production site.

For more details, please check the `docs` for the language synopsis.


Build
---------
Simply run make to produce the parser:

    git clone http://github.com/c9s/coffeephp
    cd coffeephp
    source goenv
    make


Current State
-------------
Not ready for production. we're still in alpha stage. but we'd like to recevie
comments, patches and feature requests.


Synopsis
---------
For more language details, please check the `docs` for the language synopsis.

    class Person
        say :: (name) -> "Hello #{name}, Good morning"
        getPhone :: () -> "12345678"
        
The above code compiles to:

```php
class Person {
    function say($name) {
        return "Hello " . $name . ', Good morning';
    }
    function getPhone() {
        return "12345678";
    }
}
```


Implementation
---------------
CoffeePHP uses Go yacc parser generator to produce a LALR(1) parser. 

To add new syntax, please checkout the grammar.y file, which is located in
`src/coffeephp/grammer.y`

The lexer uses concurrent strategy to parse tokens, the state machine is in
another gorouting, the parser can receive the tokens from lexer through the
channel concurrently.

CodeGen is expected to be a separated tree structure traverser, then code
generation which is not implemented inside the node structure like the
coffee-script codegen. So the code generator can be PHP code generator, LLVM
bit-code generator or JavaScript generator.

Basic optimization is on the roadmap. Optimizer uses tree-pattern matching
strategy to traverse the IR structure. 2 basic optimizations are in the plan --
constant folding and dead code elimination.


Lexer
------

    | Token Type          | Remark
--- | ------------------- | -------------
 x  | `T_IDENTIFIER     ` | 
 x  | `T_FLOATING       ` |
 x  | `T_NUMBER         ` |
 x  | `T_NEWLINE        ` |
 x  | `T_EOF            ` |
 x  | `T_ONELINE_COMMENT` |
 x  | `T_COMMENT        ` |
 x  | `T_IF             ` |
 x  | `T_ELSEIF         ` |
 x  | `T_CLASS          ` |
 x  | `T_FOR            ` |
 x  | `T_FOREACH        ` | 
 x  | `T_STRING         ` |  "String", and 'string'
 x  | `T_ECHO           ` |  echo "output"
 x  | `T_DOES           ` |  class Gate does DoorInterface

Parser
---------




Author
-------------

Yo-An Lin (yoanlin93@gmail.com)

