# iOS - Swift - SwiftUI

CS193p thank you.

---

#### Table of Contents

- [`struct` vs `class`](#struct-vs-class)
  - [Similarities](#similarities)
  - [Diversities](#diversities)
- [MVVM design pattern](#mvvm-design-pattern)

---

### `struct` vs `class`

#### Similarities

- Stored `var`s

- computed `var`s:

  ```swift
  var computed: Int = { /* compute here */ }
  ```

- `let`s

- Initializers

  ```swift
  init(argName: Int) {
  	// do something
  }
  ```

- functions

  ```swift
  // first arg label is for external caller, second for internal.
  // If there's only one, it's both internal and external.
  func multiply(_ operand: Int, by otherOperand: Int) -> Int {
    return operand * otherOperand
  }
  multiply(5, by: 6)
  ```

#### Diversities

- `struct`
  - Value type
  - When passed/assigned gets copied
  - Functional programming
  - No inheritance
  - Free `init` inits all `var`s.
  - `var` are mutable, `let` are constant.
- `class`
  - Reference type
  - When passed/assigned, the pointer to it gets passed/assigned.
  - When is not referenced by anyone, gets removed from heap.
  - Object-oriented programming
  - Inheritance (single)
  - Free `init` inits NO `var`s.
  - Always mutable

### Generics: don't care type

```swift
struct Array<Element> { // Element type is a don't care
  // ...
}
var a = Array<Int>() // NOW I care about the type.
```

### Function type

```swift
(Int, Int) -> Bool
() -> Void

func areSameNumbers(firstNumber: Int, secondNumber: Int) -> Bool {
  return firstNumber == secondNumber
}
var foo: (Int, Int) -> Bool
foo = areSameNumbers

let same = foo(1, 1) // true
```

### Other stuff

#### Sugar syntax

When I'm calling a function and its last argument is in `{}`, I can close `)` before it.

```swift
func(arg1: Int, arg2: (Int) -> String) {}

func(1) { number in 'ciao!' }

// If I don't use some argument, just change it to `_`!
func(1) { _ in 'ciao!' } // I'm telling you this is nuts!!
```





### MVVM design pattern

Model-View-ViewModel

- Model: UI independent, has [data + logic].

- View:

  `@ObservedObject`, `@Binding`, `.onReceive`, `@EnvironmentObject` 

  - reflects the model
  - <u>stateless</u>
  - declarative
  - reactive.

- ViewModel:

  `ObservableObject`, `@Published`, `objectWillChange.send()`, `.environmentObject()`

  - binds the view to the model
  - interpreter
  - know the changes
  - Publishes "something changed"
  - has intent functions to be called by the view, then modifies the model.

