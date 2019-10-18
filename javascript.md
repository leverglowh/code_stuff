# JavaScript:

#### Table of Contents

- [Variables and functions naming convention](#variables-and-functions-naming-convention)
	- [False values](#false-values)
- [Conditionals(if)](#conditionals)
	- [Dynamic assigned value](#dynamic-assigned-value)
	- [Ternary Operator](#ternary-operator)
	- [Switch](#switch)
- [Standard-output](#printing-stuff)
- [Functions](#functions)
	- [Anonymous Function](#function-expressions-anonymous-function)
	- [Other function syntaxes](#other-function-syntaxes)
	- [Default Parameters](#default-parameters)
- [Array](#array)
	- [Array methods](#array-methods)
	- [Iterators](#iterators)
- [Objects](#objects)
	- [Properties](#properties)
	- [Methods](#methods)
	- [Getter](#getter)
	- [Setter](#setter)
	- [Accessing objects](#accessing)
	- [Get a property in a var](#get-a-property-in-a-var)
	- [Built-in objects](#built-in-objects)
	- [Built-in Object methods](#built-in-object-methods)
	- [Looping through objects](#looping-through-objects)
- [Factory functions](#factory-functions)
- [Classes](#classes)


### Variables and functions naming convention:
- Start with a letter, underscore `_`, or dollar sign `$`.
- I can use letter/numbers/underscores/dollar sign $.
- I don't have to use camelCase and I can use car2go it's fine.
  But it's preferred.
- They are case sensitive.
- When a var begins with `_` means it shouldn't be freely altered.

##### False values:

- 0
- ""/'' Empty strings
- null
- undefined (Un-initialized vars)
- NaN (Not a Number)

### Conditionals:

```javascript
if () {
  //code
} else if () {
  //code
} else {
  //code
}
```

##### Dynamic assigned value
```javascript
let defaultName = username || 'Stranger'; //if username false, nome = Stranger
```
##### Ternary operator
```javascript
isOffTime ? letsGoHome() : keepWorkingHard();
```

##### Switch
```javascript
let groceryItem = 'papaya';

switch (groceryItem) {
  case 'tomato':
    console.log('Tomatoes are $0.49');
    break;
  case 'lime':
    console.log('Limes are $1.49');
    break;
  case 'papaya':
    console.log('Papayas are $1.29');
    break;
  default:
    console.log('Invalid item');
    break;
}
```

### Printing stuff:

 Template literals:

```javascript
let name = "Lucia";
console.log(`My name is ${name}.`); //use backtick to print var inline
```

### Functions:

```javascript
function addNumbers(x, y){
    return x + y;
} //traditional function, no semicolon
```

##### Function Expressions (anonymous function)
I don't name the function, and I store it in a variable (commonly a `const`);
```javascript
const calculateArea = function(width, height) {
  const area = width * height;
  return area;
};

const rectangleArea = (width, height) => {
    let area = width * height;
    return area;
};
```

To use the function Expression use the const name: `calculateArea(5, 7);`.

##### Other function syntaxes

```javascript
/* Omitting () for one parameter */

//zero parameters
const functionName = () => {};

//one parameter
const functionName = paramOne => {};

//two or more parameters
const functionName = (paramOne, paramTwo) => {};
```

```javascript
/* Omitting return and {} for single-line block */

//single-line block
const sumNumbers = number => number + number; //no need to write "return"

//multi-line block
const sumNumbers = number => {
    const sum = number + number;
    return sum; //two lines, yes "return".
};
```

##### Default parameters:

If the parameter is undefined or no argument is passed.
```javascript
function greeting(name = 'Stranger'){
    console.log(`Hello, ${name}!`);
}
```

### Array:

```javascript
var breakfast = ["coffee", "croissant"];
var many_types = ["string", 123, true, ["array", "inside"]];

breakfast[breakfast.length-1]; //access last element
```
##### Array Methods:
    - .length					//returns the length of the array
    - .concat(array a)			//returns a new array, with a concatenated
    - .pop()					//removes the last element
    - .push(element e)			//adds e as the last element
    - .reverse()				//returns the array, reversed

Index out of range: undefined.

##### Iterators:

Methods that help me iterate over arrays.

- `.forEach()` does something for each element. Returns undefined.

```javascript
let games = ['SAO', 'LZBW', 'PKM'];

games.forEach(game => {
    console.log("I don't have enough money to buy "+game+".");
});
```

- `.map()` creates a new array, mutating every item.

```javascript
const numbers = [1,2,3,4,5];

const squares = numbers.map(number => {
    return number*number;
});
```

- `.filter()` creates a new array with all elements that pass the test.
```javascript
const words = ['elite', 'exuberant', 'day', 'hi'];

const shortWords = words.filter(word => {
    return word.length < 5;
});
```

- `.findIndex()` returns the index of the first element that passes the test.
```javascript
const words = ['elite', 'exuberant', 'day', 'hi'];

const shortWords = words.filter(word => {
    return word.length < 5;
}); //2
```

- `.reduce()` returns one single value after iterating through the elements.
```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

### Objects:

##### Properties:

```javascript
let car2go = {
	name: "car sharing",
    id: 6741,
    available: true,
    'Fuel Type': 'diesel'
};
```
If the reference name has special characters (space etc.), I enclose it with quotation marks.

##### Methods:

```javascript
let car2go = {
    id: 6741,
    fuelLevel: 80,
    
    reFuel () {
        this.fuelLevel = 100;
    }
};
```

Functions that get an object as a parameter and then *reassign its properties*, **work**. 
If they *reassign the object*, they **don't work**. 

If I have to use `this` in a method, ***do not*** use arrow functions.

##### Getter:

```javascript
let car2go = {
    _fuelLevel: 80,
    
    get fuelLevel () { //Remember the `get` keyword
        if (this._fuelLevel) {
            return this.fuelLevel;
        } else {
            return null;
        }
    },
    
    reFuel () {
        this.fuelLevel = 100;
    }
};
```

##### Setter:

```javascript
let car2go = {
    _fuelLevel: 80,
    
    get fuelLevel () { //Remember the `get` keyword
        if (this._fuelLevel) {
            return this.fuelLevel;
        } else {
            return null;
        }
    },
    
    set _fuelLevel (level) {
        if (level) {
            this._fuelLevel = level;
        } else {
            console.log('nah');
        };
    }
};
```

##### Accessing:

  ```javascript
  car2go.id;
  car2go["id"];
  ```

##### Get a property in a var:

```javascript
let car2go = {
    id: 6741
}
const {id} = car2go; //var name should be the same of the property key
```

##### Built-in objects:

```javascript
Math.random(); //Random number between 0 and 1
Math.random() * 99; //Random number between 0 and 99
Math.floor(0.8945); //Round the number to the closest whole number
```

##### Built-in object methods:

- `Object.keys(obj)` returns and array of obj's property keys.

```javascript
const car2go = {
    id: 154,
    name: 'Charlie'
}
Object.keys(car2go); //['id','name']
```

- `Object.entries(obj)` returns an array of obj's properties [key, value] pairs.

```javascript
const car2go = {
    id: 154,
    name: 'Charlie'
}
Object.entries(car2go); //[['id',154],['name','Charlie']] /*Order is not guaranteed*/
```

- `Object.assign(target, source)` copies all source's properties and merges it with target's, mutating it.

```javascript
const car2go = {
    id: 154,
    name: 'Charlie'
}
const car3go = {
    id: 166,
    isFree: true
}
let assigned = Object.assign(car2go, car3go);
//car2go = {id:166, name:'charlie', isFree: true}
//assigned = {id:166, name:'charlie', isFree: true}
```

If I want to keep target the way it is, I have to use Object.create(target) .

- `Object.create(obj)` creates a new object copy of the obj.

```javascript
const car2go = {
    id: 154,
    name: 'Charlie'
}
let copy = Object.create(car2go);
//copy = {id:154, name:'Charlie'}
```

##### Looping through objects:

```javascript
let letters = {
    a: 1,
    b: 2,
    z: 26
};
for (let letter in letters) {
    console.log(letter); //a b c
    console.log(letters[letter]); //1 2 26
};
```

`letter` is the properties' key, to access the values, I have to use obj[property].

### Factory functions:

```javascript
const factory = (id, isOk) => {
    return { //returns an object
        id,
        isOk
    }
};
```

### Classes:

WIP