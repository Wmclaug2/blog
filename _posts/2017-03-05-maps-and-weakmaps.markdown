---
layout: post
title:  "Maps and Weakmaps"
date:   2017-03-05 14:55:40 -0500
categories: datastructures javascript
---
In any programming language you are going to run into data structures. Without data, the actions of a program wouldn’t be able to do much. Conditionals would mostly be out of the window without having information to compare. 

In ECMAScript 2015, new data structures were released for use including Maps, Sets, WeakMaps and WeakSets. Today we will be looking primarily at the Map and WeakMap objects, what they can do to help you better organize your data and how to use them in your programming.

# What is a Map?

The Map object (not to be confused with the `map()` method) is a simple key/value pair map that can use any value, object or primitive, as a key or a value.

#### Syntax – Initializing a new Map 

{% highlight javascript %}
let myMap = new Map();
{% endhighlight %}

Any array or other iterable object that has key-value pairs (2 element arrays) added between the parentheses when calling a new Map are added to the Map. A Map iterates through its elements by insertion order. 

#### Why use a Map over an Object? Objects use key-value pairs too, right?

They certainly do! There is a lot of similarities between Maps and Objects in the ability to set key-value pairs, retrieve the values, delete keys, etc. Therefore, Objects has often been used as Maps. All the same, there are some important differences as of ECMAScript 2015 that give Maps an advantage – 

1. Objects have a prototype which results in default keys being placed into your Map.
2. The keys of an Object are Strings and Symbols, where a Map can use any key-value pair, with an object, function, primitive, etc. as either value.
3. Object size must be determined manually while Maps have a size property that allow for quick determination of Map size. 

Despite their advantages, Maps should not replace Objects in every instance. Maps are only useful for collections of information. Per MDN (<i>[Mozilla Developer Network](https://developer.mozilla.org)</i>), you should ask yourself the following questions to determine if a Map is right for your use case – 

* Are keys usually unknown until run time, do you need to look them up dynamically?
* Do all values have the same type, and can be used interchangeably?
* Do you need keys that aren't strings?
* Are key-value pairs often added or removed?
* Do you have an arbitrary (easily changing) amount of key-value pairs?
* Is the collection iterated?

If the answer to any of these questions is ‘yes’, then you will probably want a Map. On the other hand, if you have a fixed amount of keys, operate on them each one-by-one and distinguish between their usages, then an object is a better fit for your use.

#### Methods and Properties

`Map.set(key, value)` is used to set a key-value pair within a Map, while `Map.get(key)` returns the value associated with the given key. `Map.delete(key)` can be used to delete a key-value pair by providing the targeted key. 

To find the size of a Map, you can use the Map’s `.size` property. 

{% highlight javascript %}
let candies = new Map();
candies.set('mint', 'Altoids');
candies.set('chocolate', 'Snickers');
candies.set('best', 'Twix');
console.log(candies.size); //returns 3
{% endhighlight %}

Maps are often used with `for…of` loops to iterate through their elements like so.

{% highlight javascript %}
//initialize a new Map
let favoriteTypesOfRestaurants = new Map();
favoriteTypesOfRestaurants.set('Mexican', 'Casa Grande'); 
favoriteTypesOfRestaurants.set('Chinese', 'Peter Chang'); 
favoriteTypesOfRestaurants.set('Italian', 'La Grotta'); 

//array to loop through all keys and values in declared Map
for (let [key, value] of favoriteTypesOfRestaurants){
	console.log(key+' = '+ value);
}

// Mexican = Casa Grande
// Chinese = Peter Chang
// Italian = La Grotta
{% endhighlight %}

They can also be used with the `forEach()` method as well –

{% highlight javascript %}
let users = new Map();
users.set('user1', 'Joe');
users.set('user2', 'Barbara');
users.set('user3', 'Felicity');

users.forEach(function(value, key){
	console.log(key + ' = ' + value);
});

//user1 = Joe
//user2 = Barbara
//user3 = Felicity
{% endhighlight %}

Notice that the callback function for the `forEach()` method flips the arguments to have value first, since it is the first argument returned by `forEach()`.

To find all the keys in a Map, one can call the `keys()` method on the map to return a new Iterator object that contains the keys for each pair in insertion order.

{% highlight javascript %}
let books = new Map();
books.set('Magic', 'Harry Potter');
books.set('Fantasy', 'The Hobbit' );
books.set('Sci-Fi', 'The Isle of the Dead');

var mapIter = books.keys();
console.log(mapIter.next().value); //returns "Magic"
console.log(mapIter.next().value); //returns "Fantasy”
console.log(mapIter.next().value); //returns "Sci-fi"
{%endhighlight%}

On the opposing side, one can use the `values()` method to return a new Iterator object that contains the values for each pair in insertion order.

To clear out a Map, use the `clear()` method to remove all key-value pairs from the Map object.

<i>[Learn more about maps here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)</i>

<b>Whew…. That was a lot. Thankfully, WeakMaps are very similar to Maps with just a few differences. </b>

# What is a WeakMap?

Unlike a Map where any value can be used as a part of the key-value pair, WeakMaps only accept objects as keys. The syntax for constructing a WeakMap is the same as a Map, however the methods available to a WeakMap are far less than those available to a Map. So if a WeakMap is just that, a weaker version of a Map, why would we want to use them?

#### Why WeakMaps? If they are just lame Maps, why not just use Maps?

Previously in JavaScript, experienced programmers would implement a similar idea using two arrays, one holding the keys and one holding the values. This was not the most efficient method of implementation and resulted in memory leak issues. With the Maps being manually created, the key array would keep references to the key objects which prevented them from being ‘garbage collected’ (for more on ‘garbage collection’ and memory management, see [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)) . With WeakMaps, the references to the key objects are held ‘weakly’, allowing them to be garbage collected when there was no further reference to the object being used. 

Due to these ‘weak’ references, WeakMap keys are not enumerable (no way to get a list of the keys). Therefore, if you need to use a list of your keys, you would be better off using a Map. One can see from these differences that WeakMaps are primarily used when memory efficiency is a concern.

#### Methods and Properties

As previously stated, WeakMaps have some methods in common with Maps, but are generally lacking in comparison. 

The 4 main methods for WeakMaps are – 

1. `delete()` – removes any value associated with a key and returns false
2. `get(key)` – returns the value associated with the key
3. `has(key)` – returns a Boolean based on whether a value has been associated to the key
4. `set(key, value)` – sets the value for the key in the WeakMap object and returns the WeakMap object. Key must be an object.

#### Use case for WeakMap 

Let's say I'm using an API that gives me a certain object:

{% highlight javascript %}
var obj = getObjectFromLibrary();
Now, I have a method that uses the object:
function useObj(obj){
   console.log(obj);
}
{% endhighlight %}

I want to keep track of how many times the method was called with a certain object and report if it happens more than `n` times. Naively one would think to use a Map:

{% highlight javascript %}
var map = new Map(); // maps can have object keys
function useObj(obj){
    doSomethingWith(obj);
    var called = map.get(obj) || 0;
    called++; // called one more time
    if(called > 10) report(); // Report called more than 10 times
    map.set(obj, called);
}
{% endhighlight %}

This works, but it has a memory leak - we now keep track of every single library object passed to the function which keeps the library objects from ever being garbage collected. Instead - we can use a WeakMap:

{% highlight javascript %}
var map = new WeakMap(); // create a weak map
function useObj(obj){
    doSomethingWith(obj);
    var called = map.get(obj) || 0;
    called++; // called one more time
    if(called > 10) report(); // Report called more than 10 times
    map.set(obj, called);
}
{% endhighlight %}

Some use cases that would otherwise cause a memory leak and are enabled by WeakMaps include:

* Keeping private data about a specific object and only giving access to it to people with a reference to the Map. A more ad-hoc approach is coming with the private-symbols proposal but that's a long time from now.
* Keeping data about library objects without changing them or incurring overhead.
* Keeping data about a small set of objects where many objects of the type exists to not incur problems with hidden classes JS engines use for objects of the same type.
* Keeping data about host objects like DOM nodes in the browser.
* Adding a capability to an object from the outside (like the event emitter example in the other answer).

Examples from Stack Overflow user [Benjamin Gruenbaum](http://stackoverflow.com/questions/29413222/what-are-the-actual-uses-of-es6-weakmap)
