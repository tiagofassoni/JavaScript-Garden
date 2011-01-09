### Scopes and Namespaces

Although JavaScript deals fine with the block scope syntax of two matching curly
braces, it does **not** support block scope. Therefore all that's left is *function
scope*.

> **Note:** When not used in an assignment or as a function argument, the `{...}`
> notation will get interpreted as a block statement and **not** as an `Object`. 
> This, in conjunction with 
> [automatic insertion of semicolons](#automatic-semicolon-insertion), can lead
> to subtle errors.

Additionally, there are no distinct namespaces in JavaScript, this means that 
everything gets defined in **one** globally shared namespace.

Each time one references a variable, JavaScript will traverse through the scopes 
upwards until it finds it. In the case that it reaches the global scope and still 
can't find the requested name it will raise a `ReferenceError`.


#### Local Variables

If one wants to declare a variable *local* to the current scope thes have to use 
the `var` keyword. **Always** use the `var` keyword when declaring variables
otherwise you might **overriding** things that were already defined in outer
scopes.

    // global scope
    var foo = 1;
    var bar = 2;

    function test() {

        // local scope of the function test
        var foo = 3;
        bar = 4;
    }

In the above, `var foo` inside of `test` will create a new variable that is in
the *local* scope. Therefore the value of the *global* `foo` does **not** get
changed. But the assignment `bar = 4` will override the value of the *global*
`bar` due to the missing `var` keyword.

#### Name Resolution Order

All scopes in JavaScript - including the global one, have the name 
[this](#how-this-works-in-javascript) defined in them, which refers to the 
"current object". Function scopes also have the name
[arguments](#function-arguments) defined, which contains the arguments that were 
passed to a function.

For example, when you try to access a variable named `foo` inside a function 
scope, JavaScript will lookup the name in the following order:

 1. In case there's a `var foo` statement in the current scope use that.
    
 2. If one of the function parameters is named `foo` use that.
 
 3. If the function itself is called `foo` use that.

 4. Go to the next outer scope and start from **#1** again.

> **Note:** Having a parameter called `arguments` will **override** the default
> `arguments` object.

#### Namespaces

One common problem of having only one global namespace is, that its very easy to 
run into problems where variable names clash. Luckily this can be easily avoided 
with the help of *anoynmous function wrappers*.

    (function() {
        // a self contained "namespace"
        
        window.foo = function() {
            // an exposed closure
        };

    })(); // execute the function immediately

By default you cannot just call a function, you need to **evaluate** it first. 
In this example, this is done by wrapping the 
[function expression](#functions-and-statements) in parenthesis. While this is 
the most common style to do this, everything else that forces the function to be 
evaluated works just as well, like for example `+function(){}()`.

#### Best Practices
Always use the *anonymous wrapper* to encapsulate your code in case there's any 
chance it might get used by someone else in their project. Also never define any 
variables in the *global namespace*, always use `var` to limit the scope of your 
variables.
