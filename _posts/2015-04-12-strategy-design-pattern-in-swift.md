---
ID: 58
post_title: Strategy design pattern in Swift
author: kandelvijaya
post_date: 2015-04-12 05:10:29
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2015/04/12/strategy-design-pattern-in-swift/
published: true
cleanretina_sidebarlayout:
  - default
---
<pre class="lang:swift decode:true ">//we are going to implement a strategy design pattern
protocol Displayable{
    func display()
}

class Duck:Displayable{

    var flyBehavior:FlyBehavior?{
        didSet{
            performFly()
        }
    }
    var quckBehavior: QuackBehavior?{
        didSet{
            performQuack()
        }
    }


    //we don't have abstract methods in swift or objc so this is a way to hack it around
    func display(){
        assert(false, "Must override this because this is abstract method")
    }


    init(){
        display()
    }

    func performFly(){
        if let fly = self.flyBehavior {
            fly.fly()
        }
    }

    func performQuack(){
        if let quack = self.quckBehavior{
            quack.quack()
        }
    }

    //making a method abstract is forcing the subclass to override it anyhow
}

class RedDuck:Duck{
        var name = "Red Duck"

    //overide abstract
     override func display() {
        println("\(name):this one is red")
    }



}

class RubberDuck:Duck{
    var name = "RubberDuck"
    //override
    override func display() {
        println("im a rubber one i cant fly nor quack")
    }

}

protocol FlyBehavior{
    func fly()
}

protocol QuackBehavior{
    func quack()
}

class Fly:FlyBehavior{
    func fly() {
        println("the duck is flyinh high")
    }
}

class Quack:QuackBehavior{
    func quack() {
        println("The duck is quackling")
    }
}

class Squeak:QuackBehavior{
    func quack() {
        println("I squeak")
    }
}

class DontFly:FlyBehavior{
    func fly() {
        println("I cant fly")
    }
}

class DontQuack:QuackBehavior{
    func quack() {
        println("I cant quack")
    }
}




class WhiteDuck:Duck{

    //overide the behavior
    override func display() {
        println("White Duck")
    }

    override init() {
        super.init()

        self.flyBehavior = Fly()
        self.quckBehavior = Quack()
    }

    func setBehavior(behavior:FlyBehavior){
        self.flyBehavior = behavior
    }
}

class WoodenDuck:Duck{
    override func display() {
        println("Wooden Duck Does nothing")
    }
}

var a = WhiteDuck()
a.setBehavior(DontFly())

</pre>
I was reading the Head First Design Pattern yesterday night on my iPad. I downloaded the torrent file which was scanned. I was never a fan of head first series before because of their childish presentation or i happened to pick up the subject that was not good for me: javascript. I gave it a try yesterday after lots of people were suggesting it is a great read. I went through 2 pages and couldn't stop reading the book. I loved the funny exercise and to the point pictures. In fact i ordered the book via Amazon for around $70. I know its expensive but not for a good investment.

The book however teaches the design pattern the implementation is based on java programming language. I was doing swift and almost completed the language reference and so i thought why not port it to swift. I even know there are lots of features which are impossible in swift from Java because the language is totally different.

Good Day.