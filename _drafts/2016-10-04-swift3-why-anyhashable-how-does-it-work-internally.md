---
ID: 493
post_title: 'Swift3: Why AnyHashable? How does it work internally?'
author: kandelvijaya
post_date: 2016-10-04 20:03:17
post_excerpt: ""
layout: post
permalink: http://kandelvijaya.com/?p=493
published: false
---
Evolution is predominant. Struggle for Survival applies to just anything that you see. Swift Programming Language is not an exception. Swift continues to change, evolve and mature over time. We can keep our feet wet, migrating year after year to Swift X version. I would. If it strives to be better. This years, `Swift 2 -> Swift 3` was little more than a mini project. We saw lots of changes but today, `[AnyObject: NSObject]` became `[AnyHashable: Any]`. So does JSON, NSArray and NSDictionary. We will see why `AnyObject -> AnyHashable`?

But before that, lets dig a little deep to see how and what `AnyHashable` does? Better yet, lets simulate a similar `Any2Hashable` together. 
 
# AnyObject -> Any
1. Swift focuses on using Value Types / immutable type in all cases possible. Foundation, has in other hand, all reference type. Classes. Which will be imported into reference type in swift.
2. `AnyObject` is idomatially ObjectiveC flavored and is reference type.
3. Swift is platfrom independent, it had to move away from relying on ObjectiveC idioms and its runtime. Hence `AnyObject` had to be replace with Value type and Swift flovored `Any`.

## Some more thoughts
- `Any` is value type. (We will see how it actually is boxing refernce but is Value type later on)
- All Foundation / Objective-C `id` types will be imported as `Any`. 
- All Swift types including Enum and Struct can be bridged to Objective-C as `id`. This id is minimal. 
- All Swift types that were bridged to Objective-C `id` can be bridged back to `Swift` as `Any` or casted to their previous Type. Swift doesnot remove the type information during the boxing internally.
- For Example:
```swift
enum Direction2 : String {
    case down = "UP"
    case up = "DOWN"
}
var objcArray = NSMutableArray()        // [NSObject, NSObject] or [id, id]
var swiftEnum = Direction2.down
objcArray.add(swiftEnum)

objcArray.lastObject as? Direction2     //down
objcArray.lastObject as? NSString       //nil
```

# NSObject -> AnyHashable 
Consider the situation - `[NSObject: AnyObject]`. This turned into `[NSObject: Any]`.

- Its natural to get rid of `Object` feeling. Afterall, AnyObject became Any. 
- Like before, we wanted to work with Value types. NSObject does 2 things which we want to avoid.
    + It is a reference type. (There is also NSObjectProtocol)
    + It requires us to know about ObjectiveC idiom. Swift is again platform independent.
- A `Dictionary`, `Array` , `Set` expects Key/Element to be `Hashable`. There is no requirement it can just be some few types. 
- Hence, its more fluid to represent somewhat alien `[NSObject: AnyObject]` with `[AnyHashable, Any]`. 
- Wait. Why not `[Hashable: Any]`? Good question. Lets see.

## Why not `[Hashable: Any]`?
