---
title: let,var or const?
date: "2020-12-15T22:12:03.284Z"
description: "When to use let,var and const? Check out the blog."
---

Hello everyone!!

In the previous versions of javascript, all the variable declartion was made using the "var" keyword.But after the ES6 2015 update, javascript came with some amazing features and one of the important highlight was "let" and "const" can also be used for variable declaration.Hence, in this blog we will have a look at the key differences in 'let','const' and 'var'.

Before diving into the differences directly, lets first understand the variable scoping.

#### Function Scoping

Scope of variable is limited to the function only i.e., the variables which are declared inside the function are not accessible outside that function.

```js
function func() {
  var items = 12
  console.log(items) //12
}
func()
console.log(items) //ReferenceError: items is not defined
```

So, if you look at the above code, the variable "items" is scoped inside the function and hence when we console log it inside the function we get the answer but when we did the same outside the function, we got an error that the variable is not defined.

#### Block Scoping

A block scope is the area within if, switch conditions or for and while loops. Generally speaking, whenever you see {curly brackets}, it is a block.

```js
function func() {
  if (true) {
    var fruit1 = "apple"
    let fruit2 = "mango"
  }
  console.log(fruit1) //apple
  console.log(fruit2) //fruit2 not defined
}

func()
```

If you have a look at the above code, fruit1 variable is defined but the fruit2 variable isn't . This is because the var variables' scope is limited to the function but the let variables' scope is limited to the block (in this case it is the 'if' statement).

#### let

"let" keyword works on the basis of block scope."let" keyword can be updated but cannot be re-declared.Have a look at the code below:

```js
let items = 13
console.log(items) //13
items = 20
console.log(items) //20

let items = 21
console.log(items) //SyntaxError: Identifier 'items' has already been declared
```

items is declared and initialized as 13 and then is updated to 20.But when we try to redeclare items again, it will throw an error. But if the variables are scoped in different blocks then it can be reused.

#### const

"const" keyword is immutable i.e., const variables cannot be redeclared. It should be initialized at the time of declaration only.But it can work well when the const vaiables are in different scopes.

```js
const items = 12
if (true) {
  const items = 45
  console.log(age) //45
}
console.log(age) //12
age = 15
console.log(age) //TypeError: Assignment to constant variable.
```

But but but, the const vraible works differently in case of objects. For instance:

```js
const name={
    firstName: "Pranav",
    lastName: "Bakale"
}
console.log(name);  //{ firstName: 'John', lastName: 'Doe' }
name.firstName= "Aadesh";
name.age22;
console.log(name); { firstName: 'Aadesh', lastName: 'Bakale', age: 22 }
```

Surprise surprise!!!Why has this happened?? Lets dive into the exact reason. It's because const keyword doesn't create immutable variables,but it creates immutable bindings i.e, the variable identifier cannot be reassigned but when the content is an object,and hence its properties can be updated or even new properties can be added.

####var

"var" keyword creates a variable that can be scoped globally.

```js
var items = 15
console.log(items) //15
```

####Conclusion

Highlight points:

1. let :
   * block scoped.
   * can be updated but cannot be redeclared.

1. const:
   * block scoped,
   * it is immutable.

1. var : 
   * can be scoped globally,
   * can be redeclared,

Thank you for reading!!!
