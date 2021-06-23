---
layout: post
title: Fundamental Object-Oriented Design Patterns and their uses
subtitle: A chronicling of ways to solve common patterns in Object-Oriented Design.
comments: true
---
## Fundamental Design Patterns

First of all, what is a __design pattern__? Why do we care about them?

- Simply put, a __design pattern__ is a way we, as developers, have streamlined solutions to common problems, or how we, as developers, can work around  a shortfall of our programming language.

Now, there are a ton of politics surrounding which ones to use and which ones to avoid, but I won't go into those. Here are some that I've learned about, and how we can use them:

### Builder pattern

The __Builder pattern__ is a *creational* design pattern, one which allows you to make a new object. The __Builder pattern__ is used when you want to create an object that has a __LOT__ of fields, most of which aren't always initialized.

Let's explain this with an example, which it a nice way to wrap your head around it. Suppose we have to model a Nutrition Facts label. You have a ton of different data in those labels, like calories, calories from fat, total carbs, vitamins A - Z, and so on. But also, if you look, a fair few of those are zero, since the label is __comprehensive__ and not __optimized__. So, you probably want to create a bunch of fields, and a constructor that sets up all of them, like below. (For the sake of brevity, I'm only working with 4 fields).

``` java
public class NutritionFacts {
    private int calories;
    private int transFat;
    private int vitaminA;
    private int sodium;

    public NutritionFacts(int calories, int transFat, int vitaminA, int sodium) {
        this.calories = calories;
        this.transFat = transFat;
        this.vitaminA = vitaminA;
        this.sodium = sodium;
    }
}
```

Rather basic, right! But, what if we don't have a value for 3 of these. What if, instead of just the 4 fields, we had 40? Do you really want to enter all the 0's required for the constructor?

So, one solution would be to just keep the constructors you'll most likely use. However, that's pretty short-sighted, considering you can't be certain what your client's needs are. Plus, it's pretty tedius and error-prone, since you've probably got a bunch of data that's the same type, and you'd probably need to write permutations of constructors as well.

But, Arjun, can't you use setters? Well, sure, but the point of OOP is __selective information hiding__. What's the point of creating a private field if you can use getters and setters to access and mutate it? Doesn't that violate the entire idea of working with access modifiers? So, we'll try to avoid accessors and mutators until we *absolutely* need them, which we don't in this case.

So, what's the solution? Enter, the __Builder Pattern__. Here, you create a nested class that has the same fields as our *NutritionFacts* class, and here, we can use setters to create our values. This is okay, because the Builder isn't our data, and so we don't care about our access modifiers as much. Here's an example with our basic idea so far:
``` java
// We'll keep the same fields, but we can lose the constructor for now. 
public class NutritionFacts {
    private int calories;
    private int transFat;
    private int vitaminA;
    private int sodium;

    // Here's the new Builder stuff.
    // We'd like one per class, and it doesn't need to change it's definition.
    public static final class Builder {
        private int calories;
        private int transFat;
        private int vitaminA;
        private int sodium;

        // Our setter methods should return a Builder too, so that we can chain calls.
        public Builder calories() {
            this.calories = calories;
            return this;
        }

        public Builder transFat() {
            this.transFat = transFat;
            return this;
        }

        public Builder vitaminA() {
            this.vitaminA = vitaminA;
            return this;
        }

        public Builder sodium(int sodium) {
            this.sodium = sodium;
            return this;
        }
    }
}
```

That's the basic idea, but we're missing a whole lot of plumbing. For example, we need to actually have a *NutritionFacts* constructor, and we also need to find a way to get our builder stuff __into__ our object. Also, it wouldn't hurt to keep an eye out for the future, and program to an abstraction of a builder instead of just the existing Builder class. So, here's our modified example.

``` java
public class NutritionFacts {
    private int calories;
    private int transFat;
    private int vitaminA;
    private int sodium;

    // Here's a constructor with our Builder. Keep in mind, only our Builder should 
    // use this, so let's make it private. Every client of this class 
    // should use the Builder.
    private NutritionFacts(Builder builder) {
        this.calories = builder.calories;
        this.transFat = builder.transFat;
        this.vitaminA = builder.vitaminA;
        this.sodium = builder.sodium;
    }

    // Let's make a builder() method that __returns__ a builder to use,
    // for future-proofing. Also, this applies to just the class,
    // so let's make it static.
    public static Builder builder() {
        return new Builder();
    }

    
    public static final class Builder {
        private int calories;
        private int transFat;
        private int vitaminA;
        private int sodium;

        public Builder calories() {
            this.calories = calories;
            return this;
        }

        public Builder transFat() {
            this.transFat = transFat;
            return this;
        }

        public Builder vitaminA() {
            this.vitaminA = vitaminA;
            return this;
        }

        public Builder sodium(int sodium) {
            this.sodium = sodium;
            return this;
        }

        // Here's the build method, which actually returns a NutritionFacts, using 
        // the constructor we just added.
        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }
}
```

Now that we've got the general idea, here's a use case in some other file.

``` java
public class ClientClass {
    public static void main(String[] args) {
        NutritionFacts salt = NutritionFacts.builder().sodium(100).build();
    }
}
```

Here, the only non-zero field is sodium, and it's nice and succinct. Nice!

### Factory Method pattern and Abstract Factory pattern

Both the __Factory Method__ and __Abstract Factory__ patterns are *creational* design patterns as well, meaning that they are used to create objects. These are especially useful when you don't know exactly what type you are looking for, or if you have an abstract type that you want to be able to create things with.

#### Factory Method

Here's the simple way to be able to delegate creation: __The Factory Method__. In this pattern, we can use a creator to get the exact right instance of the variable we want.

Suppose we have an interface to represent a shape:

``` java
public interface Shape {
    double getArea();
    double getPerimeter();
}
```

And suppose we have some concrete classes that implement it. Here's a Circle, for example:

``` java
public class Circle implements Shape {
    private int radius;
    
    public Circle(int radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * this.radius * this.radius;
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * this.radius;
    }   
}
```

And here's a Square:

``` java
public class Square implements Shape {
    private int side;
    
    public Square(int side) {
        this.side = side;
    }

    @Override
    public double getArea() {
        return this.side * this.side;
    }

    @Override
    public double getPerimeter() {
        return 4 * this.side;
    }   
}
```

Now that we've represented the product classes, let's talk about the Creator class which uses them. Say we have a Board, for example, that renders different images. Now, each board has the same images, but we want to be able to abstract beyond the concrete shape.

``` java
public abstract class Board {
    private final Shape toRender;

    public Board() {
        toRender = this.createShape();
    }

    abstract protected Shape createShape();
}
```

Now, we can feed in any kind of shape we wish, all we have to do is create a new subclass that implements the method.

``` java
public class CircleBoard extends Board {
    @Override
    protected Shape createShape() {
        return new Circle(1);
    }
}

public class SquareBoard extends Board {
    @Override
    protected Shape createShape() {
        return new Square(1);
    }
}
```

As you can see, the reason this pattern is called the __Factory Method__ pattern is because all of the "factory" behavior, creating the subclassed shape, is done in the ```createShape()``` method, through inheritance.

This is usually the first bit of abstraction we can do to clean up our code's creational design, and is more of a "gateway" pattern to more complex, more fruitful patterns as well, like the __Abstract Factory__ we will look at next.

#### Abstract Factory

So, we've talked about how we can use a method in our class heirarchies to set up creation of subtypes. But, what if we want to be able to use this in *many different parts of our code?* We'd need to be able to move around and use the factory by itself, without all the baggage of the creator class. Enter, the Abstract Factory.

Let's take the example from before, about our shapes, but let's say we want to throw in a new twist: We want to be able to model different sizes of our shapes, and we want that to be reflected by its color. This means that, for example, every red circle is the same size, but it can be a different size then say, a green circle, or a blue circle, and so on.

This means that we really want subdivisions in our square and our circle class. With our previous design, we really can't achieve this easily, and even if we could, it definitely won't be extensible enough to handle new colors. For our purposes, though, let's start by just working with red and blue as our colors.

So, we can start making some changes. Let's begin by making our existing Square and Circle data types abstractions rather than implementations, and we can do that by refactoring from ```class``` to ```abstract class```:

``` java
public abstract class Circle implements Shape {
    private int radius;
    
    public Circle(int radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * this.radius * this.radius;
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * this.radius;
    }   
}

public RedCircle extends Circle {
    private Color color;

    public RedCircle(int radius) {
        super(radius);
        this.color = Color.RED;
    }
}

public BlueCircle extends Circle {
    private Color color;

    public BlueCircle(int radius) {
        super(radius);
        this.color = Color.BLUE;
    }
}
```

And here's a Square:

``` java
public abstract class Square implements Shape {
    private int side;
    
    public Square(int side) {
        this.side = side;
    }

    @Override
    public double getArea() {
        return this.side * this.side;
    }

    @Override
    public double getPerimeter() {
        return 4 * this.side;
    }   
}


public RedSquare extends Square {
    private Color color;

    public RedSquare(int size) {
        super(size);
        this.color = Color.RED;
    }
}

public BlueSquare extends Square {
    private Color color;

    public BlueSquare(int size) {
        super(size);
        this.color = Color.BLUE;
    }
}
```

Now, we can create an abstract factory, which allows us to make Squares and Circles, and its subclasses.
``` java
public interface AbstractShapeFactory {
    // We could use Shape here, but that's actually limiting our functionality.
    // In the AbstractFactory pattern, the items the factory creates don't 
    // necessarily have to be related to each other.
    Square createSquare();
    Circle createCircle();
}

public class RedShapeFactory implements AbstractShapeFactory {
    @Override
    public Square createSquare() {
        return new RedSquare(10);
    }

    @Override  
    public Square createCircle() {
        return new RedCircle(10);
    }
}


public class BlueShapeFactory implements AbstractShapeFactory {
    @Override
    public Square createSquare() {
        return new BlueSquare(5);
    }

    @Override  
    public Square createCircle() {
        return new BlueCircle(5);
    }
}
```

Now, how is this used? Well, a client, say a Board that renders these, can use the factory to get what kind of Shape they want. Notice that this is done through composition, not inheritance.
 
``` java
public class Board {
    private AbstractShapeFactory factory;
    
    public Board(AbstractShapeFactory factory) {
        this.factory = factory;
    }

    public getAreas() {
        System.out.println("Circle area: " + factory.createCircle().area() 
        + " Square area: " + factory.createSquare().area());
    }
}
```

### Command pattern

Sometimes, when you are writing some kind of a controller, you need to plan for a bunch of possible cases. Say, for example, you're taking in input and doing something, based on the string value you take in.

This is where the __Command Pattern__ comes in: here, what we want to do is be able to *detach* the commands from the data that they act upon. But why? Well, there are two advantages with this approach: we can encapsulate the logic behind the move, allowing for reusability and extension. Also, we can maintain readability with our code, so that the method that uses these commands doesn't get too convoluted, and fullfills a single purpose.

Take, for example, a simple controller that uses a switch statement to execute some functions. Naively, we could just throw everything in one big switch statement, right?

``` java
public class ExectuteFunction {
    public void myFunction(String s) {
        switch (s) {
            case "q":
            case "quit":
                return;

            case "neg":
                System.out.println(-1);
                break;
            case "pos":
                System.out.println(0);
                break;
            default:
                System.out.println("%s is not a valid command.", s);
                break;  
        }
    }
}
```

So this isn't *__terrible__* to maintain, but it isn't easy, either. If we wanted to add something, we'd have to go back in here and add. Not very extensible. But, more glaringly, this violates our SOLID principles. We shouldn't have to modify this method to get an extension of functionality. So, how do we do this, so that we can keep our method tight, and have minimal functionality stuffed in here upon extension? This is where the __Command Pattern__ comes in. Here, we make each of our commands encapsulate all of it's work, so that we can use it in multiple places.
