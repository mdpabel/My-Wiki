# Object Oriented Javascript

## Creating objects using object literal syntax

```js
const obj = {};
```

## Creating objects using the ‘new’ keyword and constructor function

```js
function Person() {};
const new Person()
```

#### new keyword

- create and empty object.
- set 'this' point to the object.
- return that object.

#### constructor

- every object has the constructor property.
- **the constructor references the function that used to contruct/create that object.**

#### functions are object

- because every function has property and methods just like any other object.

```js
function func() {}
console.log(func.name); // 'func'
```

#### Abstraction

```

```

#### Private properties

```js
function Student(name) {
  let age = 20; // local variable,
  this.name = name; // this refers to the object constructed by the Student constructor.
  // name is the property of that object
}

const student = new Student('MD');
student.name; // 'MD'
student.age; // undefined
// Out of the Student function age goes out of that function scope.
```

#### Getter and Setter

```js
function Student(name) {
  let age = 20;
  this.name = name;
  Object.defineProperty(this, 'age', {
    get: function () {
      return age;
    },
    set: function (x) {
      age = x;
    },
  });
}

const student = new Student('MD');
console.log(student.name); // MD
console.log(student.age); // 20
student.age = 22;
console.log(student.age); // 22
```

#### model of OOP

- classical
- prototypical

JavaScript isn't a classed-based langauge – it's is a prototype-based langauge.

## Prototype

- Enable an object to take on the propertied and methods of another object.
- **Prototype is essentially a parent of another object. It's looks like super class in classical oop mode.**
- Every object in js has a prototype.
- Every object has a constructor property which refers to the function that was used to create or construct that object.
- **EVery object in js directly or indirectly inherit from root Object and it doesn't have prototype parent.**
- **When we access a property or method on an object, JS engine first look for that property or method first on object itself, if it can't find it then it looks at the prototype for that object**

```js
const arr = [];
```

- Here the prototype of arr is Array, which is constructor of that arr.
- The prototype of Array is Object

```js
function Student() {}

const student1 = new Student();
const student2 = new Student();
```

- The prototype of student1 and student2 is Student. This prototype is the prototype for all Student object which are using the Student constructor.
- object created by the Student constructor will have the same prototype

## DON'T MODIFY OBJECTS YOU DON'T OWN

## Prototypical inheritance

```js
function Parent() {
  this.name = 'Parent';
}

Parent.prototype.greet = function () {
  console.log('Hello from ' + this.name);
};

const child = Object.create(Parent.prototype);

child.cry = function () {
  console.log('waaaaaahhhh!');
};

child.cry();
// waaaaaahhhh!

child.greet();
// hello from Parent

child.constructor;
// ƒ Parent() {
//   this.name = 'Parent';
// }

child.constructor.name;
// 'Parent'
```

- .greet is not defined on the child, so the engine goes up the prototype chain and finds .greet off the inherited from Parent.

###### We need to call Object.create in one of following ways for the prototype methods to be inherited:

```js
Object.create(Parent.prototype);
Object.create(new Parent(null));
Object.create(objLiteral);
```

###### Currently, child.constructor is pointing to the Parent, Whenever reset the prototype of an object, we should also need to reset the constructor.

```js
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;

child.constructor.name;
// 'Child'
```

#### Calling super constructor

```js
function Person(name) {
  this.name = name;
}

function Student(age, name) {
  Person.call(this, name);
  this.age = age;
}
```

## mutable and immutable objects

- A mutable object is an object whose state can be modified after it is created.
- An immutable object is an object whose state cannot be modified after it is created.

- In JavaScript, some built-in types (numbers, strings) are immutable, but custom objects are generally mutable.
- Some built-in immutable JavaScript objects are Math, Date.

#### Object Constant Properties

- By combining writable: false and configurable: false, you can essentially create a constant (cannot be changed, redefined or deleted) as an object property.

```js
let myObject = {};
Object.defineProperty(myObject, 'number', {
  value: 42,
  writable: false,
  configurable: false,
});
console.log(myObject.number); // 42
myObject.number = 43;
console.log(myObject.number); // 42
```

#### Prevent Extensions

- prevent an object from having new properties added to

```js
let myObject = {
  a: 2,
};

Object.preventExtensions(myObject);

myObject.b = 3;
myObject.b; // undefined
```

#### Seal

Object.seal() creates a "sealed" object, which means it takes an existing object and essentially calls Object.preventExtensions() on it, but also marks all its existing properties as configurable: false.
So, not only can you not add any more properties, but you also cannot reconfigure or delete any existing properties (though you can still modify their values).

#### Freeze

Object.freeze() creates a frozen object, which means it takes an existing object and essentially calls Object.seal() on it, but it also marks all "data accessor" properties as writable:false, so that their values cannot be changed.

This approach is the highest level of immutability that you can attain for an object itself, as it prevents any changes to the object or to any of its direct properties (though, as mentioned above, the contents of any referenced other objects are unaffected).

```js
let immutableObject = Object.freeze({});
```

Freezing an object does not allow new properties to be added to an object and prevents users from removing or altering the existing properties. Object.freeze() preserves the enumerability, configurability, writability and the prototype of the object. It returns the passed object and does not create a frozen copy.

### Method overriding

- To override method, Redefined the method on It's own constructor
