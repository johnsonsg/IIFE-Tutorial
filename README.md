# IIFE - Immediately-Invoked Function Expression

* Pronounced "IIfy" by Ben Alman who introduced the acronym

* What is an IIFE
  - Only invoked once. 
  - An IIFE (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined.

# Why Use an IIFE
  - An IIFE is a good way at protecting the scope of your function and the variables within it.

* 5 Variations
  - [with anonymous arrow functions inside](#with-anonymous-arrow-function-inside)
  - [with the functions keyword](#with-the-functions-keyword)
  - [with a function name: (Allows for recursion)](#with-a-function-name)
  - [The Revealing Pattern (Variation of the Module Pattern)](#the-revealing-pattern)
  - [Injecting a namespace object](#injecting-a-namespace-object)

* 3 Reasons to use an IFFE: 
  - [It does not pollute the global object namespace - Code Example](#does-not-pollute-the-global-object-namespace)
  - [Private Variables and Methods from Closure - Code Example](#private-variables-and-methods-from-closure)
  - [The Module Pattern - Code Example](#the-module-pattern)
  

## Variations

### with anonymous arrow function inside: 
```
(() => {
// do stuff
})();

(() -> {
// When the arrow function is contained by Paranthesis that is what makes it an IIFE
})

The Paranthesis at the end (), this is the operator that calls it into action.
so when the function has no name like this it is immediately invoked. 

```

### with the functions keyword: 
```
(function (){
// essentially the same but not an arrow function, it just uses the function keyword.
})();

```


### with a function name:
```
// allows for recursion: where the function calls itself

// Immediately invoked

(function myIIFE(num = 0){
  num++
  console.log(num)
  return num !== 5 ? myIIFE(num) : console.log('finished')
})(); 

// passing in a parameter and giving it a value in case the parameter is not provided. 
This enables us to call the function (allows for recursion) again. So if we need an IIFE with recursion 
where the function actually calls itself, we can refer to the function by name within.

After, it is executed outside the function scope, you can not call it again. It can only be called once. 
It would only be referred to by the name of the function inside the IIFE, because after it is invoked,
the IIFE is no longer available. 


```

## 3 Reasons to use an IFFE

### Does not pollute the global object namespace
```
// global
const x = 'whatever'

const helloWorld = () => 'Hello World!'

// isolate declarations within the function
(() => {
  const x = 'life whatever'
  const helloWorld = () => 'Hello IIFE!'
  console.log(x)
  console.log(helloWorld())
})()

the IFFE is immediately invoked, but you can how the functions outside the IFFE are waiting to be called. so, lets now lets console.log() them.

// global
const x = 'whatever'

const helloWorld = () => 'Hello World!'

// isolate declarations within the function
(() => {
  const x = 'life whatever'
  const helloWorld = () => 'Hello IIFE!'
  console.log(x)
  console.log(helloWorld())
})()

console.log(x)
console.log(helloWorld())

// Now you return the the two functions outside the IFFE, and this creates a namespace.
```

### Private Variables and Methods from Closure
```
const increment = (() => {
let counter = 0;
console.log(counter)
const credits = (num) => console.log(`I have ${num} credits`); // Credits function
return () => { counter++; credits(counter); } // Anonymous function
})()

You will see the immediately invoked expression is called once: returning '0' and now it is complete.
But now lets call the function increment

const increment = (() => { 
  let counter = 0;
  console.log(counter);
  const credits = (num) => console.log(`I have ${num} credits`); // Credits function
  return () => { counter++; credits(counter); } // Anonymous function
})()

increment(); // Call function

Now it calls our anonymous function into action, and it still has lexible scope to 
refer to the counter variable.  And if you call increment() again you can see 
it adding to the num count.

However, if you try to access the counter var directly or the credits function 
directly you will get a ref error because they are not availabe in the global scope

credits(3); // ref error

```

### The Module Pattern
```
// (Modules were introduced ES6)

// Creating an Object that will be returned 
const score = (() => {
  let count = 0; // private variable

// return object
  return {
    current: () => { return count },
    increment: () => { count++ },
    reset: () => { count = 0 }
  }
})();

You will see that when you run it just like this, it returns nothing. 
But, 'Score' will now have access to the methods returned in the Object.

So, lets look at an example of that by calling 'Score' with a dot notation:

// Creating an Object that will be returned 
const Score = (() => {
  let count = 0;

  return {
    current: () => { return count },
    increment: () => { count++ },
    reset: () => { count = 0 }
  }
})();

Score.increment();
console.log(Score.current());

```

# More Variations
## The Revealing Pattern
```
// The Revealing Pattern is a variation of the Module Pattern

const Game = (() => {
  let count = 0;
  const current = () => { return `Game score is ${count}.`};
  const increment = () => { count++ };
  const reset = () => { count = 0 }

  return {
    current: current,
    increment: increment,
    reset: reset
  }
  
})();

Game.increment();
console.log(Game.current());

// Using pointers instead of defining these methods inside of the return Object.
// This is called the Revealing Pattern.


```

## Injecting a namespace object
```
((namespace) => {
  namespace.count = 0;
  namespace.current = function() { return `App count is ${this.count}.`}; 
  namespace.increment = function() { this.count++ };
  namespace.reset = function () { this.count = 0 };
})(window.App = window.App || {});


// Noticed how we dont have a const definition at the beginning of the iife.
We are just defining the iife, and we call it into action with the (window.App...)
So, lets break it down. We got a namespace parameter at the beginning, and refer to 
the namespace through out passing it into an Object to this iife. 
Then we are using the dot notation to set the values to the property/method.

Also noticed how we are using the function keyword instead of the arrow function, 
we do this so we can use the keyword 'this.' inside of the methods.

```





Notes: To learn more about github styling please visit: https://github.com/fefong/markdown_readme
Ref: https://www.youtube.com/watch?v=8GDk8sj0YgQ
