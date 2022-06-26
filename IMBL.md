# IMBL

## Version 0.0.0 DRAFT

*This might change at any moment!*

IMBL is a strongly typed imperative language aiming for easy application making. It stands for Itzz Me\'s Basic Language, and it is pronounced (im-ble).

### Goals and Ideas

I want to make this an easy experience, but I also want to follow in the lead of mainstream langauges. While the language will not be OO (Object Oriented), I still need to think if I should have those constructs as an optional part of IMBL. I probably won\'t be able to do much other than specification, since I\'m not a good enough programmer to implement a programming language yet.

Here\'s a wackier idea, which is that IMBL is implemented on .NET, which honestly I\'m not sure how you\'d do that. ~~JVM?~~

### Examples

Hello, World!

```IMBL
#from <std> include <stdio>

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
#from <std> include <stdio>

namespace Program {
    null Main() {
        //readLine() returns nullable sting type
        //the variable must be nullable
        string? input = readLine();
        printLine(input);
    }
}
```

Calculator

```IMBL
#from <std> include <stdio>
#from <std> include <converts>

namespace Program {

    null Main() {
        int? input1 = toInt(readLine("First Input: "));
        int? input2 = toInt(readLine("Second Input: "));
        string? operator = readLine("Operator (+-*/): ");

        switch( operator ) {
            case( "+" ) {
                //formated string
                printLine(f"{input1} + {input2} = {input1 + input2}");
                break;
            }

            case( "-" ) {
                printLine(f"{input1} - {input2} = {input1 - input2}");
                break;
            }

            case( "*" ) {
                printLine(f"{input1} * {input2} = {input1 * input2}");
                break;
            }

            case( "/" ) {
                printLine(f"{input1} / {input2} = {input1 / input2}");
                break;
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

### Syntax and that

Take the hello world example from earlier.

```IMBL
#from <std> include <stdio>

namespace Program {
    null Main() {
        printLine("Hello, World!");
    }
}
```

First, we include the stdio namespace from std, which allows us to call `printLine()` directly. Then we define the Program namespace, which isn't nessesary, but can prevent name collisions. Then we define a `Main` function that is the programs entry point, and which returns null. Functions that return null don't need an explicit `return` expresion. Note how there are curly braces; these define the scope of the namspace and Main function. Finally we call `printLine` with the string arguement of `"Hello, World!"`. Note that this langauge requires a `;` at the ends of expressions and function calls.

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

        // ERROR! Line 5; Cannot redefine const marked variable "num1".
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
        // ERROR! Line 6; Cannot define 2 as another value. Did you mean `==`?
        // NUM1 = 1;

        // compiled as std::stdio::printLine(2);
        std::stdio::printLine(NUM1);
    }
}
```

### Flow Control

#### Conditionals

if & else

```IMBL
namespace Program {
    null Main() {
        bool orange = false;

        if( orange ) {
            std::stdio::printLine("Orange");
        } else {
            std::stdio::printLine("no orange!!!!!!");
        }
    }
}
```

switch

```IMBL
namespace Program {
    null Main() {
        string? input = std::stdio::readLine();

        // thing to match
        switch( input ) {

            // what to match the thing with
            case( "AAA!" ) {
                std::stdio::printLine("Stop playing CS:GO"); 
                
                // break so it doesnt fall through
                break;
            }

            case( "Hi" ) {
                std::stdio::printLine("Hi!");
                break;
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

```IMBL
namespace Program {
    null Main(){
        for( int i = 0; i < 10; i++; ) {
            std::stdio::printLine(i);
            //prints 0 to 10
        }
    }
}
```

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

### Graphics

Not real yet. Although here's some things I'm thinking of.

1. They should probably be in one namespace.
2. They should also be easy to use.

Here's a mock up, with a theoretical gfx lib.

```IMBL
#from <std> include <stdio>
#include <gfx>

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
                break;
            }

            case('a') {
                gfx::circle(
                    20, // Center X
                    20, // Center Y
                    10, // Radius
                    0xFF0000 // Color
                );
                break;
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
