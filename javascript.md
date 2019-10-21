# JavaScript:

#### Table of Contents

- [Variables/functions/classes naming convention](#variables-functions-and-classes-naming-convention)
	
	- [False values](#false-values)
	
- [Conditionals(if)](#conditionals)
	- [Dynamic assigned value](#dynamic-assigned-value)
	- [Ternary operator](#ternary-operator)
	- [Switch](#switch)
	
- [Standard-output](#printing-stuff)

- [Functions](#functions)
	- [Anonymous function](#function-expressions-anonymous-function)
	- [Other function syntaxes](#other-function-syntaxes)
	- [Default parameters](#default-parameters)
	
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
	- [Built-in object methods](#built-in-object-methods)
	- [Looping through objects](#looping-through-objects)
	
- [Factory functions](#factory-functions)

- [Classes](#classes)
  - [Using classes to create objects](#using-classes-to-create-objects)
  - [Subclasses and inheritance](#subclasses-and-inheritance)
  - [Static methods](#static-methods)
  
- [Modules](#modules)
	- [ES5](#es5)
	- [ES6](#es6)
	
- [Promises](#promises)
  - [setTimeout](#settimeout)
  - [then](#then)
  - [catch](#catch)
  - [Chaining Promises](#chaining-promises)
  - [Promise.all()](#promiseall)
  
- [async ... await](#async--await)
	- [async](#async)
	- [await](#await)
- [try ... catch](#try--catch)
	- [try](#try)
	- [catch](#catch)
---

### Variables, functions and classes naming convention:

Variables and functions:

- Start with a letter, underscore `_`, or dollar sign `$`.

- I can use letter/numbers/underscores/dollar sign $.

- I don't have to use `camelCase` and I can use car2go it's fine. But camelCase is preferred.
  
- They are <u>case sensitive</u>.

- When a var begins with `_` means it shouldn't be freely altered. (private)

Classes:

- `CamelCase`

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

##### Function expressions (anonymous function)
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
##### Array methods:
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

```javascript
class Cat {
    //special method that creates the object
    constructor(name){
        this._name = name; //these are the properties
        this._favoriteFoods = [];
    }
    
    get name(){
        return this._name;
    }
    get favoriteFoods(){
        return this._favoriteFoods; //!!! Reference is returned, may not be safe.
    }
    addFavoriteFood(food){
        this._favoriteFoods.push(food);
        return food;
    }
}
```

##### Using classes to create objects:

```javascript
class Cat { ... }
let sachi = new Cat('Sachi'); //a new Cat object with name = 'Sachi'
```

##### Subclasses and inheritance:

```javascript
class Animal {
    constructor(name){
        this._name = name;
        this._favoriteFoods = [];
    }
    get name(){}
    get favoriteFoods(){}
}

class Cat extends Animal {
    constructor(name, usesLitter){
        super(name);
        this._usesLitter = usesLitter;
    }
}
let sachi = new Cat('Sachi', usesLitter);
sachi.name //Sachi
```

##### Static methods:

I use these if I have to call a method from the class itself and not from an object of the class.

```javascript
class Animal {
    constructor(name){
        this._name = name;
        this._favoriteFoods = [];
    }
    get name(){}
    get favoriteFoods(){}
    static generateName () { //returns a random element from listNames list
        const listNames = ['a', 'b', 'c'];
        return listNames[Math.floor(Math.random()*listNames.length)];
    }
}
Animal.generateName //a / b / c
```

### Modules:

##### ES5:

- Export the module (ES5):

```javascript
let Airplane = {
    type: "StarJet",
    showTyp: function () {
        console.log(this.type);
    }
};

module.exports = Airplane;
```

- Import it in another file (ES5):

```javascript
const Plane = require('./airplane.js') //import from file
function showType() {
    console.log(Plane.type);
}
showType(); //"StarJet"
Plane.showTyp(); //"StarJet"
```

##### ES6:

- export default:

```javascript
const Cat = {
    name: 'Sachi',
    isCute: true
};
export default Cat; //same as module.exports = Cat;
```

- import:

```javascript
import sachi from './cat'
//sachi is now my variable for Cat module
console.log(sachi.isCute); //true
```

- named export:

```javascript
let nameList = ['a', 'b', 'c'];
let num = 4;

export {nameList, num}; //export later

export let keys = [1, 2, 3]; //export at once
```
- named export as:

```javascript
export {nameList as people, num as quota};
```

- named import:

```javascript
import {nameList, num} from './cat'
console.log(nameList[0]); //'a'
console.log(num); //4
```

- named import as:

```javascript
import {nameList as people} from './cat'
import * as Cat from './cat';
Cat.name; //'Sachi'
```

- Mixing these up:

```javascript
export let foods = '';
function isVeg() {
}; 
function isLow() {
}; 
function isGFree() {
};

export {isVeg as veg};
export default isGFree;
```

```javascript
import {foods, isVeg} from './menu';
import isGFree from './menu';
```

### Promises:

Promises are special objects. When executed, the `pending` state (initial state) is changed to `resolved` or `rejected`.

```javascript
let isHappyDay = false;
const someExecutor = (resolve, reject) => {
    if (isHappyDay) {
        resolve('Success message!');
    } else {
        reject('Reject message!');
    };
};

let myPromise = new Promise(someExecutor); //Executes someExecutor
console.log(myPromise); //'Reject message!'
```

##### setTimeout:

`setTimeout(function, timeout)` waits at least `timeout` milliseconds, then executes the `function`.

```javascript
function printOne(){
    console.log('1');
};
setTimeout(printOne, 2000);
//After at least 2 seconds, '1' is printed

const makePromise = () => {
    return new Promise((resolve, reject) => {
        setTimeout( () => {resolve('I will make a Promise!')}, 1000);
    });
};
const promised = makePromise();
```

##### then:

When a promise is settled, I can use `.then(onFulfilled, onRejected)`. The arguments are functions, handlers of the settled value.

```javascript
const myNumber = Math.random()*999999;
let lottery = new Promise((resolve, reject) => {
    let num = Math.random()*999999;
    if (myNumber===num){
        resolve("I'll buy everything.");
    } else {
        reject("Keep working!");
    };
});

const handleWin = (winMessage) {
    console.log(winMessage);
    myNumber = Math.random()*999999;
    playAgain();
};
const handleLost = (loseMessage) => console.log(loseMessage);

lottery.then(handleWin, handleLost);
```

##### catch:

This is the same thing as `then` except it accepts only one argument: `onRejected`. We can use `then` *and* `catch` to write clearer code:

```javascript
prom
	.then((resolvedValue)=>{
    	console.log(resolvedValue);
	})
	.catch((rejectionReason)=>{
    	console.log(rejectionReason);
	});
```

##### Chaining promises:

I can chain multiple promises, using `then` in chain:

```javascript
let ok = true;

const startWashing = (clothes) => { 
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            ok? resolve(clothes) : reject('nope');
        }, 500);
    });
}
const startDrying = (clothes) => {
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            ok? resolve(clothes) : reject('nooope');
        }, 500);
    });
}

const foldClothes = (clothes) => {
	return new Promise((resolve, reject)=>{
        setTimeout(()=>{
        	ok? resolve(clothes) : reject('nooooooooope');
        }, 500);
    });
}

function putOnClothes = (clothes) => {
    console.log('Put those on!!!');
};
function burnEverything = (clothes) => {
    console.log('I burnt everything :>');
}
//When everything should be done in order
startWashing(myClothes) //returns a promise
	.then((washedClothes) => {
    	return startDrying(washedClothes);
	})
	.then((dryedClothes)=>{
    	return foldClothes(dryedClothes);
	})
	.then(putOnClothes, burnEverything);
```

The variable `clothes` is *automatically passed on* through the chain if promises are successfully resolved, otherwise the error message is passed on. 

##### Promise.all()

`Promise.all([promise1, promise2])`
I let all promises start together, if everything returns ok, `Promise.all()` returns an array of resolved values, otherwise returns the first reject reason and stops (*failing fast*).

```javascript
let myPromises = Promise.all([promise1, promise2]);
myPromises
	.then((arrayOfOks)=>{
    	console.log(arrayOfOks);
	})
	.catch((errorMessage)=>{
    	console.log(errorMessage);
	});
```

### async ... await:

##### async:

I use `async` keyword to write functions that handle asynchronous actions.
`async` functions *always* return a `promise`: 

1. The function returns nothing: promise with resolved value = undefined.
2. The function returns a non-promise value: promise with resolved value = value.
3. The function return a promise: promise.

```javascript
async function myFunc(){
    //yadayadayada
};

const myFunc = async () => {
    //yadayadayada
};

myFunc();
```

##### await:

It can only be used inside an `async` function; it is an operator, ***pauses the async function until a given promise is resolved*** and ***returns the resolved value***.

```javascript
const makePromise = () => {
    return new Promise((resolve, reject) => {
        setTimeout( () => {resolve('I will make a Promise!')}, 1000);
    });
};

async function waitForPromise() {
    let promised = await makePromise(); //waits for it to finish promising
    console.log(promised); //'I will make a Promise!'
}

async function waitForAll() {
    let promises = await Promise.all([makeOne(), makeTwo(), makeThree()]);
}
```

If I don't put `await` in an `async` function, it can be executed *before we expected*, getting an unresolved promise in the "pending" state.

It's nice if we add `await` to vars only the moment they're used and not when they're declared. In this way all the async functions start to run, and I wait only if I need to and only for the one I need.

```javascript
async function hello(){
    let promised = makePromise();
    console.log(await promised);
}
```

### try ... catch:

I use `try ... catch` to catch errors in `async ... await` functions.

```javascript
async function catchingErrors() {
    try {
    	let resolvedValue = await asyncFunction();
    } catch (errorMessage) {
        console.log(errorMessage)
    }
}
```

