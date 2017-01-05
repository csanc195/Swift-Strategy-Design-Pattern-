# Swift-Strategy-Design-Pattern-
This is a simple Swift playground which shows in simple terms the concept of the Strategy Design pattern

This is the <b>STRATEGY DESIGN PATTERN</b> interpreted by me in Swift,
this is a simple Swift playground that serves as proof of concept;
notice that the book <i>"Head First Design Patterns by Eric Freeman
and Elisabeth Robson in Java"</i> was used as reference. The following design principles
can be found in this book under <i>"Strategy Design Pattern"</i>:
<h3>Design Principles:</h3>
<ul>
<li>Identify aspects of your application that vary and separate them from what stays the same.</li>
<li>Program to an interface, not an implementation.</li>
<li>Favor composition over inheritance.</li>
</ul>

We start by encapsulating the behavior using a simple base Protocol and creating classes as needed to describe the diferent 
behaviors that we might need. This makes use of the first principle: <i>"Identify aspects of your application that vary and separate them from what stays the same."</i>

```swift
protocol FlyBehavior {
    func fly()
}

class FlyWithWings: FlyBehavior {
    
    func fly() {
        print("I am flying!")
    }
}

class FlyNoWay: FlyBehavior {
    func fly() {
        print("I cant fly! :(")
    }
}
class FlyWithRocket: FlyBehavior {
    func fly() {
        print("I am Flying with a rocket!")
    }
}
```

Describing the base object Duck being composed of a fly behavior we set in practice the second and third principles: <i>"Program to an interface, not an implementation."</i> and <i>"Favor composition over inheritance"</i> by making the class duck "have" a <b>FlyBehavior</b> instead of making the Duck obtain these behaviors through inheritance we ensure that any duck is really decoupled from the behaviors and not attached to them, so instead of having just one behavior aquired statically any duck can be assigned a different instance of <b>FlyBehavior</b> on runtime programatically!.

```swift
class Duck {
    var flyBehavior: FlyBehavior
    
    init(_ flyBehavior: FlyBehavior) {
        self.flyBehavior = flyBehavior
    }
    func describeDuck(){
    }
}

class mallardDuck: Duck {
    
    let duckName: String
    
    override init(_ flyBehavior: FlyBehavior){
        self.duckName = "Mallard Duck"
        super.init(flyBehavior)
    }
    override func describeDuck() {
        print("\(duckName)")
    }
}

class RubberDuck: Duck {
    let duckName: String
    
    override init(_ flyBehavior: FlyBehavior) {
        duckName = "Rubber Duck"
        super.init(flyBehavior)
    }
    override func describeDuck() {
        print("\(duckName)")
    }
}


class RocketDuck: Duck {
    let duckName: String
    
    override init(_ flyBehavior: FlyBehavior){
        duckName = "Rocket Duck"
        super.init(flyBehavior)
    }
    override func describeDuck() {
        print("\(duckName)")
    }
}

// create some fly behavior and create a new Duck and assign this behavior to it:
let someFlyBehavior: FlyBehavior = FlyWithWings()
var someDuck: Duck = mallardDuck(someFlyBehavior)
someDuck.describeDuck()
someDuck.flyBehavior.fly()

// Change the behavior of the object to flyNoWay()
print("------------")
someDuck.flyBehavior = FlyNoWay()
someDuck.describeDuck(); someDuck.flyBehavior.fly()

// Create another Duck and assign it a fly behavior
print("------------")
var anotherDuck: Duck = RubberDuck(FlyNoWay())
anotherDuck.describeDuck(); anotherDuck.flyBehavior.fly()
// Change the behavior of the previous object
print("------------")
anotherDuck.flyBehavior = FlyWithRocket()
anotherDuck.describeDuck(); anotherDuck.flyBehavior.fly()

```
This design pattern allows to define a family of algorithms aka behaviors
encapsulating each one making them interchangeable. 









<h1>References: </h1>
<i>Freeman, Eric, Elisabeth Robson, Kathy Sierra, and Bert Bates. Head First design patterns. Sebastopol, CA: O'Reilly, 2004. Print.</i>
