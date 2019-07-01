# what is an execution context? 

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

2. **ExecutionPhase**:














