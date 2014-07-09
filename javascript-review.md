# JavaScript Review

## Expressions vs. statements

In JavaScript, an *expression* returns a value:

```sh
> 5
  5
> 5 + 7
  12
> Math.sqrt(16)
  4
> 1 === 1.0 ? "yes" : "no"
  'yes'
```

A *statement* may or may not return a value. So all expressions are statements, but not all statements are expressions. Statements are generally used to perform an action.

An example of a statement is the `if` statement:

```js
var x;
if (1 === 1.0) {
  x = "yes";
} else {
  x = "no";
}
```

The `if` statement above does not return a value, so we must assign the value inside the branches. The *conditional operator*, also called the *ternary operator*, **does** return a value, so we could have written the above thus:

```js
var x = 1 === 1.0 ? "yes" : "no";
```

## Parameters vs. arguments

Most JavaScript programmers (ore programmers in general) use the terms "parameters" and "arguments" interchangeably; however, they are not the same thing.

The parameters are the *specification* of inputs to a function. For example, in the following code, the parameters are `x` and `y`:

```js
var show = function(x, y) { console.log(x, y); };
```

When we call this function, we will pass in specific *arguments*:

```sh
> show(1, 2)
  1 2
  undefined
```

In this situation, `1` and `2` are the *arguments* to the function with *parameters* `x` and `y`, thus `1` is assigned to `x` and `2` to `y`.

## Objects

We can create a new Object thus:

```js
var obj = new Object();
```

or

```js
var obj2 = {};
```

We can then add key-value pairs to the object:

```js
obj.name = "Jeff";
obj.height = 167;
obj.male = true;
```

or

```js
obj2["name"] = "Sally";
obj2["height"] = 170;
obj2["male"] = false;
```

This gives us:

```sh
  Object {name: "Jeff", height: 167, male: true}
  Object {name: "Sally", height: 170, male: false}
```

We can see that we end up in the same place, namely, with an object with key-value pairs.

Values can also be functions, so we can add a method to `obj` thus:

```js
obj.getSex = function() { return this.male ? "Male" : "Female"; };
```

This yields:

```sh
> obj.getSex()
  "Male"
```

When a function is assigned to an object property like this, we call that function a *method*. Then intent of the method is to operate on the object that contains it. Here's another example:

```js
obj2["setHeightInCm"] = function(cm) { return this.height = cm; };
```

So now:

```sh
> obj2.setHeightInCm(150)
  150
> obj2.height
  150
```

We can also create an object using a constructor function. The constructor acts as a kind of "class" in JavaScript:

```js
var Animal = function(isAlive, bornOn, name) {
  this.isAlive = isAlive;
  this.bornOn = bornOn;
  this.name = name;
  this.setBornOn = function(bornOn) { this.bornOn = bornOn; };
}
```

We can think of the Animal constructor as a sort of factory for producing Animal objects. By doing it this way, we also set up *prototypical inheritance* for our Animal objects.

According to Douglas Crockford:

> You make prototype objects, and then ... make new instances. Objects are mutable in JavaScript, so we can augment the new instances, giving them new fields and methods. These can then act as prototypes for even newer objects. We don't need classes to make lots of similar objects ... objects inherit from objects. What could be more object oriented than that?

The role of a constructor function is to create multiple objects that share certain qualities and behaviors.

The constructor function is just a function until we invoke it with the `new` keyword. When this happens, JavaScript treats the function as special by setting the value of `this` for the function to the new object that is being constructed, and returns the newly created object (i.e. `this`) by default instead of the value `undefined`.

In other words, when we call `Animal()` with the `new` keyword, `this` is set to a new object (`this = {}`) and that new object is returned from the function call. We can then set properties on that object using the `this` keyword.

We can see that the type of the returned object is "object":

```sh
> var a = new Animal(true, new Date(2010,1,1), "Spot")
> typeof a
  "object"
> a.constructor
  function (isAlive, bornOn, name) {
    this.isAlive = isAlive;
    this.bornOn = bornOn;
    this.name = name;
    this.setBornOn = function(bornOn) { this.bornOn = bornOn; };
  }
```

*Everything* created with the `new` operator is a type of "object":

```sh
> var s = new String("hola!")
  undefined
> typeof s
  "object"
> var a = new Array()
  undefined
> typeof a
  "object"
> var n = new Number(3)
  undefined
> n.valueOf()
  3
> typeof n
  "object"
```

But there are **primitives** in JavaScript, too. (Primitives are the simplest form of a type, considered irreducible to simpler types.)

Complex types (objects) are types that contain primitive or other complex types, such as `Array()`, `Date()`, and `RegEx()` (for regular expressions). Complex types are sometimes called "composite" types for this reason.

```sh
> var i = 7
  undefined
> typeof i
  "number"
> var t = true
  undefined
> typeof t
  "boolean"
```

When we use the `new` keyword with a constructor function, we *instantiate* an object and we call that new object an *instance* of the constructor. So `new Animal()` instantiates an `Animal` object which is an instance of the `Animal` constructor.

This is the same terminology we use in a class-based language (such as CoffeeScript): we *instantiate* a new *instance* of a class. The JavaScript constructor function is the JS equivalent of a class.

JavaScript comes with 9 native object constructors:

  * Number()
  * String()
  * Boolean()
  * Object()
  * Array()
  * Function()
  * Date()
  * RegExp()
  * Error()

There are also string, number, and boolean primitive values.

There is also a `Math` object, but you can't call `new` on it. You use it for built-in math properties and methods, e.g., `Math.PI` or `Math.abs(-5)` (absolute value: returns `5`).

Using `Number()`, `String()`, or `Boolean()` is what we call "boxing primitives." We can return a complex object wrapped around the primitive value, or the primitive value itself:

```js
var boxedBool = new Boolean(true); // Boxed
var bool = true; // Primitive

var boxedStr = new String("hello"); // Boxed
var str = "hello"; // Primitive
```

```sh
> boxedBool
  Boolean {}
> typeof boxedBool
  "object"
> boxedBool.valueOf()
  true

> bool
  true
> typeof bool
  "boolean"

> boxedStr
  String {0: "h", 1: "e", 2: "l", 3: "l", 4: "o", length: 5}
> typeof boxedStr
  "object"
> boxedStr.valueOf()
  "hello"
> boxedStr.length
  5
> boxedStr[1]
  "e"

> str
  "hello"
> typeof str
  "string"
```

Note that except for the primitive types (number, boolean, and string), the literal methods for object creation work exactly the same as using `new` with the object constructor:

```js
var oArr = new Array('a', 'b', 'c'); // With `new`
var lArr = ['a','b','c']; // Literal
```

```sh
> oArr
  ["a", "b", "c"]
> lArr
  ["a", "b", "c"]

> typeof oArr
  "object"
> typeof lArr
  "object"
```

## By value or by reference

Primitive values are copied. With objects, a pointer to the object is copied rather than the object itself. So with primitive values we can do this:

```sh
> var x = 1
  undefined
> var y = x
  undefined
> x = 2
  2
> y
  1
```

So the *value* of `x` was copied to `y`. When we later changed the value of `x`, the value of `y` remained unchanged.

But compare this to what happens when we try the same thing with an object:

```sh
> var o = { x: 1 }
  undefined
> var p = o
  undefined
> o.x = 2
  2
> p.x
  2
```

Note that when we changed `o.x`, then `p.x` changed as well. That is because `o` and `p` point to *the same object.* When we set `var p = o;` only the *pointer* to that object was copied, not the object itself.

What happens when we pass objects into functions?

```js
var doubler = function(arr, prim) {
  var len = arr.length;
  for (var i = 0; i < len; ++i) { arr[i] *= 2; }
  prim = prim * 2;
  return prim;
}
```

Now look:

```sh
> var a = [1, 2, 3]
  undefined
> var p = 7
  undefined
> doubler(a, p)
  14         // doubled inside doubler
> a
  [2, 4, 6]  // doubled outside doubler
> p
  7          // unchanged outside doubler
```

Our array `a` changed *outside* the call to `doubler`, but while primitive `p` was doubled inside `doubler` (as shown by the return value), the value of `p` *outside* the doubler call was unchanged.

This is because the array object `a` was **passed by reference**, while the primitive `p` was **passed by value** (copied).

## Strict comparison operators

In JavaScript, `===` and `!==` are *strict* comparison operators. This means:

* Two strings are *strictly* equal when they have the same sequence of characters exactly
* Two numbers are *strictly* equal when they are numerically equal (have the same number value)
  * NaN (not a number) is not equal to anything, including NaN!
  * Positive and negative zeros are equal to one another
* Two Boolean operands are strictly equal if both are true or both are false
* Two objects are strictly equal *only if they reference the same Object*
* Null and Undefined types not strictly equal (but will return true if compared with `==`)

## More on custom objects

Now that we have an `Animal()` constructor, we can make unique instances of animals:

```sh
> var rover = new Animal(true, new Date(2010,3,1), "Rover")
  undefined
> var spot = new Animal(false, new Date(2010,1,3), "Spot")
  undefined
> rover.name
  "Rover"
> spot.name
  "Spot"
```

Sweet! But what if I want to add a new method to both `rover` and `spot`?

We can do so by adding our methods to the `prototype` for the `Animal` constructor:

```js
Animal.prototype.kill = function() { this.isAlive = false; };
Animal.prototype.reanimate = function() { this.isAlive = true; };
```

Now let&apos;s see what happens with our previously defined animals, Rover and Spot:

```sh
> rover.isAlive
  true
> spot.isAlive
  false
> rover.kill()
  undefined
> spot.reanimate()
  undefined
> rover.isAlive
  false
> spot.isAlive
  true
```

We can even add functionality to built-in objects. For example, we could add a `map` method that takes a function and applies it to each member of the array, and then returns a new array:

```js
Array.prototype.mapper = function(f) {
  var len = this.length;
  var a = [];
  for (var i = 0; i < len; ++i) {
    a[i] = f(this[i]);
  }
  return a;
}
```

Now we can test it:

```sh
> var a = [1, 2, 3]
  undefined
> var dbl = function(i) { return i * 2; }
  undefined
> a.mapper(dbl)
  [2, 4, 6]
```

But we should generally avoid this sort of modification of built-in objects. That said, there is a library&mdash;called *[Sugar](http://sugarjs.com/)*&mdash;that does just this. [Sugar provides a good overview of the pros and cons of modifying native JavaScript objects](http://sugarjs.com/native). Take a look.

## Use of `instanceof`

If we use a constructor function to create a new Object, then when we check its type with `typeof` we find that it is of type "object". How can we find out whether it was constructed with the constructor for, say, `Animal()` rather than as a generic `Object()`?

For this, we have `instanceof`:

```sh
> var a = new Animal(false, null, "Marv")
  undefined
> a instanceof Object
  true
> a instanceof Animal
  true
> a instanceof Array
  false
```

Note that an Animal is also a type of Object (every complex type is), so `instanceof` also returns true for `Object()`.

But be careful with primitives! They are not instances of their respective constructors even though the constructor is used to make them:

```sh
> var p = 5
  undefined
> p
  5
> p instanceof Number
  false

> var o = new Number(5)
  undefined
> o
  Number {}
> o instanceof Number
  true
```

But it&apos;s the `new` keyword that creates the `Number()` object rather than a primitive:

```sh
> var q = Number(5)
  undefined
> q
  5
> q instanceof Number
  false
```

That is because the `new` keyword *binds* `this` in the  constructor function to the constructor itself rather than to the calling context.

## Everything is mutable

Pretty much everything in JavaScript is mutable, so I can also mutate individual objects. For example, I can take my `rover` Animal from above and add a `playDead` method:

```sh
> rover.playDead = function() { console.log("Urk!"); }
  undefined
```

Now I can call this method on `rover`, but not on `spot`. Had I added the method to the `Animal()` prototype instead, then it would be available on **both** animals:

```sh
> rover.playDead()
  Urk!
  undefined
> spot.playDead()
  TypeError: undefined is not a function
```

Of course, we can do this with an instance of a native object as well:

```sh
> var a = [1, 2, 3]
  undefined
> a.mapper = function(f) {
    var len = this.length;
    var a = [];
    for (var i = 0; i < len; ++i) {
      a[i] = f(this[i]);
    }
    return a;
  }
  function (f) {
    var len = this.length;
    var a = [];
    for (var i = 0; i < len; ++i) {
      a[i] = f(this[i]);
    }
    return a;
  }
> function dbl(i) { return i * 2; }
  undefined
> a.mapper(dbl)
  [4, 5, 6]

> var b = [7, 8, 9]
  undefined
> b.mapper(dbl)
  TypeError: undefined is not a function
```

## Resolving object properties

When we call an object property using either dot or bracket notation, how does the JavaScript interpreter handle it?

As we discussed above, each object has a prototype. We also have inheritance, with all objects inheriting from the base `Object()`. So every type of object, e.g., `Array()`, has both its own prototype (`Array.prototype`) and the Object prototype (`Object.prototype`).

When we call a property on an object in JavaScript, the interpreter first looks for that property on the instance itself. If the property does not exist on the instance, then it looks up the tree to the prototype for that instance. If the property does not exist there either, then the interpreter will continue up the prototype chain until it reaches the `Object.prototype` itself.

When all possibilities are exhausted, the interpreter will throw an error.

This means that if I add a property to an instance of an array, and that property has the same name as a property in the Array prototype, then my new property will override the prototype property.

Similarly, properties in the Array prototype with the same name as properties in the Object prototype will override the Object prototype properties.

This allows us to customize and reimplement functionality in when using inheritance.

For example, the `Array()` object includes a `map` method:

```sh
> var a = new Array(1, 2, 3)
  undefined
> a.map
  function map() { [native code] }
> a.map(function(i) { return i * 2; })
  [2, 4, 6]
> a
  [1, 2, 3]
```

If we create a property called `map` on our array `a`, then we can no longer access the prototype method:

```sh
> a.map = "hi!"
  "hi!"
> a.map(function(i) { return i * 2; })
  TypeError: string is not a function
```

Bummer. Be careful that you don&apos;t do this by mistake!

What if we wanted to *adapt* a method? For example, suppose for some reason we wanted to update the `map` method so that it printed out a message before completing the mapping. We could do this:

```js

```

## A few more things about objects (before we get into the hard stuff)

Objects can contain other types of Objects, which can contain other types of Objects, and so on to an arbitrary depth.

For example, we can have a hash of hashes of hashes:

```js
var colors = {
  red: {
    tints: {
      pink: "#FFC0CB",
      salmon: "#FF91A4",
      coral: "#F88379"
    },
    shades: {
      scarlet: "#FF2400",
      desire: "#EA3C53",
      lust: "#E62020"
    }
  },
  green: {
    tints: {
      artichoke: "#8F9779",
      asparagus: "#87A96B",
      fern: "#71BC78"
    },
    shades: {
      pine: "#01796F",
      teal: "#008080",
      army: "#4B5320"
    }
  }
};
```

We can access the properties of our `colors` hash using dot or bracket notation (or a combination thereof):

```sh
> colors.red['tints']
  Object {pink: "#FFC0CB", salmon: "#FF91A4", coral: "#F88379"}
> colors["red"].tints.pink
  "#FFC0CB"
> colors["green"]["shades"]["teal"]
  "#008080"
> colors.green.tints.fern
  "#71BC78"
```

We can have an array of arrays (called a "multidimensional array"):

```js
var table = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];
```

We can access via bracket notation by index:

```sh
> var row = 1
  undefined
> var col = 2
  undefined
> table[row][col]
  6
> table[2][1]
  8
```

And, of course, we can mix the two:

```js
var friends = [
  { name: "Bob" },
  { name: "Ted" },
  { name: "Carol" },
  { name: "Alice" }
]
```

And access them in typical ways:

```sh
> friends[1]
  Object {name: "Ted"}
> friends['1']
  Object {name: "Ted"}
> friends[0].name
  "Bob"
> friends["3"].name
  "Alice"
```
