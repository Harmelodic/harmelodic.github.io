# Double-not with Truthy/Falsy values - JavaScript

> Originally published: 05 October 2018

## Truthy/Falsy

In JavaScript, not only do we have normal boolean (true/false) values, but we also have the idea of "Truthy" and "Falsy"
values.  
These are values that may not boolean, but will still resolve a conditional statement.

All the Falsy values are:

```js
if (false) {
}
if (null) {
}
if (undefined) {
}
if (0) {
}
if (NaN) {
}
if ('') {
}
if ("") {
}
if (document.all) {
}
```

Any value that isn't one of the above, is Truthy. For example:

```js
if ({}) {
}
if ("badger") {
}
if (["Amy", "Steve", "Geoff"]) {
}
if (new Date()) {
}
// etc.
```

You can read more about Truthy and Falsy on MDN:

- https://developer.mozilla.org/en-US/docs/Glossary/Falsy
- https://developer.mozilla.org/en-US/docs/Glossary/Truthy

## Not and Double-Not

Most programmers are aware of the "not" logical operator: `!`; which flips the boolean output of a conditional
statement. For example:

```js
let something = "Badger";

if (!(something === "Steve")) {
}
// This will resolve true, 
// as `something === "Steve"` is false, 
// and the `!` flips it to true.
```

But a lot of programmers aren't aware that you can chain these logical operators:

```js
let something = "Badger";

if (!!(something === "Steve")) {
}
// This will resolve false, 
// as `something === "Steve"` is false, 
// and the `!` flips it to true, 
// and the second `!` flips it to false.
```

## Combining Truthy/Falsy with the Double-Not

Taking the above 2 concepts, we can combine them and create some clean conditional statements that convert Truthy/Falsy
falues into actual boolean values.

```js
let something = "Badger";

if (!!something) {
}
// Resolves to true, 
// as "Badger" is Truthy, 
// which flips to a boolean false, 
// which flips to boolean true.
```

## Why is this useful?

If you're building an application and you're passing variables around, sometimes you want to check if something actually
has a value. This is common for choosing whether to render something, performing a validation check, or whether to run
some code using a particular value.  
However, we often pass these variables to external functions & components to perform this logic, and passing in the
value of the variable can expose data to a function that shouldn't have it, or can cause quirky errors (this happens a
lot when things resolve to Truthy because the code is working with an non-boolean value).

So, if you want to anonymise and solidify these values, whilst also passing something useful to a function, the
double-not combined with truthy/falsy is a clean way to do it.
