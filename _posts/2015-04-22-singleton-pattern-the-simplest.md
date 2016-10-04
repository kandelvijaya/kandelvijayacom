---
ID: 70
post_title: 'Singleton Pattern: the simplest!'
author: kandelvijaya
post_date: 2015-04-22 16:44:55
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2015/04/22/singleton-pattern-the-simplest/
published: true
---
<blockquote>Singletons are the one of the kind.</blockquote>
Singletons lets anyone create them only once and that one created object is held strongly on the static variable of the same class. Sounds complex but it is not. Sure it is tricky. You do try to do everything to make it work but to break it down. It comes to clarity and the intent.
<pre class="lang:default decode:true " title="A singleton implementation in Swift. ">/*
*   Singletons
*/

class BaseSingleton{
    var name:String = "bj"
    static var sharedInstnace:BaseSingleton?

    private init(){
        //we need to make this constructor uncallable from anywhere then this class itself
        assert(true, "You cant create a concrete object directly for a singleton")
        //however this is a run time erroring system
    }

    static func getSharedInstance() -&gt; BaseSingleton{
        if sharedInstnace == nil {
            sharedInstnace = BaseSingleton()
        }

        return sharedInstnace!
    }
}
</pre>
&nbsp;

With swift you just can't return value from a initializer. But we can hide other classes from outside this file to call init by making it private. So none can do something like Singleton() to check how we do. Its a compile time error.

However, the above code is not multi-thread safe. The implementation is straightforward relative to how java handles thread safety. We are going to look at it in the next post.