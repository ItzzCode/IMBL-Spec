# IMBL

## Version 0.0.1 DRAFT

*This might change at any moment!*

IMBL is a strongly typed imperative language aiming for easy application making. It stands for Itzz Me\'s Basic Language, and it is pronounced (im-ble).

Feel free to make a pull request to the spec, and make sure you explain why you changed what you did!

### Index of Contents

* Index of Contents
* Goals and Ideas
* Implementation
* Changelog
* Examples
* Types
* Syntax and that
* Variables
* Flow Control
  * Conditionals
  * Loops
* Errors
* Graphics
* Garbage Collection
* The Standard Library

### Goals and Ideas

I want to make this an easy experience, but I also want to follow in the lead of mainstream langauges. While the language will not be OO (Object Oriented), I still need to think if I should have those constructs as an optional part of IMBL. I probably won\'t be able to do much other than specification, since I\'m not a good enough programmer to implement a programming language yet.

### Implementation

I wanna use C# to implement it because it's what I know. I think ill get it hooked up to the LLVM backend if its compiled, but it can be interpreted initialy. Feel free to implement or to give tips on how to.

Thinking:

* Lexer
* Parser
* LLVM

### Changelog

0.0.1

* `break` no longer needed in switch cases
* include no longer formatted as if it were a preprocessor statement
* try catch added
* Cleaned up some stuff
* `any` type added

### Examples

Hello, World!

```IMBL
from <std> include <stdio>;

namespace Program {
    null Main() {
        printLine("Hello, World!");
    }
}
```

Alternatively,

```IMBL
null Main() {
    std::stdio::printLine("Hello, World!");
}
```

Cat

```IMBL
from <std> include <stdio>;

namespace Program {
    null Main() {
        // readLine() returns nullable sting type
        // the variable must be nullable
        string? input = readLine();
        printLine(input);
    }
}
```

Calculator

```IMBL
from <std> include <stdio>;

// I'm making it more explicit
// from <std> include <converts>;

namespace Program {

    null Main() {

        // check for errors, what if the user enters "among sus"?
        try {
            int? input1 = converts::toInt(readLine("First Input: "));
            int? input2 = converts::toInt(readLine("Second Input: "));
        }
        // what if there's an error?
        catch( ErrorBadConversion ) {
            // I'll just set them to zero, and warn the user
            printLine("One or more of your inputs is not a valid number.");
            int? input1 = 0;
            int? input2 = 0;
        }

        string? operator = readLine("Operator (+-*/): ");

        switch( operator ) {
            case( "+" ) {
                //formated string
                printLine(f"{input1} + {input2} = {input1 + input2}");
            }

            case( "-" ) {
                printLine(f"{input1} - {input2} = {input1 - input2}");
            }

            case( "*" ) {
                printLine(f"{input1} * {input2} = {input1 * input2}");
            }

            case( "/" ) {
                printLine(f"{input1} / {input2} = {input1 / input2}");
            }

            default {
                printLine("Not a supported operator");
            }
        }

        //heheheha
    }

}
```

### Types

* Signed Integers
  * int (Int32)
  * int8 - 8 Bit Signed Integer
  * int16 - 16 Bit Signed Integer
  * int32 - 32 Bit Signed Integer
  * int64 - 64 Bit Signed Integer
  * int128 - 128 Bit Signed Integer
* Unsigned Integers
  * uint8 / Byte - 8 Bit Unsigned Integer
  * uint16 - 16 Bit Unsigned Integer
  * uint32 - 32 Bit Unsigned Integer
  * uint64 - 64 Bit Unsigned Integer
  * uint128 - 128 Bit Unsigned Integer
* Text Types
  * string (utf_char array)
  * char (uint8)
  * utf16_char (A character encoded in UTF-16)
* Decimal Types
  * float
  * double
* Other
  * bool (true / false)
  * null
  * any
    * This is usually not a good idea.

### Syntax and that

Take the hello world example from earlier.

```IMBL
from <std> include <stdio>;

namespace Program {
    null Main() {
        printLine("Hello, World!");
    }
}
```

First, we include the stdio namespace from std, which allows us to call `printLine()` directly. Then we define the Program namespace, which isn't nessesary, but can prevent name collisions. Then we define a `Main` function that is the programs entry point, and which returns null. Functions that return null don't need an explicit `return` expresion. Note how there are curly braces; these define the scope of the namspace and Main function. Finally we call `printLine` with the string arguement of `"Hello, World!"`.

***Note that this langauge requires a `;` at the ends of expressions and function calls. The line will not end until you end it with a `;`.***

### Variables

You can declare a variable by typing its type, its name, and optionally its value. If you use a variable which has been declared, but has no value, you get an error.

```IMBL
namespace Program {
    null Main() {
        int num1 = 2;

        // Si
        num1 = 1;

        std::stdio::printLine(num1);
    }
}
```

You can also mark a variable with `const`. Now the variable cannot be changed, and uncommenting the line `num1 = 1` causes an error.

```IMBL
namespace Program {
    null Main() {
        const int num1 = 2;

        // ERROR! ErrorAttemptedConstantMutation, Line 5; Cannot redefine const marked variable "num1".
        // num1 = 1;

        std::stdio::printLine(num1);
    }
}
```

You can also use a preprocessor statement to make the variable constant. In this case, the constant is replaced before compile time. You should name the constant in full caps. Trying to change it does not work, as in the compiled code, you'd be trying to define a value as another.

```IMBL
#define NUM1 as 2

namespace Program {
    null Main() {
        // ERROR! ErrorValueDefinedAsAnother, Line 6; Cannot define 2 as another value. Did you mean `==`?
        // NUM1 = 1;

        // compiled as std::stdio::printLine(2);
        std::stdio::printLine(NUM1);
    }
}
```

While not recommended, you can do type inference with the `any` type. The variable must be declared or it will throw an error (ErrorVariableUndefined).

### Flow Control

#### Conditionals

An `if` statement can be used to choose certain code paths if a boolean condition is met. `elif` lets you make another comparasion if the first one was not met, and `else` is used when no condition matches.

```IMBL
namespace Program {
    null Main() {
        bool orange = false;

        if( orange ) {
            std::stdio::printLine("Orange");
        } elif( 1 == 2 ) {
            std::stdio::printLine("Huh?");
        } else {
            std::stdio::printLine("no orange!!!!!!");
        }
    }
}
```

A switch statement can be used to quickly compare a value to others, and to choose what path to take in that case.

```IMBL
namespace Program {
    null Main() {
        string? input = std::stdio::readLine();

        // thing to match
        switch( input ) {

            // what to match the thing with
            case( "AAA!" ) {
                std::stdio::printLine("Stop playing CS:GO"); 
                
                // break is not explicitly needed.
            }

            case( "Hi" ) {
                std::stdio::printLine("Hi!");
            }

            // what if nothing matched
            default {
                break;
            }
        
            // hahahahe
        }
    }
}
```

conditional operators

```IMBL
namespace Program {
    null Main() {
        bool tomato = true && false; // true and false: false
        tomato = true && true // true and true: true
        tomato = true || false // true or false: true
        tomato = !false // not false: true
        tomato = true == false: // true equal false: false
        tomato = true != false // true not equal false: true 
    }
}
```

#### Loops

The `for` loop takes in 3 expressions. The first expression is for initializing variables. The second is a condition, which if true continues the for loop. The final expression updates the variables.

```IMBL
namespace Program {
    null Main(){
        for( int i = 0, i < 10, i++ ) {
            std::stdio::printLine(i);
            //prints 0 to 10
        }
    }
}
```

The `while` loop takes in a condition, and while it's true, it keeps looping.

```IMBL
namespace Program {
    null Main() {
        string? input = "";

        while( input != "exit" ) {
            std::stdio::printLine("Running");
            input = std::stdio::readLine();
        }
    }
}
```

### Errors

Sometimes something goes wrong, and an error is thrown. An unhandled error will cause execution to stop, so make sure to remove the posibity of an error, or you can also use `try{}` to catch errors at runtime and let the program continue.

Here's the error syntax:

```sh
ERROR! [Error], Line [Number]; [Error Description].
```

Here's a list of errors that may happen.

* ErrorVariableUndefined
  * [Variable] has no value yet.
* ErrorVariableUndeclared
  * No variable is named [Variable] in this scope.
* ErrorValueDefinedAsAnother
  * Cannot define [Value] as another value. Did you mean `==`?
* ErrorAttemptedConstantMutation
  * Cannot redefine const marked variable [Variable].
* ErrorBadConversion
  * Cannot convert from [Type1] to [Type2].
* ErrorMismatchedTypes
  * Cannot use [Type1] on [Type2].

### Graphics

Not real yet. Although here's some things I'm thinking of.

1. They should probably be in one namespace.
2. They should also be easy to use.

Here's a mock up, with a theoretical gfx lib.

```IMBL
from <std> include <stdio>;
include <gfx>;

// gfx searches for funcs here
namespace gfxHandling {
    /* 
        leaving here
        if it is defined in gfxStart(), then it isnt accessible in gfxStep()
        if it is defined in gfxStep(), its gonna be declared repeatedly
    */
    char input = '';

    // Here's where you do things at the begining of graphics
    null gfxStart() {
        //heheheha
    }

    // This is called each step
    // While it returns true it keeps running
    bool gfxStep() {
        gfx::clearScreen(0xFFFFFF);

        // from stdio, returns character, does not pause code
        input = readKey();

        switch( input ) {
            case( 'e' ) {
                gfx::rectangle(
                    0, // Corner X
                    0, // Corner Y
                    100, // Size X
                    100, // Size Y
                    0x00FF00 // Color
                );
            }

            case('a') {
                gfx::circle(
                    20, // Center X
                    20, // Center Y
                    10, // Radius
                    0xFF0000 // Color
                );
            }

            //escape key
            case('\e') {
                return false;
            }

        }

        return true;
    }
}

namespace Program {
    null Main() {
        gfx::init(
            "IMBL Example Window", // Window name
            "icon.png", // Image to use as the icon
            500, // X resolution
            500, // Y resolution
            60 // update rate
        );
    }
}
```

### Garbage Colelction

Automatic. Variables are deleted at the end of the scope scope they are declared in. You can also delete a variable manually like this:

```IMBL
delete [variable]
```

Where `[Variable]` is the variable. Obviously.

### The Standard Library

Also known as `std` (Nailuj, not what you're thinking), this contains some useful functions to help programmers code easier.

`std` is stored in a single namspace, which itself also contains several other namespaces.

* `namespace stdio`
  * `null print( any input )`
    * Prints input to stdout
  * `null printLine( any input )`
    * Prints input with newline to stdout
  * `string? readLine( string prompt = "" )`
    * Reads from stdin, optionally displays a prompt.

* `namespace converts`
  * `string toString( any input )`
    * Converts any valid inputs to `string`. Throws  ErrorBadConversion if it cannot.
  * `string toString( any input )`
    * Converts any valid inputs to `char`. Throws  ErrorBadConversion if it cannot.
  * `int toInt( any input )`
    * Converts any valid inputs to `int`. Throws  ErrorBadConversion if it cannot (not an number, or too big/small).
  * `float toFloat( any input )`
    * Converts any valid inputs to `float`. Throws  ErrorBadConversion if it cannot (not an number, or too big/small).
  * `double toDouble( any input )`
    * Converts any valid inputs to `double`. Throws  ErrorBadConversion if it cannot (not an number, or too big/small).
