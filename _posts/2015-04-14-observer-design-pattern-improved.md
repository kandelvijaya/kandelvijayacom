---
ID: 67
post_title: Observer Design Pattern Improved!
author: kandelvijaya
post_date: 2015-04-14 17:25:07
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2015/04/14/observer-design-pattern-improved/
published: true
cleanretina_sidebarlayout:
  - default
---
The previous observer pattern allowed the subject to broadcast whenever the subject state changed. However not all minor changes are good to be sent. Moreover the observer has to get all the big data struct of state changes and use some few which is  memory management wise bad decision. Why not allow the user to decide which state data to user once it knows there is a significant changes? The former approach is called <strong>PULL</strong>. while the first one was <strong>PUSH. </strong>Its true for pull the observer has the added responsibility but sometimes it comes useful.
<pre class="lang:default decode:true ">//Push and Pull Observer pattern
//usage: When we use push only, the observer maynot want to get all the things that changes
//A observer might be interested to look at some few values rather then the whole set
//Or moreover compute other things when the first parameter is somewhat good for that object
//So a way to push and pull both implemented as below

protocol WeatherStation{
    var name:String{get}
    func addObserver(observer:WeatherObserver)
    func removeObserver(observer:WeatherObserver)
    func onChange()
    func notifyObserver(tempData:Double, sender:WeatherStation)
}

protocol WeatherObserver{
    func onWeatherUpdate(tempData:Double, sender:WeatherStation)
}

//rule of object oriented design 1: program to interface rather than implementation
//it increases reuse and less code modification when some changes has to be made in on class

class NepalWeatherStation:WeatherStation{
    //backing storage
    var currentObservers = [WeatherObserver]()
    var currentTemp:Double!     //once it gets its always something
    var significantChangeNoticed:Bool!
    var name = "NepalWeatherStation"

    //methods
    func addObserver(observer: WeatherObserver) {
        self.currentObservers.append(observer)
    }

    func removeObserver(observer: WeatherObserver) {
        //implment some removing method here
    }

    func onChange() {
        self.significantChangeNoticed = true;
    }

    func notifyObserver(tempData: Double, sender: WeatherStation) {

        if self.significantChangeNoticed != false{
            for observer in currentObservers{
                observer.onWeatherUpdate(self.currentTemp, sender: self)
            }
        }
        //set the flag to false again
        self.significantChangeNoticed = false
    }



    //this method is called when weather changes
    func weatherChanged(temp:Double){
        self.currentTemp = temp;
        self.onChange()
        self.notifyObserver(currentTemp, sender: self)

    }
}

class NationalTVWeatherForecast:WeatherObserver{
    func onWeatherUpdate(tempData: Double, sender: WeatherStation) {
        //decide to do pull or push
        //if it was pushed: direct use the data parameter
        //it is is pulled: then ask the weatehrstation to give certain fields
        if sender.name == "NepalWeatherStation"{
            println("The national temp is \(tempData)")
        }else{
            println("This is not national data")
            //retrieve country name or measurement unit used there
            println("The station name is \(sender.name)")

        }
    }

    init(subject:WeatherStation){
        subject.addObserver(self)
    }
}

//do it
var nepalWDStation = NepalWeatherStation()
var ntv = NationalTVWeatherForecast(subject: nepalWDStation)

//simulate weather change
nepalWDStation.weatherChanged(24)
nepalWDStation.weatherChanged(55)


</pre>
&nbsp;