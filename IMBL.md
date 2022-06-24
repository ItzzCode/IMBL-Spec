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
#from <std> import <stdio>

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
#from <std> import <stdio>

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
