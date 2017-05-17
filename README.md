# iOS-interview

## <a name='contents'>Table of Contents</a>

* [Cocoa touch](#cocoa-touch)
* [Memory Management](#memory-management)
* [Networking](#networking)
* [Code persistance](#code-persistance)
* [CoreAnimation, CoreGraphics, CoreText](#coreanimation-coregraphics-coretext)

* [Patterns(4)](#patterns)
* [Multithreading](#multithreading)
* [Maps](#maps)
* [Algorithms](#algorithms)
* [Logical](#logical)
* [Code Puzzels](#code-puzzels)
* [Additional](#additional)
* [Podcasts](#podcasts)
* [General](#general)
* [Swift](#swift)
* [Objective-C](#objective-c)
* [Code](#code)
* [HomeWork](#homework)
* [Job websites](#jobwebsites)
* [Companies](#companies)
* [Stories](#stories)
* [Proccess](#proccess)


## Cocoa touch 
[[⬆]](#contents) <a name='general'></a>

## Memory Management

## Networking
<details> 
  <summary> Which JSON framework is supported by iOS?  </summary>
A. SBJson framework is supported by iOS.  It is a JSON parser and generator for Objective-C. SBJson provides flexible APIs and additional control that makes JSON handling easier.

</details>



## Data persistance
### (CoreData)

## CoreAnimation CoreGraphics CoreText

## Patterns

<details> <summary>What is OOP?</summary></details>
<details> <summary>What are the pros and cons of inheritance?</summary></details>
<details> <summary>What is polymorphism?</summary></details>
<details> <summary>What is tight coupling?</summary></details>
<details> <summary>What are design patterns and how they are good?</summary></details>
<details> <summary>Tell me some important design patterns used in iOS?</summary></details>
<details> <summary>What is singleton?</summary></details>
<details> <summary>What challenges you have encounter implementing MVC?</summary></details>
<details> <summary>What is MVVM and why you will use it?</summary></details>
<details> <summary>What is dependency injection, how it is good?</summary></details>

* [Behavioral](#behavioral)
* [Creational](#creational)
* [Structural](#structural)

Behavioral
==========

>In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.
>
>**Source:** [wikipedia.org](http://en.wikipedia.org/wiki/Behavioral_pattern)

```swift

import Swift
import Foundation
```

👓 Observer
-----------

The observer pattern is used to allow an object to publish changes to its state.
Other objects subscribe to be immediately notified of any changes.

### Example

```swift

protocol PropertyObserver : class {
    func willChange(propertyName: String, newPropertyValue: Any?)
    func didChange(propertyName: String, oldPropertyValue: Any?)
}

final class TestChambers {

    weak var observer:PropertyObserver?

    private let testChamberNumberName = "testChamberNumber"

    var testChamberNumber: Int = 0 {
        willSet(newValue) {
            observer?.willChange(propertyName: testChamberNumberName, newPropertyValue: newValue)
        }
        didSet {
            observer?.didChange(propertyName: testChamberNumberName, oldPropertyValue: oldValue)
        }
    }
}

final class Observer : PropertyObserver {
    func willChange(propertyName: String, newPropertyValue: Any?) {
        if newPropertyValue as? Int == 1 {
            print("Okay. Look. We both said a lot of things that you're going to regret.")
        }
    }

    func didChange(propertyName: String, oldPropertyValue: Any?) {
        if oldPropertyValue as? Int == 0 {
            print("Sorry about the mess. I've really let the place go since you killed me.")
        }
    }
}
```

### Usage

```swift

var observerInstance = Observer()
var testChambers = TestChambers()
testChambers.observer = observerInstance
testChambers.testChamberNumber += 1
```

>**Further Examples:** [Design Patterns in Swift](https://github.com/kingreza/Swift-Observer)


Creational
==========

> In software engineering, creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. The basic form of object creation could result in design problems or added complexity to the design. Creational design patterns solve this problem by somehow controlling this object creation.
>
>**Source:** [wikipedia.org](http://en.wikipedia.org/wiki/Creational_pattern)

```swift

import Swift
import Foundation
```

🌰 Abstract Factory
-------------------

The abstract factory pattern is used to provide a client with a set of related or dependant objects. 
The "family" of objects created by the factory are determined at run-time.

### Example
 
Protocols

```swift

protocol 

imal {
    func stringValue() -> String
    // factory
    static func make(string : String) -> Decimal
}

typealias NumberFactory = (String) -> Decimal

// Number implementations with factory methods

struct NextStepNumber: Decimal {
    private var nextStepNumber: NSNumber

    func stringValue() -> String { return nextStepNumber.stringValue }
    
    // factory
    static func make(string: String) -> Decimal {
        return NextStepNumber(nextStepNumber: NSNumber(value: (string as NSString).longLongValue))
    }
}

struct SwiftNumber : Decimal {
    private var swiftInt: Int

    func stringValue() -> String { return "\(swiftInt)" }
    
    // factory
    static func make(string: String) -> Decimal {
        return SwiftNumber(swiftInt:(string as NSString).integerValue)
    }
}
```

Abstract factory

```swift

enum NumberType {
    case nextStep, swift
}

enum NumberHelper {
    static func factory(for type: NumberType) -> NumberFactory {
        switch type {
        case .nextStep:
            return NextStepNumber.make
        case .swift:
            return SwiftNumber.make
        }
    }
}
```

### Usage

```swift

let factoryOne = NumberHelper.factory(for: .nextStep)
let numberOne = factoryOne("1")
numberOne.stringValue()

let factoryTwo = NumberHelper.factory(for: .swift)
let numberTwo = factoryTwo("2")
numberTwo.stringValue()
```

💍 Singleton
------------

The singleton pattern ensures that only one object of a particular class is ever created.
All further references to objects of the singleton class refer to the same underlying instance.
There are very few applications, do not overuse this pattern!

### Example:

```swift

class DeathStarSuperlaser {
    static let sharedInstance = DeathStarSuperlaser()

    private init() {
        // Private initialization to ensure just one instance is created.
    }
}
```

### Usage:

```swift

let laser = DeathStarSuperlaser.sharedInstance
 [Behavioral](Behavioral) |
 [Creational](Creational) |
 Structural
```

Structural
==========

>In software engineering, structural design patterns are design patterns that ease the design by identifying a simple way to realize relationships between entities.
>
>**Source:** [wikipedia.org](http://en.wikipedia.org/wiki/Structural_pattern)

```swift

import Swift
import Foundation
```

🍧 Decorator
------------

The decorator pattern is used to extend or alter the functionality of objects at run- time by wrapping them in an object of a decorator class. 
This provides a flexible alternative to using inheritance to modify behaviour.

### Example

```swift

protocol Coffee {
    func getCost() -> Double
    func getIngredients() -> String
}

class SimpleCoffee: Coffee {
    func getCost() -> Double {
        return 1.0
    }

    func getIngredients() -> String {
        return "Coffee"
    }
}

class CoffeeDecorator: Coffee {
    private let decoratedCoffee: Coffee
    fileprivate let ingredientSeparator: String = ", "

    required init(decoratedCoffee: Coffee) {
        self.decoratedCoffee = decoratedCoffee
    }

    func getCost() -> Double {
        return decoratedCoffee.getCost()
    }

    func getIngredients() -> String {
        return decoratedCoffee.getIngredients()
    }
}

final class Milk: CoffeeDecorator {
    required init(decoratedCoffee: Coffee) {
        super.init(decoratedCoffee: decoratedCoffee)
    }

    override func getCost() -> Double {
        return super.getCost() + 0.5
    }

    override func getIngredients() -> String {
        return super.getIngredients() + ingredientSeparator + "Milk"
    }
}

final class WhipCoffee: CoffeeDecorator {
    required init(decoratedCoffee: Coffee) {
        super.init(decoratedCoffee: decoratedCoffee)
    }

    override func getCost() -> Double {
        return super.getCost() + 0.7
    }

    override func getIngredients() -> String {
        return super.getIngredients() + ingredientSeparator + "Whip"
    }
}
```

### Usage:

```swift

var someCoffee: Coffee = SimpleCoffee()
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")
someCoffee = Milk(decoratedCoffee: someCoffee)
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")
someCoffee = WhipCoffee(decoratedCoffee: someCoffee)
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")
```

🎁 Façade
---------

The facade pattern is used to define a simplified interface to a more complex subsystem.
(REST api)

### Example

```swift

enum Eternal {

    static func set(_ object: Any, forKey defaultName: String) {
        let defaults: UserDefaults = UserDefaults.standard
        defaults.set(object, forKey:defaultName)
        defaults.synchronize()
    }

    static func object(forKey key: String) -> AnyObject! {
        let defaults: UserDefaults = UserDefaults.standard
        return defaults.object(forKey: key) as AnyObject!
    }

}
```

### Usage

```swift

Eternal.set("Disconnect me. I’d rather be nothing", forKey:"Bishop")
Eternal.object(forKey: "Bishop")
```

## Multithreading
<details> 
  <summary> Does iOS support multitasking? </summary>
iOS 4 and above supports multi-tasking and allows apps to remain in the background until they are launched again or until they are terminated.  
</details>





## Maps

## Testing


<details><summary> Do you write unit tests? how you write in iOS?</summary></details>
<details><summary> What is TDD?</summary></details>
<details><summary> How TDD is good, and What problem does it solve?</summary></details>
<details><summary> What is Mocking, when you will use mocking?</summary></summary></details>
<details><summary> What is UI testing?</summary></details>


## Algorithms

<details><summary>Given two linked lists, find the node at which they merge. </summary></details> 
<details><summary>Given an n x n grid, find the number of ways to get from the bottom left to the top right given that you can only move up or right each time and you cannot move to any node on the bottom diagonal half of the grid, i.e. row less than col.  </summary></details>
## Logical

## Code Puzzels

## Additional

## Podcasts

https://www.youtube.com/user/SiliconValleyVoice/playlists

## General

<details> 
  <summary> What’s the difference between the frame and the bounds? </summary>
   The bounds of an UIView is the rectangle, expressed as a location (x,y) and size (width,height) relative to its own coordinate system (0,0). 
The frame of an UIView is the rectangle, expressed as a location (x,y) and size (width,height) relative to the superview it is contained within.
</details>




## Swift

## Objective-C

<details><summary> What is the difference between underscore and self (i.e self.xx and _xx) ?</summary></details>
<details><summary> What is property?</summary></details>
<details><summary> What is the difference between weak and strong?</summary></details>
<details><summary> What is Retain Cycle? And how we can avoid it?</summary></details>
<details><summary> What does copy means while declaring a property?</summary></details>
<details><summary> What is synthesize and when we need to use it?</summary></details>
<details><summary> What is a category?</summary></details>
<details><summary> What is an extension?</summary></details>
<details><summary> How we can add a property in a category?</summary></details>
<details><summary> What is method swizzling and when we should use it?</summary></details>
<details><summary> How we can layout subviews in a view?</summary></details>
<details><summary> What are the size classes?</summary></details>
<details><summary> What is ARC?</summary></details>
<details><summary> What is @autorelease and when we should use it?</summary></details>
<details><summary> How to animate view with constraint?</summary></details>
<details><summary> What is core data?</summary></details>
<details><summary> What is core data stack?</summary></details>
<details><summary> What is managed object context?</summary></details>
<details><summary> How we can do multithreading with core data?</summary></details>
<details><summary> How to transfer manage object from one thread to another thread?</summary></details>
<details><summary> How to merge changes from one moc to another?</summary></details>
<details><summary> Difference between bounds and frame?</summary></details>
<details><summary> What does dispatch_once do?</summary></details>
<details><summary> What is delegate?</summary></details>
<details><summary> How we can do multithreading in iOS?</summary></details>
<details><summary> When you will use NSOperations over GCDs and vice versa?</summary></details>
<details><summary> What does alloc do?</summary></details>
<details><summary> How we can create a class in objective c which don’t inherit from NSObject?</summary></details>
<details><summary> What are the different application states in iOS?</summary></details>
<details><summary> How we can execute some code when app is in background?</summary></details>
<details><summary> How we can wait for some thread to finish before starting another?</summary></details>
<details><summary> How you will store user info (username, password or token) securely in iOS?</summary></details>




<details> 
  <summary> Whats the difference between frame and bounds? </summary>

The frame of a view is the rectangle, expressed as a location (x,y) and size (width,height) relative to the superview it is contained within. The bounds of a view is the rectangle, expressed as a location (x,y) and size (width,height) relative to its own coordinate system (0,0).
</details>

Name the framework that is used to construct application’s user interface for iOS
In addition to the core app behaviors, UIKit provides support for the following features:

 <details> 
  <summary>Whats a struct? </summary>
A struct is a special C data type that encapsulates other pieces of data into a single cohesive unit. Like an object, but built into C. </details>

<details> 
  <summary>
What are the different ways to specify layout of elements in UIView?</summary>
Here are a few common ways to specify the layout of elements in UIView:
• Using InterfaceBuilder, you can add a XIB file to your project, layout elements within it, and then load the XIB in your application code (either automatically, based on naming conventions, or manually). Also, using InterfaceBuilder you can create a storyboard for your application.

Can outlet be static?

How would you implement static blur effect to the UIView subclass to make it work fast (as of iOS 6-7 it was a problem)  

Describe managed object context and is function.
The key functions of managed object context include

What is the difference between atomic and non-atomic properties? Which is the default for synthesized properties? When would you use one over the other?</details>

<details> 
  <summary>Does Objective-C contain private methods?</summary>
NO, there is nothing called a private method in Object-C programming. If a method is defined in .m only then it becomes protected. If in .h,it is public.
If you really want a private method then you need to add a local category/ unnamed category/ class extension in the class and add the method in the category and define it in the class.m</details>

<details> 
  <summary>What is plist?</summary>
Plist refers to Property lists that organize data into named values and lists of values using several object types. These types provide you the means to produce data that is meaningfully structured, transportable, storable, and accessible, but still as efficient as possible. Property lists are frequently used by applications running on both Mac OS X and iOS. The property-list programming interfaces for Cocoa and Core Foundation allow you to convert hierarchically structured combinations of these basic types of objects to and from standard XML. You can save the XML data to disk and later use it to reconstruct the original objects.
The user defaults system, which you programmatically access through the NSUserDefaults class, uses property lists to store objects representing user preferences. This limitation would seem to exclude many kinds of objects like as NSColor and NSFont objects, from the user default system. But if objects conform to the NSCoding protocol they can be archived to NSData objects, which are property list–compatible objects</details>

What is the purpose of reuseIdentifier? What is the benefit of setting it to a non-nil value?

<details> 
  <summary>What is the difference between an “app ID” and a “bundle ID” and what is each used for?</summary>
Since most users consider the App ID as string, they think it is interchangeable with Bundle ID. Once the App ID is created in the Member Center, you can only use the App ID Prefix that matches the Bundle ID of the Application Bundle.
The bundle ID uniquely defines each App. It is specified in Xcode. A single Xcode project can have multiple targets and therefore, output multiple apps. A common use case for this – An app having both lite/free and pro/full versions or branded multiple ways.</details>

<details> 
  <summary>What is Abstract class in Cocoa?</summary>
Cocoa doesn’t provide anything called abstract. We can create a class abstract that gets checked only at runtime while it is not checked at compile time.
@interface AbstractClass : NSObject
@end
@implementation AbstractClass
+ (id)alloc{
    if (self == [AbstractClass class]) {
        NSLog(@"Abstract Class can’t be used");
    }
    return [super alloc];
@end</details>

<details> 
  <summary>What is NSURLConnection class? Define its types and use case.</summary>
There are two ways of using NSURLConnection class. One is asynchronous and the other is synchronous.
An asynchronous connection will create a new thread and performs its download process on the new thread. A synchronous connection will block the calling thread while downloading content and doing its communication.
Many developers think that a synchronous connection blocks the main thread, which is not true. A synchronous connection will always block the thread from which it is fired. If you fire a synchronous connection from the main thread, the main thread will be blocked. But, if you fire a synchronous connection from a thread other than the main thread, it will be like an asynchronous connection and won’t block your main thread.
In fact, the only difference between a synchronous and an asynchronous connection is that at runtime, a thread will be created for the asynchronous connection while it won’t do the same for a synchronous connection.
In order to create an asynchronous connection, we need to do the following:
1. Have our URL in an instance of NSString
2. Convert our string to an instance of NSURL
3. Place our URL in a URL Request of type NSURLRequest, or in the case of mutable URLs, in an instance of NSMutableURLRequest.
4. Create an instance of NSURLConnection and pass the URL request to it.</details>

<details> 
  <summary>What is the relation between iVar and @property?</summary>
iVar is an instance variables. It cannot be accessed unless we create accessors, which are generated by @property. iVar and its counterpart @property can be of different names.
@interface Box : NSObject{
    NSString *boxName;
}
@property (strong) NSString *boxDescription;//this will become another ivar
-(void)aMethod;
@end
@implementation Box
@synthesize boxDescription=boxName;//now boxDescription is accessor for name
-(void)aMethod {
    NSLog(@"name=%@", boxName);
     NSLog(@"boxDescription=%@",self.boxDescription);
    NSLog(@"boxDescription=%@",boxDescription); //throw an error
}
@end</details>

<details> 
  <summary>
What happens if IApplication object does not handle an event?
</summary>
In such case the event will be dispatched to your app delegate for processing.</details>

<details> 
  <summary>
Name the framework that is used to construct application's user interface for iOS?
</summary>
The UIKit framework is used to develop application's user interface for iOS. UIKit framework provides event handling, drawing model, windows, views, and controls specifically designed for a touch screen interface.</details>

<details> 
  <summary>
How is the app delegate is declared by Xcode project templates?
</summary>
App delegate is declared as a subclass of UIResponder by Xcode project templates.</details>

<details> 
  <summary>
How can you respond to state transitions on your app?
</summary>
On state transitions can be responded to state changes in an appropriate way by calling corresponding methods on app's delegate object.

For example:
application Did Become Active method can be used to prepare to run as the foreground app.
application Did Enter Background method can be used to execute some code when app is running in the background and may be suspended at any time.
application Will Enter Foreground method can be used to execute some code when your app is moving out of the background application Will Terminate method is called when your app is being terminated.</details>

<details> 
  <summary>
When an app is said to be in active state?
</summary>
An app is said to be in active state when it is running in foreground and is receiving events.</details>

<details> 
  <summary>
Assume that your app is running in the foreground but is currently not receiving events. In which sate it would be in?
</summary>
An app will be in InActive state if it is running in the foreground but is currently not receiving events. An app stays in InActive state only briefly as it transitions to a different state.</details>

<details> 
  <summary>
Give example scenarios when an application goes into InActive state?
</summary>
An app can get into InActive state when the user locks the screen or the system prompts the user to respond to some event e.g. SMS message, incoming call etc.</details>

<details> 
  <summary>
List down apps state transitions when it gets launched?
</summary>
Before the launch of an app, it is said to be in not running state.
When an app is launched, it moves to the active or background state, after transitioning briefly through the inactive state.</details>

<details> 
  <summary>
Who calls the main function of you app during the app launch cycle?
</summary>
During app launching, the system creates a main thread for the app and calls the app's main function on that main thread. The Xcode project's default main function hands over control to the UIKit framework, which takes care of initializing the app before it is run.</details>

<details> 
  <summary>How can you respond to state transitions on your app?</summary>
State transitions can be responded to state changes in an appropriate way by calling corresponding methods on app’s delegate object.
For example:
applicationDidBecomeActive( ) method can be used to prepare to run as the foreground app.
applicationDidEnterBackground( ) method can be used to execute some code when app is running in the background and may be suspended at any time.
applicationWillEnterForeground( ) method can be used to execute some code when your app is moving out of the background
applicationWillTerminate( ) method is called when your app is being terminated.</details>

<details> 
  <summary>What is the difference between retain & assign?</summary>
Assign creates a reference from one object to another without increasing the source’s retain count.
if (_variable != object)
{   
 [_variable release];  
  _variable = nil;  
  _variable = object;
 }
Retain creates a reference from one object to another and increases the retain count of the source object.
if (_variable != object)
{
  [_variable release];
    _variable = nil;  
_variable = [object retain];  
}</details>

<details> 
  <summary>
Whats fast enumeration?
</summary>
Fast enumeration is a language feature that allows you to enumerate over the contents of a collection. (Your code will also run faster because the internal implementation reduces
message send overhead and increases pipelining potential.)</details>
<details> 
  <summary>
What is iphone sdk?
</summary>
iPhone SDK is available with tools and interfaces needed for developing, installing and running custom native applications.
Native applications are built using the iPhone OS’s system frameworks and Objective-C language and run directly on iPhone OS.
Native applications are installed physically on a device and can run in presence or absence of network connection.</details>

<details> 
  <summary>
What is an iPhone app?
</summary>
An iPhone app is a program that runs on our iPhone/iPod Touch. It enables us to ccomplish a certain task. They could be utility apps, games, enterprise apps, entertainment apps, apps to access our bank account etc.</details>
<details> 
  <summary>
What is iphone architecture?
</summary>
It is similar to MacOS X architecture
It acts as an intermediary between the iPhone and iPod hardware an the appearing applications on the screen
The user created applications never interact directly with the appropriate drivers, which protects the user applications from changes to the hardware.</details>

<details> 
  <summary>
Introduction to Iphone application Development?
</summary>
In 2007, Apple entered the cellular phone business with the introduction of the iPhone, a multi-touch display cell phone, which also includes the features of iPod.</details>

<details> 
  <summary>
What is iphone reference library?
</summary>
iPhone reference library is a set of reference documents for iPhone OS .
It can be downloaded by subscribing to the iPhone OS Library doc set.
Select Help>Documentation from Xcode, and click the subscribe button next to the iPhone OS Library doc set, which appears in the left column.</details>

<details> 
  <summary>
How many bytes we can send to apple push notification server.?
</summary>
256bytes.</details>

<details> 
  <summary>
What are sensors in iphone?
</summary>
The proximity sensor immediately turns off the display when the iPhone is lifted to ear. With this sensor the power is saved and accidental dialing is prevented.
The display is automatically brightens the iPhone by the ambient light sensor when the sunlight or bright rooms and dims in darker places.</details>




<details> 
  <summary>
Which object manage the presentation of apps content on the screen?
</summary>
View controller objects takes care of the presentation of app's content on the screen. A view controller is used to manage a single view along with the collection of subviews. It makes its views visible by installing them in the app's window.</details>

  <summary>
How to change the content of your app in order to change the views displayed in the corresponding window?
</summary>
To change the content of your app, you use a view controller to change the views displayed in the corresponding window. Remember, window itself is never replaced.</details>

<details> 
  <summary>
Difference between shallow copy and deep copy?
</summary>
Shallow copy is also known as address copy. In this process you only copy address not actual data while in deep copy you copy data.
Suppose there are two objects A and B. A is pointing to a different array while B is pointing to different array. Now what I will do is following to do shallow copy.
Char *A = {‘a','b','c'};
Char *B = {‘x','y','z'};
B = A;
Now B is pointing is at same location where A pointer is pointing.Both A and B in this case sharing same data. if change is made both will get altered value of data.Advantage is that coping process is very fast and is independent of size of array.
while in deep copy data is also copied. This process is slow but Both A and B have their own copies and changes made to any copy, other will copy will not be affected.</details>

<details> 
  <summary>
Which app specific objects store the apps content?
</summary>
Data model objects are app specific objects and store app's content. Apps can also use document objects to manage some or all of their data model objects.</details>

<details> 
  <summary>
Name the application thread from where UIKit classes should be used?
</summary>
UIKit classes should be used only from an application's main thread. Note: The derived classes of UIResponder and the classes which manipulate application's user interface should be used from application's main thread.</details>

<details> 
  <summary>
What are the location services?
</summary>
Applications such as Maps, camera and compass are allowed to use the information from cellular, Wi-Fi and Global Positioning System networks for determining the approximate locations.
The location is displayed on the screen, using a blue marker.</details>


<details> 
  <summary>
Describe the functionality of accelerometer of an iphone
</summary>
iPhone responds to motion using a built-in accelerometer.
The accelerometer detects the movement and changes the display accordingly, at the time of rotating iPhone from portrait to landscape.</details>
<details> 
  <summary>
Where can you test Apple iPhone apps if you don't have the device?
</summary>
iOS Simulator can be used to test mobile applications. Xcode tool that comes along with iOS SDK includes Xcode IDE as well as the iOS Simulator. Xcode also includes all required tools and frameworks for building iOS apps. However, it is strongly recommended to test the app on the real device before publishing it.</details>

<details> 
  <summary>What are the features added in iOS 9?</summary>
• Intelligent Search and Siri- Intelligent Search is an excellent mechanism to learn user habits and act on that information- open apps before we need them, make recommendations on places we might like and guide us through our daily lives to make sure we’re where we need to be at the right time.
Siri is a personal assistant to the users, able to create contextual reminders and search through photos and videos in new ways. Swiping right from the home screen also brings up a new screen that houses “Siri Suggestions,” putting favorite contacts and apps right on your fingertips, along with nearby restaurant and location information and important news.
</details>


<details> 
  <summary>Explain Singleton class.</summary>
Only one instance of that class is created in the application.
@interface SomeManager : NSObject
             + (id)singleton;
 @end
 @implementation SomeManager
            + (id)singleton {    
                                 static id sharedMyManager = nil; 
                                 @synchronized([MyObject class]){ 
                                                     if (sharedMyManager == nil) { 
                                                                         sharedMyManager = [[self alloc] init]; 
                                                      } 
                                 }
                                return sharedMyManager;
            }
 @end
//using block
+ (id) singleton {
    static SomeManager *sharedMyManager = nil;
    static dispatch_once_t  onceToken;
    dispatch_once(&onceToken, ^{
        sharedMyManager = [[self alloc] init];
    });
    return sharedMyManager;</details>

## Code

## HomeWork

## Job websites

## Companies

## Stories

## Process

## Credits
https://github.com/ochococo/Design-Patterns-In-Swift#creational
https://github.com/soapyigu/LeetCode_Swift/blob/master/README.md
https://github.com/9magnets/iOS-Developer-and-Designer-Interview-Questions#general
https://github.com/durul/My-Medium-Articles
https://github.com/5uper0/ios-science
http://www.geekinterview.com/Interview-Questions/iOS
github.com


