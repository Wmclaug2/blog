---
layout: post
title:  "Undefined, Null and NaN"
date:   2017-03-01 12:55:40 -0500
categories: primitives javascript
---
<i>This post has been written using information found in the [ECMAScript 2016 Language Specification](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf)</i>.

### Undefined
#### <i>‘Undefined is a primitive value used when a variable has not been assigned a value.’</i>

The global `undefined` property represents the primitive value for undefined and is included in JavaScript’s primitive types. Undefined values have no type and will return undefined if used with the `typeof` operator. It basically means ‘there is no property assigned to the called variable’. This also applies to functions and their arguments if the variable being used does not have an assigned value. 
	
Commonly seen when:
* Declaring a variable without assigning a value.

{%highlight javascript %}
var x;
alert(x); //returns undefined
{%endhighlight%}

* A function gets fewer arguments than declared

{%highlight javascript%}
function test(x,y,z){
    console.log(x,y,z);
}
test(x,y); //returns undefined
{%endhighlight%}

* Accessing a non-existent object property

{%highlight javascript%}
let test = {
    name:'Roger',
    age: 32,
}
console.log(test.height); //returns undefined
{%endhighlight%}

### Null 
#### <i>‘A primitive value that represent the intentional absence of any object value’</i>

Null is an assignment value. It must be explictly assigned to a variable to represent no value  - is viewed as an object.

{%highlight javascript%}
var test = null;
alert(test); //shows null
alert(typeof test) //shows object
{%endhighlight%}

### NaN
#### <i>‘Number value that is an [IEEE 754-2008 “Not-a-Number”](http://ieeexplore.ieee.org/document/30711/) value’</i>

Now…. What is an [‘IEEE 754-2008’](http://ieeexplore.ieee.org/document/30711/)….?

I am glad you asked. 

The Institute of Electrical and Electronics Engineers, better known as IEEE, is a professional association that does many useful things in the engineering community, sets up conferences, creates publications, and in this case creates technical standards. The IEEE 754-2008 Standard surrounds floating point computation and was established in 1985. This standard was necessary to address the inherent issues found with attempting to use floating point numbers reliably. If you are not aware of issues caused by using floating point numbers, a good example is the [failure of the Patriot Missile in 1991](http://www.math.umn.edu/~arnold/disasters/patriot.html).

 In JavaScript, `NaN` is a property of the global object that is often returned when a mathematical operation fails. `NaN` compares unequal to any other value, including itself. Since this is true, the method `isNaN()` allows comparison of a value to see if it is `NaN`.  

For example – 
{% highlight javascript%}
let failTest = Math.sqrt(-1);
console.log(failTest);
{%endhighlight%}