SOLID Principles are an important coding standard that fosters a common concept among developers for designing software in a way that avoids poor design. These ideas, introduced by Robert C. Martin, are frequently used in the field of object-oriented design. SOLID makes code more extensible, logical, and understandable when used diligently.

Ignoring these rules can result in rigid and brittle programming, with modest software changes introducing errors. As a result, following SOLID Principles becomes critical for strong software development.

Although it may take some practise, coding in accordance with these principles considerably improves code quality and allows for a more in-depth understanding of well-designed software. Understanding how to use interfaces effectively is critical. Let's dissect each principle one at a time.

###### [Github](https://github.com/nraxz/solid-principle) Repositories


## Single Responsibility Principle :

>A class should have one, and only one, reason to change.

The Single Responsibility Principle (SRP) states that a class should have only one reason to change, meaning it should only have one responsibility. In PHP, this translates to a class having only one job or functionality. Here's an example to illustrate SRP:

**Please look at the following code:**	

```php
class User {
    private $name;
    private $email;

    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
    }

    public function saveToDatabase() {
        // Code to save user data to the database
    }

    public function sendWelcomeEmail() {
        // Code to send a welcome email to the user
    }
}
```

In this example, the `User` class has two responsibilities: saving user data to the database and sending welcome emails. This violates SRP because a change in one responsibility (e.g., database handling) may affect the other (e.g., email functionality).

**With SRP (Adhering to SRP):**	

```php
class User {
    private $name;
    private $email;

    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
    }
}

class UserDatabaseHandler {
    public function saveToDatabase(User $user) {
        // Code to save user data to the database
    }
}

class EmailService {
    public function sendWelcomeEmail(User $user) {
        // Code to send a welcome email to the user
    }
}

```

In this improved version, we've separated concerns. The `User` class now only represents a user, while the `UserDatabaseHandler` and `EmailService` classes handle database and email functionality, respectively. Each class has a single responsibility, adhering to the Single Responsibility Principle. If there's a change in database handling, it won't affect the email functionality, and vice versa.

## Open-closed Principle :

>Entities should be open for extension, but closed for modification.

The Open-Closed Principle (OCP) states that a class should be open for extension but closed for modification. In other words, you should be able to add new functionality to a class without altering its existing code. This is achieved through the use of abstractions and interfaces.

**Without OCP (Violation):** 

```php
class Rectangle {
    public $width;
    public $height;

    public function __construct($width, $height) {
        $this->width = $width;
        $this->height = $height;
    }
}

class AreaCalculator {
    public function calculateRectangleArea(Rectangle $rectangle) {
        return $rectangle->width * $rectangle->height;
    }
}

```

In this example, if we want to extend the `AreaCalculator` to calculate the area of a circle, we would need to modify the `AreaCalculator` class, violating the OCP.

**With OCP (Adhering to OCP):**


```php
interface Shape {
    public function calculateArea();
}

class Rectangle implements Shape {
    public $width;
    public $height;

    public function __construct($width, $height) {
        $this->width = $width;
        $this->height = $height;
    }

    public function calculateArea() {
        return $this->width * $this->height;
    }
}

class Circle implements Shape {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }

    public function calculateArea() {
        return pi() * $this->radius * $this->radius;
    }
}
```

Now, with the introduction of the `Shape` interface and separate classes for `Rectangle` and `Circle`, we can extend the functionality without modifying existing code. The `AreaCalculator` can work with any class that implements the `Shape` interface, allowing for easy extension without altering the existing code. This adheres to the Open-Closed Principle.


## Liskov Substitution Principle:

The Liskov Substitution Principle (LSP) states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, if a class is a subclass of another class, it should be able to be used wherever the parent class is used, without causing issues.

**A code snippet to show how violates LSP and how we can fix it:** 

```php
class Bird {
    public function fly() {
        return "Flying...";
    }
}

class Penguin extends Bird {
    public function fly() {
        // Penguins can't fly, so override the method
        return "Sorry, I can't fly.";
    }
}

function makeBirdFly(Bird $bird) {
    return $bird->fly();
}

$eagle = new Bird();
$penguin = new Penguin();

echo makeBirdFly($eagle);   // Output: Flying...
echo makeBirdFly($penguin); // Output: Sorry, I can't fly.

```

In this example, `Penguin` is a subclass of `Bird`, and it overrides the `fly` method since penguins can't fly. The `makeBirdFly` function accepts any object of type `Bird`. When we pass an instance of `Penguin` to this function, it still works correctly without causing issues, adhering to the Liskov Substitution Principle.

The key is that the subclass (`Penguin`) should be substitutable for its superclass (`Bird`) without altering the correctness of the program.

## Interface Segregation Principle :

>A Client should not be forced to implement an interface that it doesn't use.

The Interface Segregation Principle (ISP) states that a class should not be forced to implement interfaces it does not use. In other words, it suggests that it's better to have multiple smaller, specific interfaces rather than a single large, general-purpose interface.

**Bad Example: Violating ISP**

```php
// Single interface with fly and swim methods
interface FlyableAndSwimmable {
    public function fly();
    public function swim();
}

// Bird class implementing FlyableAndSwimmable
class Bird implements FlyableAndSwimmable {
    public function fly() {
        return "Flying...";
    }

    public function swim() {
        // Birds generally don't swim, so this method is irrelevant
        return "Sorry, I can't swim.";
    }
}

// Fish class implementing FlyableAndSwimmable
class Fish implements FlyableAndSwimmable {
    public function fly() {
        // Fish can't fly, so this method is irrelevant
        return "Sorry, I can't fly.";
    }

    public function swim() {
        return "Swimming...";
    }
}

```

In the above code, we have a single interface `FlyableAndSwimmable` that combines both flying and swimming methods. Both `Bird` and `Fish` are forced to implement both methods, even though one of the methods is irrelevant for each class. How we can fix it please see the following code **Good Example**:

```php
// Separate interfaces for flying and swimming
interface Flyable {
    public function fly();
}

interface Swimmable {
    public function swim();
}

// Bird class implementing Flyable
class Bird implements Flyable {
    public function fly() {
        return "Flying...";
    }
}

// Fish class implementing Swimmable
class Fish implements Swimmable {
    public function swim() {
        return "Swimming...";
    }
}

// Duck class implementing both Flyable and Swimmable
class Duck implements Flyable, Swimmable {
    public function fly() {
        return "Flying...";
    }

    public function swim() {
        return "Swimming...";
    }
}

```


## Dependency Inversion Principle :

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

> Abstractions should not depend on details. Details should depend on abstractions.

Or simply : Depend on Abstractions not on concretions

The Dependency Inversion Principle (DIP) states that high-level modules should not depend on low-level modules but rather both should depend on abstractions. In addition, abstractions should not depend on details; details should depend on abstractions. This principle encourages the use of interfaces or abstract classes to achieve a flexible and decoupled architecture.

Let's start with a bad example that violates the Dependency Inversion Principle, and then we'll provide a good example that adheres to the principle.

**Bad Example: Violating DIP**

```php
// Low-level module representing a light switch
class LightSwitch {
    public function turnOn() {
        // Code to turn on the light
    }

    public function turnOff() {
        // Code to turn off the light
    }
}

// High-level module representing a house
class House {
    private $lightSwitch;

    public function __construct() {
        $this->lightSwitch = new LightSwitch();
    }

    public function manageLights() {
        $this->lightSwitch->turnOn();
        // Code to manage lights
    }
}

```

In the above code, the `House` class directly depends on the concrete implementation of the `LightSwitch` class, violating the Dependency Inversion Principle. If the `LightSwitch` implementation changes, the `House` class needs to be modified.

**Good Example: Adhering to DIP**

```php
// Abstraction for a switch interface
interface Switchable {
    public function turnOn();
    public function turnOff();
}

// Low-level module representing a light switch implementing the interface
class LightSwitch implements Switchable {
    public function turnOn() {
        // Code to turn on the light
    }

    public function turnOff() {
        // Code to turn off the light
    }
}

// High-level module representing a house depending on the abstraction
class House {
    private $switchable;

    public function __construct(Switchable $switchable) {
        $this->switchable = $switchable;
    }

    public function manageLights() {
        $this->switchable->turnOn();
        // Code to manage lights
    }
}
```

In this good example, the `Switchable` interface is introduced, and both the `House` class and the `LightSwitch` class depend on this abstraction. This adheres to the Dependency Inversion Principle, as the high-level module (`House`) now depends on an abstraction (`Switchable`) rather than a concrete implementation. It allows for flexibility and easy substitution of different implementations of the `Switchable` interface without modifying the `House` class.

Please also see a post that [explains SOLID principle in the context of Database](https://nareshshahi.com/articles/importance-of-explaining-solid-principle-in-the-context-of-database), I hope this will help beginners to understand SOLID principle.