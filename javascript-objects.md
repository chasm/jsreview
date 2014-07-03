# JavaScript Objects

We can create a new Object thusly:

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

Values can also be functions, so we can add a method to `obj` thusly:

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
> obj2.setHeightInCm(150);
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
> var a = new Animal(true, new Date(2010,1,1), "Spot");
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
> var s = new String("hola!");
> typeof s
  "object"
> var a = new Array();
> typeof a
  "object"
> var n = new Number(3);
> n.valueOf();
  3
> typeof n
  "object"
```

But there are primitives in JavaScript, too:

```sh
> var i = 7;
> typeof i
  "number"
> var t = true;
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

## More on custom objects

Now that we have an `Animal()` constructor, we can make unique instances of animals:

```sh
> var rover = new Animal(true, new Date(2010,3,1), "Rover");
  undefined
> var spot = new Animal(true, new Date(2010,1,3), "Spot");
  undefined
> rover.name
  "Rover"
> spot.name
  "Spot"
```

Sweet!
