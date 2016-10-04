---
ID: 65
post_title: 'Observer Pattern in Swift : Simple Code only!'
author: kandelvijaya
post_date: 2015-04-14 16:53:17
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2015/04/14/observer-pattern-in-swift-simple-code-only/
published: true
cleanretina_sidebarlayout:
  - default
---
Observer pattern is mostly used in making User Interface Interactions. In iOS its mostly used to listen for changes in some class; when the internet connection is established, when data source for table view changes and lots of things. In Android, the use is manifested in the usage of adding clickListener for a button. You just ask the button you want your custom class that implements someInterface to receive touch events and you handle them without ever knowing how is this all done.

Its okay to use the design patterns provided but sometimes you might just need to do something really useful with them given you know how they work. Observer pattern simply takes a SUBJECT which has some information thats going to be changed and some classes that depend on the information. For example, a user on Facebook posted a post and your application needs to know that or other users get notified of this on their wall.

Its like Newspaper subscription. The dependent class ask the subject to receive notification or remove from listening. The rest is done dynamically. This concept comes from a important design concept is OOP that is:
<blockquote>encapsulate the changing fields. Prefer composition rather than inheritance.</blockquote>
So lets see the observer method applied in Swift simple command line program:
<pre class="lang:default decode:true " title="Push Observer Design Pattern">//observer pattern
//objects can opt in to observe a subject or opt out
//whenever there is a change in the data in the subject all object gets notified of the change


//simple way of implementing this would be
protocol WeatherDisplayable{

    var name:String{ get }

    func tempChanged(temp:Double)
    //func humidityChanged(humindity:Double)
    //func pressureChanged(pressure:Double)
}


class WeatherData{
    var temp:Double?


    var observers=[WeatherDisplayable]()

    func addObserver(observer:WeatherDisplayable){
        self.observers.append(observer)
    }

    func removeObserver(observer:WeatherDisplayable){
        var thisObserverIndexOnArray:Int?
        for index in 0..&lt;observers.count{
            if observers[index].name == observer.name{
                //this is it
                thisObserverIndexOnArray = index
            }
        }

        //remove it
        if let index = thisObserverIndexOnArray {
            observers.removeAtIndex(index)
        }
    }

    func tempChanged(t:Double){
        for observer in observers{
            observer.tempChanged(t)
        }
    }
}


class NepalWeather:  WeatherDisplayable{

    var name:String {
        return "Nepal";
    }

    func tempChanged(temp: Double) {
        println("new temperature for \(name) is \(temp)")
    }
}

class KittuWeather:WeatherDisplayable{
        var name:String{
            return "Kittu"
        }
        func tempChanged(temp: Double) {
            println("The new weather for \(name) is\(temp)")
        }
    }


//do it
var wdstation = WeatherData()
var kittu = KittuWeather()
var nepalw = NepalWeather()
wdstation.addObserver(kittu)


//change the weather from the station
wdstation.tempChanged(12.0)
wdstation.tempChanged(24)
wdstation.addObserver(NepalWeather()) //we dont have a way to remove this item because we dont have any pointer or var to it
//this in fact is a bad practice
wdstation.tempChanged(55)

wdstation.removeObserver(kittu)

wdstation.tempChanged(33.33)
</pre>
&nbsp;