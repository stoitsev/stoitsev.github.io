---
layout: post
title: "Handling NaN in JavaScript"
date: 2015-03-01 01:06:45 +0200
comments: true
categories: javascript
---

NaN is a special value representing something that is not a number. It may apear as a result of some illegal operation. For example:
```
var nanValue = 1 / 'a';
```
After this expression is evaluated, the value of the variable ```nanValue``` will be ```NaN```.

The fun starts here.

If you try to check the type of ```nanValue ``` like this:
```javascript
typeof a; // "number"
```
The result will be "number".

Also, if you try to compare NaN to NaN, here is what happens:
```javascript
nanValue === nanValue; // false
nanValue === NaN; // false
nanValue == NaN; // false
```
So, if you have to check for ```NaN``` value in some production code, you might write the following block:
```javascript
if (someValue !== NaN) {
	// do something;
}
```
This condition will be always true. No matter if ```someValue``` contains ```NaN``` or not.

The correct way to do this is to use the built in function [isNaN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN) or perform a special type of comparison.

The block of code, shown above, should be refactored like this:
```javascript
if (isNaN(someValue)) {
	// do something;
}
```

The second approach looks like this:
```javascript
if (someValue !== someValue) {
	// do something;
}
```
This comparison will be true, only if the value of ```someValue``` is ```NaN```.

In my humbe oppinion, the first approach is far more readable and should be prefered. 
