# Swift 5

---

#### Table of contents

- [Basics](#basics)
  - [var and let](#var-&-let)
    - [Printing stuff](#printing-stuff)
      - [Same line print](#print-without-linebreak)
      - [String interpolation](#string-interpolation)
  - [Comments](#comments)
  - [Semicolons](#semicolons)
  - [Integers](#integers)
    - [`Int`](#int)
  - [Floating-point numbers](#floating-point-numbers)
  - [Type safety and type inference](#type-safety-and-type-inference)
  - [Numeric literals](#numeric-literals)

---

### Basics

#### var & let

```swift
var x = 0.0, y = 0.0, z = 0.0 // You can declare multiple vars at once
var x, y, z: Double // Still ok
var name: String = "Steve"
name = "Jobs" //ok
```

```swift
let name = "Steve" //Recognises the string type
name = "Jobs" //Error
```

##### Printing stuff

```swift
var welcome = "Welcome!"
print(welcome) // Prints "Welcome!"
```

###### Print without linebreak

```swift
print(someValue, terminator: "")
```

###### String interpolation

```swift
var welcome = "Welcome!"
print("I'm saying \(welcome)") // Prints "I'm saying Welcome!"
```



#### Comments

```
// This is a comment.

/* This is also a comment
but is written over multiple lines. */

/* This is the start of the first multiline comment.
 /* This is the second, nested multiline comment. */
This is the end of the first multiline comment. */
```



#### Semicolons

The use of semicolons `;` is not required at every line end. It's required *only* when you want to write multiple statements on a single line.

```swift
let cat = "🐱"; print(cat)
// Prints "🐱"
```



#### Integers

- Signed 8-bit: `Int8`
- Unsigned 16-bit: `UInt16`
- .. 32
- ..64

##### `Int` and `UInt`

They are the same size Integer (signed or unsigned) of the current platform.
eg. (32bit platform => `Int32`/`UInt32`).

The use of the default `Int` is recommended. Use `UInt` <u>only when</u> you need an unsigned integer the same size as the platform.



#### Floating-Point Numbers

- `Double`: 64-bit floating-point number (min 15 decimal digits precision).
- `Float`: 32-bit floating-point number (min 6 decimal digits precision).

`Double` is preferred.



#### Type safety and type inference

Swift is a *type-safe* language. It also does a brilliant job at guessing a type (type inference) so
`let meaningOfLife = 42` is ok. When infering a floating-point type, Swift will always choose `Double`.



#### Numeric literals

- Decimal number: `17`
- Binary number: `0b10001`
- Octal number: `0o21`
- Hexadecimal number: `0x11`