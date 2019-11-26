# Swift 3

- [Xcode tips](#xcode-tips)
  - [Documentation](#Documentation)
  - New Class
  - [UI + code](#combining-ui-and-code)
    - [Declaring button function](#declaring-button-function)
    - [Calling button function](#calling-button-function)
  - Autolayout
- [Swift language](#swift-language)
  - [var and let](#var-&-let)
    - Access Control
    - Memory Management
    - [Computed Properties](#computed-properties)
    - [didSet](#didset)
    - Arrays
    - Dictionaries
    - Tuples
    - Enums
    - Optionals
    - Type conversion
  - Conditionals
  - Loops
    - Range
  - Classes vs Structs
    - Extensions
  - Assertions

## Xcode tips

### Documentation

`⌥ + clic` on elements to get the documentation.

### Combining UI and code

`⌘+⌥+⏎` Open assistant editor to show both.

`ctrl` pressed: drag a UI element to my code area.

*Action* is a *Method*:
	Name: Method's name.
	Type: UIButton.
	Arguments: Sender (if I'm using it).

*Outlet* is and *Instance Variable*:
	Name: Variable's name.

*Outlet Collection* is an *Array of things*:
	Name: Array's name.

#### Declaring button function

```swift
func funcName(outParamName inName: Type, on button: UIButton) {}
```

#### Calling button function

```swift
funcName(outParamName: localParam, on: localButtonName)
```

I can use the same name for both outside and inside parameter name, but it's not recommended.

## Swift Language

###var & let

```swift
var name: String = "Steve"
name = "Jobs" //ok
```

```swift
let name = "Steve" //Recognises the string type
name = "Jobs" //Error
```

####Computed Properties



####didSet

```swift
var name = "Steve" {
    didSet{
        print("The new name is: \(name)")
    }
}
```

Every time I change the value of `name` prints the new name.

#### Arrays

Declaring an array:

```swift
var array: [Int]
var array: Array<Int>
```

