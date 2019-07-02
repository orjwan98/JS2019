# What is an execution context? 

All JavaScript code needs to run in an environment, and that environment is called **The Execution Context**. You can imagine the Execution Context as a box, a container, or a wrapper which stores variables and a place in which our code is evaluated and executed. 

Now, the default execution context is *always* the **Global Context**. All code that is **NOT** inside a function is executed in the **Global Context**. For example, variables and functions that are NOT defined inside other functions are evaluated and executed inside the Global Context. 

You can think of contexts as objects. The Global execution context is associated with the Global Object. For example, the global context/object in browsers is the **window object**. 

```js
var name = 'Mark'

console.log(this) //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
console.log(this.name) //logs 'Mark'
console.log(window.name) //logs 'Mark'
console.log(name === this.name) //logs true

```
As you can see, it's as if the `name` variable is a property of the window object. Remember that properties are variables living on an object. 

**Now, what about code that lives inside functions? Like variables and functions that are declared inside another function's scope?**

It's pretty simple. Each time a function is **called**, it gets its own execution context. 

# The Execution Stack

The Execution Stack keeps track of each new context created and what context is active in a certain moment of time. Let's take the following example:

```js
var name = 'Mark'

function first(){
var a = 'Hello!';
second(); 
var x = a + name;
}

function second(){
var b = 'Hi';
third();
var z = b + name;
}

function third(){
var c = 'Hi';
var z = c + name;
}

first();

```

Let's visualize this: 

- First, the current active context (by default) is the global context. 

In this code, we are **declaring** the variable `name`, and the functions `first`, `second` and `third` in the **Global Context**. And we are by default working in that context, but when we call `first()`, is when a new context is created and added to the **Execution Stack**. 

Let's read the code step by step: 

- First, all variable and function declarations that are out in the open, are declared inside the default context. 
- When `first()` is called. A new context is created and added to the execution stack. Right ontop of the default `global` context. 
- Now `first` is the current active context. 
-  Variable `a` in `first` context is declared. 
- 'second()' is called. A new context `second` is created and added to the execution stack. Right ontop of `first` context. 
- Now,`second` is the current active context. 
- Variable `b` in `second` context is declared. 
- `third()` is called. A new context `third` is created and added to the execution stack. Right ontop of `second` context. 
- `third` is the current active execution context.
- Variable 'c' and then variable 'z' in `third` are declared. 
- Exit of context 'third' and remove it from the execution stack. 
- Context `second` is currently active again. 
- Declare variable 'z' in 'second'. 
- Exit 'second' context and remove it from the execution stack. 
- Context 'first' is currently active again. 
- Declare var 'x' in 'first'. 
- Exit 'first' context and remove from execution stack. 
- Current active context is the default 'global' context. 

# Execution context in depth. 

We explained above when an execution context is created. Right now, we're going to discuss the *how*. Previously we have mentioned that we can associate an execution context with an *object*, and this object has 3 properties: 

1. **The Variable Object**: Which will include the function's arguments, the inner variable declarations of the function, and any other function declarations inside the current context. 
2. **The Scope Chain**: Which contains the current variable object, as well as the variable object of all its parents. 
3. **"This" Variable**: Which we've talked about before. 

Like we said previously, when a function is called a new execution context is created and put ontop of the execution stack. And this happens in two phases:

1. **Creation Phase**:
In the creation phase, we define/create/deteremine each property of the execution object above.

2. **ExecutionPhase**: The code of the current execution context is ran line by line.

# What happens to the Variable Object in the creation phase?

1. The argument object is created, containing all the arguments that were passed into the fucntion. 
2. The code is scanned for **function declarations**: for each function, a property is created in the Variable Object, **pointing to the function**. This means that all the functions will be stored inside the variable object. Even before the code starts executing. 
3.The code is scanned for **variable declarations**: for each variable, a property is created in the Variable Object, **and is set to undefined**.

We usually refer to points no.2 and no.3 as **hoisting**. Functions and variables are hoisted in JS, which means that *they are available before the execution phase actually starts*.But there is a difference in the way a variable and a function is hoisted. Functions are actually already defined before the execution phase starts. While variables are setup to `undefined` and will *only* be defined in the execution phase. 

# Hoisting in practice

## Functions and functions expressions hoisiting

Look at the following block  of code and try to guess what will happen. 

```js

calcAge(1998) //will this throw an error or log 21? //it will log 21

function calcAge(currentYear=2019, birthYear){
console.log(currentYear - birthYear)
}

```
Now, you may be thinking that this will throw an error about the function being undefined. But, when the context where `calcAge` lives gets created which in this case, is the global context, all functions get defined in the Variable Object before any code is executed. Which means that the function is available to be used. 

**What about function expressions?**
This behavior doesn't apply to function expressions. Example:


```js
calcAge(1998) //will this throw an error or log 21? //it will throw a reference type error

var calcAge = function (currentYear=2019, birthYear){
console.log(currentYear - birthYear)
}

```
## Variable hoisting

Look at the following block of code and try to guess what will happen:

```js

console.log(greeting) //logs undefined

var greeting = 'Hi'
//var farewell = 'Bye'

console.log(greeting) //logs Hi
console.log(farewell) //logs an error - not defined
```
Now, why does this happen?

In the creation phase, variables are declared but their value is set to undefined. So, JS knows we have a variable that holds the name `greeting` but its value is yet to be defined in the execution phase. So when we try to log it, it logs out `undefined`, which is the data type for any variable that doesn't hold a value yet. But JS can't recognize a variable called `farewell` at all. So when we try to log it, it tells us that its `not defined` *at all*.

Now take a look at these next blocks of code and try to understand what is happening:

```js
var age = 23

sayAge();

function sayAge(){
  var age = 48
  console.log('first',age) //logs first 48
}

console.log('second',age) //logs second 23
```

In the code snippet above, each one of the `age` variables is a  seperate variable that's living in a different execution context and lives in a seperate variable object than the other. 

```js
var age = 23

sayAge();

function sayAge(){
  age = 48
  console.log('first',age) //logs first 48 
}

console.log('second',age) //logs second 48
```

```js
var age = 23

sayAge();

function sayAge(){
  console.log('first',age) //logs first 23
}

console.log('second',age) //logs second 23
```

