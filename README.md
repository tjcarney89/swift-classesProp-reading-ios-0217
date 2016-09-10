# Classes & Properties

![NameOfPerson](http://i.imgur.com/GAIzIs0.jpg?1)  

> People often say that motivation doesn't last. Well, neither does bathing--that's why we recommend it daily. -[Zig Ziglar](https://en.wikipedia.org/wiki/Zig_Ziglar)

## Overview

In this lesson we'll introdue properties, the attributes that separate an instance of a class from other instances. 

## Learning Objectives

* Create stored properties
* Create computed properties that are based on the values of other properties
* Explain the difference between constant and variable properties
* Explain the difference between constant and variable objects

## Properties

Think about a car for a second. A car is a fairly generic object: it's a thing you drive that has four wheels, a body, an engine, and some seats. (It has a lot of other stuff, too, like a transmission and brakes and stuff, but let's not get too hung up on details.) What, then, differentiates one car for another? When you're looking for your car in a parking lot, how can you pick out your car from the dozens or hundreds of others parked there, too?

Cars, like all things in real life, have certain attributes that let you tell the difference between them. Your car, for example, might be a green Toyota with four doors and a black interior. It also has other identifying items, like a license plate, that let you easily pick out _your_ car from another one. You wouldn't mistake a bright red two-door Lamborghini for your green Toyota!

In Swift, the attributes that separate an instance of a class from other instances are called _properties_. Properties are what drive instances of classes: If all instances were the same, you wouldn't need properties, but because objects have properties, you can create several instances of the same type (or class) that have different qualities.

There are several types of properties in Swift. Let's look at the first, simplest type of property: _stored properties_.

## Stored Properties

Consider a very boring class: a `Square`. Overall, all squares have the same parts: They have a point in 2D space, sides (which have length), an area. They're pretty easy to imagine and not too complex. But are all squares the same? No! They lie at different points in 2D space, and they are different sizes. Some are big, some are small. While we can represent the general idea of a square with a single class, squares still have unique properties that make them different from one another.

Take a look at this simple definition of a `Square` class (it appears in the playground file in this repo, so open that up to take a closer look). It has a property called `topLeftCorner` that defines where the top left corner is in 2D space. This property is a tuple consisting of two `Double`s (`(Double, Double)`). It also has a property called `lengthOfSide` (also a `Double`) that tracks the length of a side of the square. And really, that's all the information you need to know about a square.

```swift
class Square {
    let topLeftCorner: (Double, Double)
    let lengthOfSide: Double

    init(topLeftCorner: (Double, Double), lengthOfSide: Double) {
        self.topLeftCorner = topLeftCorner
        self.lengthOfSide = lengthOfSide
    }
}
```

Properties are declared just like you declare variables and constants, except they're declared inside the class. Unlike simple variable or constant definitions, you _must_ declare a type. Why? You're not immediately assigning a value to the property (remember, that happens in the initializer, or `init` method), so Swift can't _infer_ the type automatically.

The `Square` class also contains an initializer that takes the properties' parameters and assigns them to the property.

With this definition, you can create a lot of squares:

```swift
class Square {
    let topLeftCorner: (Double, Double)
    let lengthOfSide: Double

    init(topLeftCorner: (Double, Double), lengthOfSide: Double) {
        self.topLeftCorner = topLeftCorner
        self.lengthOfSide = lengthOfSide
    }
}

let square1 = Square(topLeftCorner: (0.0, 0.0), lengthOfSide: 10.0)
let square2 = Square(topLeftCorner: (5.5, 7.25), lengthOfSide: 9.5)
let square3 = Square(topLeftCorner: (14.5, 2.3), lengthOfSide: 7.8)
let square4 = Square(topLeftCorner: (23.4, 2.0), lengthOfSide: 12.1)
```

Accessing the properties is pretty simple: You type the variable or constant's name, followed by a dot (`.`), and then the property name:

```swift
square1.lengthOfSide
```

If you forget the name of the property, Xcode's autocomplete will be there to help you out:

![Property autocomplete](https://s3.amazonaws.com/learn-verified/xcode-property-autocomplete.png)

Stored properties are useful, but not terribly interesting or difficult to use. Let's take a look at the next type of property: _computed properties_.

## Computed Properties

Squares have another property that is interesting: area. Every square has an area. However, a square's area is not an independent property. It's actually based on the length of a side: A square's area is just the length of a side squared (get it?). You could add an `area` property to a square:

```swift
class Square {
    let topLeftCorner: (Double, Double)
    let lengthOfSide: Double
    let area: Double

    init(topLeftCorner: (Double, Double), lengthOfSide: Double, area: Double) {
        self.topLeftCorner = topLeftCorner
        self.lengthOfSide = lengthOfSide
        self.area = area
    }
}
```

But that wouldn't make much sense, because it would allow you to create nonsensical squares, like this:

```swift
let square4 = Square(topLeftCorner: (5.0, 5.0), lengthOfSide: 12.0, area: 4.0)
```

A square with sides of length 12.0 has an area of 144.0, but you created it with an area of just 4.0. That doesn't make sense! A square like that cannot exist!

You may have had an aha! moment right now: Why not just compute the area inside the `Square`'s initializer?

```swift
init(topLeftCorner: (Double, Double), lengthOfSide: Double) {
    self.topLeftCorner = topLeftCorner
    self.lengthOfSide = lengthOfSide
    self.area = lengthOfSide * lengthOfSide
}
```

That, of course, will totally work. In fact, it's so common, that Swift actually provides a shorthand for doing something similar.

Swift properties can be _computed_, meaning they appear to be properties, but the value is computed based on _other_ properties. Here's a definition of `Square` in which `area` is such a computed property:

```swift
class Square {
    let topLeftCorner: (Double, Double)
    let lengthOfSide: Double
    var area: Double {
        return lengthOfSide * lengthOfSide
    }

    init(topLeftCorner: (Double, Double), lengthOfSide: Double) {
        self.topLeftCorner = topLeftCorner
        self.lengthOfSide = lengthOfSide
    }
}
```

Pay special attention to the definition of the `area` property now. It looks almost like a function definition, doesn't it? It sort of is, too. Now instead of having a value, `area` _computes_ its value based on the value of `lengthOfSide`. It kind of looks like a function that returns `lengthOfSide * lengthOfSide`.

Another thing to note is that the `area` property is declared as a variable, using the `var` keyword, instead of a constant using `let`. Computed properties _must_ be declared using `var`, even though their values cannot be changed.

You can now refer to `area` just like a property, even though it is really more like a function that computes and returns a value. That means that you don't need parentheses when referring to it.

```swift
let square1 = Square(topLeftCorner: (10.0, 10.0), lengthOfSide: 12.0)
print("The area of the square is \(square1.area)")
// prints "The area of the square is 144"
```

## Mutability

So far, the stored properties you have created for the `Square` class have been _constant_, created using the `let` keyword. Even the computed property `area` is read-only: It is declared as `var` (which is required for computed properties), but you have no way to change it, and it is based on a constant property.

Constant properties can only be assigned values in two places: Where they are declared, or in a class's initializer. They cannot be changed at any other time.

A class that only contains constant properties is said to be _immutable_.

Recall what you learned about the `var` and `let` keywords. If a constant is created using `let`, it can never be changed, right?

```swift
let x = 5  // x can never be assigned a new value!
```

A variable created using `var` can be changed, though:

```swift
var y = 10
y = 15  // y can be changed to 15!
```

When working with objects, `let` and `var` behave nearly the same way. Once you assign an instance of a class to a `let` constant, that constant can never be assigned to another instance:

```swift
let square1 = Square(topLeftCorner: (0.0, 0.0), lengthOfSide: 1.0)
square1 = Square(topLeftCorner: (10.0, 10.0), lengthOfSide: 10.0)
// An error will occur on the second line.
// square1 cannot be assigned to another object!
```

However, if you assign an instance of a class to a `var` variable, you _can_ reassign it to another instance:

```swift
var square2 = Square(topleftCorner: (1.0, 1.0), lengthOfSide: 9.0)
square2 = Square(topLeftCorner: (2.0, 3.0), lengthOfSide: 3.5)
// This is fine!
```

This is because you're not _changing_—or _mutating_—the instance of a class; you're just assigning `square2` to a new instance.

The same applies to the `Square` class's properties: Because they were declared as constants, you cannot change them after an instance of the class has been created:

```swift
var square3 = Square(topLeftCorner: (10.0, 9.8), lengthOfSide: 3.2)
square3.lengthOfSide = 3.4  // This causes an error!
```

This is why the `Square` class is called an _immutable_ class.

On the other hand, if you declared the constants using the `var` keyword, like this:

```swift
class Square {
    var topLeftCorner: (Double, Double)
    var lengthOfSide: Double
    var area: Double {
        return lengthOfSide * lengthOfSide
    }

    init(topLeftCorner: (Double, Double), lengthOfSide: Double) {
        self.topLeftCorner = topLeftCorner
        self.lengthOfSide = lengthOfSide
    }
}
```

You could, in fact, change the properties from outside the class:

```swift
var square4 = Square(topLeftCorner: (8.0, 7.0), lengthOfSide: 15.0)
square4.lengthOfSide = 9.7  // This is okay!
```

And since `area` is a computed property, it would even print the correct area after `lengthOfSide` is changed:

```swift
var square4 = Square(topLeftCorner: (8.0, 7.0), lengthOfSide: 15.0)
print("Area is \(square4.area)")
// Prints "Area is 225.0"
square4.lengthOfSide = 9.7
print("Area is \(square4.area)")
// Prints "Area is 94.09"
```

One thing to note: If you declare a constant (using `let`), the properties of the object _can_ be changed, but only if those properties were declared as variables. For example, this code is valid:

```swift
let square5 = Square(topLeftCorner: (10.0, 10.0), lengthOfSide: 6.0)
square5.lengthOfSide = 4.0 
```

Declaring `square5` with the `let` keyword, thus making it a constant is only _immutable_ in the sense that we can't have `square5` equal another instance of Square, meaning the following code is invalid:

```swift
square5 = Square(topLeftCorner: (99.0, 99.0), lengthOfSide: 15.0) // invalid code
```

Here trying to assign a new instance of Square to our `square5` constant which is not allowed, this code will generate an error. We can though change any instance properties on `square5` that are marked with the `var` keyword thus making them variables even though `square5` itself was marked as a constant.

## Setting Computed Properties

In the previous examples, the computed property `area` was read-only; it could not be changed, only _read_. You can also create computed properties that can be changed, and can change other variables.

It's easy to compute the area of a square based on the length of side. Of course, the reverse is also true: Given an area, it's easy to compute the length of a side, as well. The length of a side is simply the square root on the area.

Here is an example of a `Square2` class which allows the area to be set directly:

```swift
class Square2 {
    var topLeftCorner: (Double, Double)
    var lengthOfSide: Double
    var area: Double {
        get {
            return lengthOfSide * lengthOfSide
        }
        set {
            lengthOfSide = sqrt(newValue)
        }
    }

    init(topLeftCorner: (Double, Double), lengthOfSide: Double) {
        self.topLeftCorner = topLeftCorner
        self.lengthOfSide = lengthOfSide
    }
}
```

Notice how the computed property is defined. As before, it opens with a set of curly brackets. However, this time, there are two keywords, `get` and `set`. These each have a set of curly braces as well. Inside `get`'s curly braces is the previous definition of `area`: `return lengthOfSide * lengthOfSide`. However, the code inside `set`'s curly braces is new: `lengthOfSide = sqrt(newValue)`.

As you can guess, the code inside `get` is called when _reading_ the `area` property. The code inside `set` is called when _writing_, or assigning, the area property. It simply sets `lengthOfSide` to the square root of the new value (using the built-in `sqrt()` function). Notice that the value being assigned is not explicitly listed as a parameter; rather, the value is referred to as `newValue` inside the `set` code block.

You can verify that this new computed property works as expected:

```swift
var square6 = Square2(topLeftCorner: (10.0, 10.0), lengthOfSide: 10.0)
print("Area is 100.0? \(square6.area)")
// Prints "Area is 100.0? 100.0"
square6.area = 144.0
print("Length of side is 12.0? \(square6.lengthOfSide)")
// Prints "Length of side is 12.0? 12.0
```

That's all the important stuff you need to know about properties. These are an integral part of Swift programming, so you'll be working with them a lot more in future lessons.

<a href='https://learn.co/lessons/Classes-Prop' data-visibility='hidden'>View this lesson on Learn.co</a>

<p class='util--hide'>View <a href='https://learn.co/lessons/swift-classesProp-reading'>Classes and Properties</a> on Learn.co and start learning to code for free.</p>
