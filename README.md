# Swift-Strategy-Design-Pattern-
<h4>This is a simple Swift playground which shows in simple terms the concept of the Strategy Design pattern.</h4>
This is the <b>STRATEGY DESIGN PATTERN</b> interpreted by me in Swift,
this is a simple Swift playground that serves as proof of concept;
notice that the book <i>"Head First Design Patterns by Eric Freeman
and Elisabeth Robson in Java"</i> was used as reference. The following design principles
can be found in this book under <i>"Strategy Design Pattern"</i> :
<h3>Design Principles:</h3>
<ul>
<li>Identify aspects of your application that vary and separate them from what stays the same.</li>
<li>Program to an interface, not an implementation.</li>
<li>Favor composition over inheritance.</li>
</ul>
Imagine a hypothetical situation where you have an object, or a set of related objects that have a common ancestor, or parent whom they inherit from. For the sake of simplicity lets asume that this parent object is a <b>Duck</b> and that the objects <b>MallardDuck</b>, <b>RocketDuck</b> and <b>RubberDuck</b> all inherit from <b>Duck</b>. 
Suppose that the need arrises to implement a method that allows <b>Ducks</b> to perform the kind of flight you would expect from any of them. How would a programmer tackle this problem?<br><br>

One quick solution, and the one that will probably come to mind to the <b>OOP</b> savy is to declare a method fly() as part of the class <b>Duck</b>, this would allow each <b>Duck</b> to <i>inherit</i> the <b>fly()</b> method:
```swift
class Duck {
    func fly()->String{ return "I fly!"}
}

class MallardDuck: Duck {
    //...
}

class RocketDuck: Duck {
    //...
}

class RubberDuck: Duck {
    //...
}
var someDuck: Duck = MallardDuck()
print(someDuck.fly())
//will print "I fly!"
var anotherDuck: Duck = RocketDuck()
print(anotherDuck.fly())
//will print "I fly!"
```
However simple and easy to grasp this may not be a good solution, as not all <b>Ducks</b> fly the same way, MallardDucks are supposed to fly long distances while RubberDucks just bounce.
 <br><br>
One would say a better approach would be then to make each duck overide the <b>fly()</b> function each inherit from the <b>Duck</b> class giving it each it's own implementation, that way each duck implements its unique <b>fly()</b> function and every duck can <b>fly</b> the way they are supposed to without worrying about the other <b>Ducks</b>. This seems like a good solution, but what would happen if we include in the program new objects that are also <b>Ducks</b>, (-that is they inherit from the <b>Duck</b> class-) but are not suppossed to implement <b>fly()</b>.<br> For example think of an object which is a duck but the method <b>fly()</b> is completely irrelevant to it, that is to have this object conform to this function is not only erroneous but also ilogical, lets call this object <b>DuckStatue</b>, then our current configurations would make the following occur:
```swift
class Duck {
    func fly()->String{ return "I fly!"}
}

class DuckStatue: Duck {
    //...
}

var thisDuck: Duck = DuckStatue()
thisDuck.fly()
//will print "I Fly!", this is not good as DuckStatue is not supposed to have a fly functionality!

//also this:
class DuckStatue: Duck {
    override func fly()-> String{
        return "I do not Fly!"
    }
}
var thisDuck: Duck = DuckStatue()
thisDuck.fly()
//will print "I do not Fly!", again this is not good, since DuckStatue is not supposed to have a fly functionality at all!
```

<br>
This object is not supposed to have a <b>fly()</b> method because of its nature, however our current program implementation will force a fly function unto this object and that is absolutely <i>"no bueno"</i>.
<br>
<h4>The better solution is to instead of concentrating in the <b>Ducks</b>, <b>MallardDucks</b>, and other <b>Duck</b> objects focus on the behaviors which vary across all of the Duck classes:</h4>
<br>
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
        print("I can not fly, just bounce! :(")
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

print(someDuck.describeDuck())      //Will print "Mallard Duck"
print(someDuck.flyBehavior.fly())   //Will print "I am flying!"

// Change the behavior of the object to flyNoWay()
print("------------")
someDuck.flyBehavior = FlyNoWay()  
print(someDuck.describeDuck()); print(someDuck.flyBehavior.fly()) 
//Will print "Mallard Duck" followed by "I can not fly, just bounce!"

// Create another Duck and assign it a fly behavior
print("------------")
var anotherDuck: Duck = RubberDuck(FlyNoWay())
anotherDuck.describeDuck(); anotherDuck.flyBehavior.fly()
//Will print "Rubber Duck" followed by "I can not fly, just bounce!"

// Change the behavior of the previous object
print("------------")
anotherDuck.flyBehavior = FlyWithRocket()
anotherDuck.describeDuck(); anotherDuck.flyBehavior.fly()
//Will print "Rocket Duck" followed by "I am flying with a Rocket!" 

```
As it is shown this pattern allows to loosely provide any Duck a fly behavior programatically instead of fixing any kind of duck to an specific behavior through code.

<h3>This design pattern allows to define a family of algorithms aka behaviors
encapsulating each one making them interchangeable. </h3>

<h1>References: </h1>
<i>Freeman, Eric, Elisabeth Robson, Kathy Sierra, and Bert Bates. Head First design patterns. Sebastopol, CA: O'Reilly, 2004. Print.</i>
