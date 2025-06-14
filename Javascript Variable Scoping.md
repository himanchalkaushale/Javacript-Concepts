Javascript has two kinds of scope:

1. Local (inside a function)
2. Global (outside a function)

### The `var` Keyword
* Using the `var` keywords creates the variable in the current scope
    * If the current scope is global, then `var` is unnecessary (see below)
    * If the current scope is local, then you’re creating a local variable in the current scope
* If you don’t use `var`, then Javascript goes up the “scope chain” to see if it’s already been declared
    * If the variable is not found to already exist, then the variable is created in the outer-most global scope
    * Therefore, you can accidentally create global variables inside a function by not using `var`
        * Bad side effects
    * Technically, when you’re not using `var`, you're writing a property assignment, not a variable assignment
        * You’re updating the *property* with that variable name, on whatever object that property is part of
            * Could be the "local scope" object or any outer scope objects or the global scope object
        * If it doesn’t already exist, added as a property to the global object (ie “window” in browsers)

Local scopes can be nested, like so:

``` Javascript
// Code block #1
(function(){
  var a = 'outer';
  console.log("outer scope: " + a); // prints “outer"
  (function(){
    var a = 'middle';
    console.log("middle scope: " + a); // prints “middle"
    (function(){
      var a = 'inner';
      console.log("inner scope: " + a); // prints “inner"
      (function(){
        console.log("inner most scope: " + a); // prints “inner”, since `a` was found one level up in the scope chain
        var b = "inner most";
      })();
      console.log(b); // ReferenceError since b isn't defined in this function's scope.
    })();
  })();
})();
```

### Hoisting
* Javascript variables are “hoisted” automatically by the interpreter
    * The `var` declaration is treated as if it appears at the very top of its current scope

``` Javascript
// Code block #2
function sayHi() { 
 alert(message) 
 var message = "Hi there"; 
}
```

is actually treated as if it were written


``` Javascript
// Code block #3
function sayHi() { 
 var message;  // implicitly equals “undefined”
 alert(message) 
 message = "Hi there"; 
} 
```

Automatic hoisting is why code block 2 alerts out “undefined” instead of throwing an error, even though the variable doesn’t appear to be declared until after the alert. It is “hoisted” to the top of its scope, and declared there so that it can be used anywhere in its scope, even if it hasn’t been assigned a value yet.

Hoisting can cause weird side effects, like in this example:

``` Javascript
// Code block 4
var num = 100; 
foo(); 
 
function foo(){ 
    alert(num);
    if (false) { 
        var num = 123;   
    } 
}
```

You might expect this to alert out “100”, but it will actually alert out “undefined” since the inner `var num` declaration is hoisted to the top of the foo() function’s scope, where it is given the value "undefined".

### Block Level Scope
* There is no block level scope, yet...
    * variables created inside if-statements and for-loops are available to code outside that statement
    * as we’ve seen in code block 4, they can be hoisted up and overwrite references even before they’re assigned to
* ECMAScript 6 introduces block level scope between curly braces {…}
    * Use the `let` keyword instead of `var`
        * Variables defined with `let` keyword aren’t hoisted

``` Javascript
// Code block 5
function example(x) { 
    console.log(y); // ReferenceError: y is not defined since y isn’t hoisted  
    if (x) { 
        let y = 5; 
    } 
}
```

ES6 also introduces the `const` keyword, which is a “true” constant and must be initialized.

``` Javascript
// Code block 6
var a; // a === undefined 
const b; // SyntaxError: const declarations must have an initializer 
```

``` Javascript
// Code block 7
const b = 10; // b === 10 
b = 20; // SyntaxError: Assignment to constant variable 
```

Unfortunately, ES6 is [still experimental for now](http://stackoverflow.com/questions/13355486/when-will-i-be-able-to-use-es6-in-a-browser).

## References
[http://www.coolcoder.in/2014/03/everything-you-need-to-know-about.html](http://www.coolcoder.in/2014/03/everything-you-need-to-know-about.html)
[http://stackoverflow.com/questions/1470488/what-is-the-function-of-the-var-keyword-in-ecmascript-262-3rd-edition-javascript](http://stackoverflow.com/questions/1470488/what-is-the-function-of-the-var-keyword-in-ecmascript-262-3rd-edition-javascript)