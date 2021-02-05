---
layout: post
title: Understanding Dynamic Typing in JavaScript
author: Naveen Bhatia
comments: true
tags:
- Javascript

---
JS is an interpreted and dynamically typed language - which means that the "type" of data that can be stored in a variable is not fixed before the program is actually executed - it's determined by the type of the data stored in it at run time i.e. at the time the assignment instruction is executed during the program run. In other words, it's not the variables that are tied to a type - it's the data that has a type. So we can have data values that are numbers or strings, but not the variables.

![typeof](/assets/imgs/typeof-var.png)

In the example above, we can see that a variable by itself - i.e when it's declared without assigning a value to it - has no type (its type is _undefined)_. And when a value is assigned to it, its type is reported as the type of the data value contained in it.

## Type of a variable is dynamic

In JS, the type of a variable is _dynamic_ - which means that a variable in JS can have a value of any type at any point during its lifetime. Let's understand this with the help of another example:

![DynamicTyping](/assets/imgs/dynamic-typing.png)

We know from the previous example that when we create a variable without assigning any value to it, its type is reported as undefined. And here in this example we can see that when we assign different values to it, the _type of the variable_ changes according to the type of the data value assigned to it. It changes from "number" to "string" to "boolean" to "object" etc as per the type of the value contained in it. So we see that the type of the variable is not fixed, it can change with each assignment - and that's why it's dynamic typing. And that's why it's _loosely typed_ too (as opposed to strict typing) - it's not tied to any particular type during it's lifetime.

Contrast this with strict and static type checking. In a typical compiled language (C, C++, Java etc), the type of a variable has to be declared clearly in the program, and this type can not be changed at runtime. So a variable declared of the type 'integer' can only be assigned integer values - and can not have non-integer data values assigned to it at run-time. This is static typing. Any attempt to assign a value to a variable which if different from the type declared for that variable would result in a compilation error.

## So which is better - dynamic or static typing?

While dynamic typing gives you a lot of freedom and flexibility as a programmer - you don't need to over-burden your mind thinking about what types are appropriate, you don't need to declare a new variable every time you're handling a new data type, you can easily reuse a previously declared variable etc. But this same flexibility could result in some unintentional and hard-to-find bugs being introduced in the program. Consider the following example:

```javascript
let applyCharges = function (accObj, charges, msg) {
  if (accObj.balance < accObj.MIN_BALANCE_ALLOWED) {
    accObj.balance -= charges; 
    // log this transaction..
    addTransaction(accObj, msg);
  }
  else {
    // do not apply any charges
    // ...
  }
}

// meanwhile in some other place in the code..
let msg = "Low Balance Charges Applied";
let charges = LOW_BALANCE_CHARGES;
applyCharges(accObj, msg, charges); // order of function parameters flipped 
```

In the example above, we want to apply some charges to a bank account when a minimum balance in the account is not maintained. But while implementing this simple logic, the programmer seems to have made an inadvertent error. In the parameters that are passed to the function responsible for applying these charges, the order of the parameters being passed is different from what the function expects. Since it's the second parameter which is what is subtracted from the account balance - it's expected to be of numeric type. But instead a string type parameter is being passed in its place. As we know that the type of variables expected can't be explicitly stated in JS, there's no type checking performed by the language (remember, there's no compiler).

Also, because of the automatic type conversion (aka **implicit coercion**) in JS, the JS interpreter quietly performs this type conversion, and saves **NaN** in accObj.balance (whenever there's a subtraction operation between a number and a string, the result is NaN - a special value designated to indicate that the value is not-a-number). Now clearly that's a problem. And if you fail to catch this error during testing, it will only come to light when a customer reports it.
On a compiled language which is statically typed, there's no way such an error could go unnoticed. The compiler would catch it immediately.

![automatic-type-conversion](/assets/imgs/implicit-coercion.png)

Another thing worth highlighting through this example is the likelihood of the buggy block of code getting executed. If it's execution is contained in a if-else block, and if the condition/s requiring it to execute is less likely to be true - then the bug could just sit there in the production code for many days before it's reported.

One way to catch these bugs before they get to production is to write enough quality test cases so as to ensure good code coverage. But is there something we could do to avaoid such situaltions completely? Read on.

## How to avoid errors caused by weak type checking

Some of the ways we can avoid such errors completely are:

1. Using a linter
2. Doing explicit type checks
3. Using Typescript instead of Javascript

Using a linter is a good practice, although in this particular case my linter (ESLint) failed to catch or report this error (even with no-implicit-coercion rule turned on).

Another way to avoid such errors is to do explicit type checking:

```jsx
var addNumbers = (a, b) => {
  console.assert( typeof a == "number" && typeof b == "number", 
         "Error: Both params not numbers");
        
  // now we can safely compute the result without any risk 
  // of automatic type conversion
  return a+b;
}

var first = "two";
var second = 2;

var result;
// this would succeed
result = addNumbers(10, 20);

// this would fail
result = addNumbers(first, second);
```

But doing explicit type-checking before every operation is not feasible. But for a critical function like updating an account, in my opinion it makes sense to do it.

And finally, you could switch to Typescript - which is a superset of Javascript. It extends JS by adding types to it. Code written in TS is transformed into JS by compilation. And since TS enforces typed declarations of variables, such errors are caught during the compilation process - much before the code is actually executed.