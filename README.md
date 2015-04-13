# Swift iOS Code Recipes
This page contains minimal code snippets for programatically implementing aspects of iOS applications using the Swift programming language.

The idea of these recipes is to provide a compact reference document for quickly looking up how to do something using Swift. It assumes that you already know the basics of programming and iOS development. Explanations are left out in order to keep the pages as compact as possible. You'll also need to modify them to suit your purposes. (If you don't know how to use these, you'll want to study some introductory tutorials e.g. [Apple's Swift documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/), and [Ray Wenderlich's iOS development tutorial](http://www.raywenderlich.com/1797/ios-tutorial-how-to-create-a-simple-iphone-app-part-1).)

The recipes provided here are far from comprehensive - they simply cover those that I've come across whilst I was learning iOS development during an [online Swift/iOS bootcamp](http://www.hacker-dad.com/sprint-2-ios-development/). Likewise, there's no guarantee that these are correct or examples of best practice. If you see something that needs improving, please let me know. Corrections and pull requests would be greatly appreciated. 

The code recipes are split into the following categories:

* [Swift language reference](#swift) - a very quick summary of the language.
	* [Variables and Constants](#vars)
	* [Operators](#operators)
	* [Inputs and Outputs](#io)
	* [Collections](#collections)
	* [Control Flow](#flow)
	* [Functions](#functions)
	* [Classes and Structs](#oop)
* [iOS development reference](#ios) - basic GUI features that you could create with Interface Builder.
	* [GUI Layout](#gui_layout)
	* [GUI Events](#gui_events)
* [Design patterns reference](#patterns) - simple Swift implementations of the main Gang Of Four patterns.
	* [Singleton](#singleton)

# <a name="swift"/>Swift language basics


## <a name="vars"/>Variables and Constants

Variables vs Constants

```swift
var variableName = someVariable
let constantName = someConstant
```

Types

```swift
String, Int, UInt
Float	 	// (32 bit)
Double 	// (64 bit, default for type inference)
Bool
```

Integer literals can be written as:
*	A decimal number, with no prefix
*	A binary number, with a 0b prefix
*	An octal number, with a 0o prefix
*	A hexadecimal number, with a 0x prefix

Initialising variables 

```swift
var milesRunToday = 3
var milesRunToday: Double = 3 	// type annotation
```
Note that you need to do a specific cast if you need floats or ints, because otherwise initial assignment will be locked in as int.

Optionals

```swift
var serverResponseCode: Int? = 404
serverResponseCode = nil
var surveyAnswer: String?  	// default value is nil
```

Force unbinding / unwrapping (need to do this to get the data from optional variables). Use when you know that varName is not nil.

```swift
varName!
```


## <a name="operators"/>Operators

Ternary operators
```swift
hasHeader ? 50 : 20
```

Nil coalescing operator
```swift
a ?? b 	// means a != nil ? a! : b
```

Range operators

```swift
a..b  		// closed range, returns (a, a+1, a+2, ..., b-1, b)
a..<b 		// half open range, returns ((a, a+1, a+2, ..., b-1)
```

Half-open ranges are great for zero-index arrays, where you don't want to call the length of the list.


## <a name="io"/>Inputs and Outputs

```swift
println 	// prints with a carriage return
print 		// no carriage return
println("\(var1)\(var2)")  // string interpolation
println("+ Signs can concatenate strings with " + otherStrings)
println("Can also concatenate by ").append("doing this")
```

## <a name="collections"/>Collections

Arrays - all items are of same type.

```swift
var shoppingList: [String] = ["Eggs", "Milk"]

var someInts = [Int]()  		// empty array
var threeDoubles = [Double](count: 3, repeatedValue: 0.0)    // repeated zeros array

for item in shoppingList {
	...
}

for (index, value) in enumerate(shoppingList) {
    println("Item \(index + 1): \(value)")
}
```

Dictionaries - handles key-value pairings of the same type.

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
var namesOfIntegers = [Int: String]()

for (airportCode, airportName) in airports {
	println("\(airportCode): \(airportName)")
}

for airportCode in airports.keys {
	println("Airport code: \(airportCode)")
}
```

Tuples - groups together items of different types.
```swift
let http404Error = (404, "Not Found")
	
let (statusCode, statusMessage) = http404Error
let (justTheStatusCode, _) = http404Error
println("The status code is \(http404Error.0)")
let http200Status = (statusCode: 200, description: "OK")
println("The status code is \(http200Status.statusCode)")
```

## <a name="flow"/>Control Flow

For loops.

```swift
for index in 1...5 {
    println("\(index) times 5 is \(index * 5)")
}

for var index = a; index < x; ++index {
    //do some code here
}

for _ in 1...power {
    answer *= base 		// don't need the iterating index variable
}
```

(Do-)While loops.

```swift
while (x > 5) {
}

do {
} while (x < 10)
```

If statements.

```swift
if () {
	...
} else if () {
	...
} else {
	
}
```

Switches.

```swift
switch (someCharacter) {
	case "a":
	default:
}	 	// no explicit break needed in a match (use fallthrough to override)

// can do interval matching
switch count {
	case 0:
	    naturalCount = "no"
	case 1...10:
	    naturalCount = "a few"
	default:
	    naturalCount = "lots"
}

// can use with tuples
let somePoint = (1, 1)
switch somePoint {
	case (0, 0):
	    println("(0, 0) is at the origin")
	case (_, 0):
	    println("(\(somePoint.0), 0) is on the x-axis")
	case (0, _):
	    println("(0, \(somePoint.1)) is on the y-axis")
	case (-2...2, -2...2):
	    println("(\(somePoint.0), \(somePoint.1)) is inside the box")
	default:
	    println("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
```

## <a name="functions"/>Functions

One input, no output.

```swift
func checkValue(counter: Int) {
    ...
}
```

One input, one output.

```swift
func sayHello(personName: String) -> String {
    let greeting = "Hello, " + personName + "!"
    return greeting
}
```

One input, multiple outputs via tuple.

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

Can make the return tuple completely optional by 

```swift
(min: Int, max: Int)?
```

Can add external parameter names
```swift
func join(string s1: String, toString s2: String, withJoiner joiner: String) -> String {
	return s1 + joiner + s2
}

join(string: "hello", toString: "world", withJoiner: ", ")
```

Can use the internal param name as the external name by prefixing with #

```swift
func containsCharacter(#string: String, #characterToFind: Character) -> Bool {
		    ...
}
```

External name given automatically if default value defined

```swift
func join(s1: String, s2: String, joiner: String = " ") -> String {
	return s1 + joiner + s2
}

join("hello", "world", joiner: "-")
```

Variadic parameters (can have zero or more of same type, must appear last)

```swift
func arithmeticMean(numbers: Double...) -> Double {
	for number in numbers {
		total += number
	}
}

arithmeticMean(1, 2, 3, 4, 5)
arithmeticMean(3, 8.25, 18.75)
```

Can have in-out parameters

```swift
func swapTwoInts(inout a: Int, inout b: Int) {
	let temporaryA = a
	a = b
	b = temporaryA
}

swapTwoInts(&someInt, &anotherInt)
```

Function Types

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

## <a name="oop"/>Classes, Objects and Structures

Simple class

```swift
class Shape {
    var color:UIColor
    init(color:UIColor) {
        self.color = color
    }
    func getArea() -> Double {
        return 0
    }
}
```

Inheritance.

```swift
class Circle: Shape {
    var radius:Int
    init(radius:Int, color:UIColor) {
        self.radius = radius
        super.init(color: color)
    }
    override func getArea() -> Double {
        return M_PI * Double(self.radius * self.radius)
    }
}

var circle:Circle = Circle(radius: 2, color: UIColor.redColor())
circle.color
print(circle.getArea())
```

Structures can't inherit, override methods etc.

Structures are a VALUE TYPE, whereas classes are a REFERENCE TYPE.

Structures have initializers, but it can create its own (a memberwise initializer).

e.g. [CGPoint](https://developer.apple.com/Library/ios/documentation/GraphicsImaging/Reference/CGGeometry/index.html#//apple_ref/c/tdef/CGPoint) (core graphics point)
		

# <a name="ios"/>iOS development

## <a name="gui_layout"/>GUI Layout 

### Alerts

```swift 
var alert = UIAlertController(title: "Alert", message: "Message", preferredStyle: UIAlertControllerStyle.Alert)
alert.addAction(UIAlertAction(title: "Click", style: UIAlertActionStyle.Default, handler: nil))
self.presentViewController(alert, animated: true, completion: nil)
```

### Buttons on navigation bar

```swift 
let newButton = UIBarButtonItem(barButtonSystemItem: .Edit, target: self, action: "doSomething:")
self.navigationItem.leftBarButtonItem = newButton
```

### Button

Define button.

```swift
let button   = UIButton.buttonWithType(UIButtonType.System) as UIButton
button.frame = CGRectMake(100, 100, 100, 50)
button.backgroundColor = UIColor.greenColor()
button.setTitle("Test Button", forState: UIControlState.Normal)
button.addTarget(self, action: "buttonAction:", forControlEvents: UIControlEvents.TouchUpInside)

button.layer.cornerRadius = 5
button.layer.borderWidth = 1
button.layer.borderColor = UIColor.blackColor().CGColor
			    
self.view.addSubview(button)
```

Define target action called by button.

```swift
func buttonAction(sender:UIButton!) {
}	    
```

### Labels

```swift
newLabel = UILabel(frame: CGRect(x: 20, y: 10, width: 300, height: 200))
newLabel.text = "Tap and hold for settings"
newLabel.textColor = UIColor.whiteColor()
newLabel.textAlignment = NSTextAlignment.Center
newLabel.font = UIFont(name: "HelveticaNeue", size: CGFloat(17))
```

### Slider

Define slider control.

```swift
var slider = UISlider(frame: CGRectMake(0, 0, 300, 200))
slider.addTarget(self, action: "changeSomething:", forControlEvents: UIControlEvents.ValueChanged)
slider.backgroundColor = UIColor.blackColor()
slider.minimumValue = 0.0
slider.maximumValue = 1.0
slider.continuous = true
slider.value = 0.5
self.view.addSubview(slider)
```

Implement the target action.

```swift
func changeSomething(sender:UISlider!) {
	self.someValue = sender.value
}
```

### Dimensions

```swift
var screenSize: CGRect = UIScreen.mainScreen().bounds
```

### Collection View

```swift
let layout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()
layout.sectionInset = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
layout.itemSize = CGSize(width: 120, height: 90)

collectionView = UICollectionView(frame: self.view.frame, collectionViewLayout: layout)
collectionView!.dataSource = self
collectionView!.delegate = self
collectionView!.registerClass(UICollectionViewCell.self, forCellWithReuseIdentifier: reuseIdentifier)
collectionView!.backgroundColor = UIColor.blackColor()
self.view.addSubview(collectionView!)
```

### Table View

```swift
tableView = UITableView(frame: self.view.frame, style: .Plain)
self.view.addSubview(tableView)
tableView.delegate = self
tableView.dataSource = self
tableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: cellIdentifier)        
```

### Image View
	
```swift
var frame = self.view.frame
someImage = UIImageView(frame: CGRect(x: 0, y: 0, width: frame.size.width, height: frame.size.height))
```

### Transitions

Pushes.

```swift
let vc = SettingsViewController()
self.navigationController?.pushViewController(vc, animated: true)
```

Segues. Need to use Interface Builder to set up the segue identifier.

```swift
override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
	if (segue.identifier == "aSegueIdentifier") {
		let newViewController = segue.destinationViewController as NewViewController
	}
}
```

### Constraints with Masonry Snap

[Masonry Snap](https://github.com/Masonry/Snap is a great CocoaPod for quickly defining constraints.

```swift
let padding = UIEdgeInsetsMake(10, 10, 10, -50)
let superview = self.view
        
someView.snp_makeConstraints { make in
	make.top.equalTo(superview.snp_top).with.offset(padding.top)
	make.left.equalTo(superview.snp_left).with.offset(padding.left)
}
```

## <a name="gui_events"/>GUI Events

### Gray Activity Indicator View

To start the grey spinner.

```swift
var activityIndicatorView = UIActivityIndicatorView(activityIndicatorStyle: .Gray)
view.addSubview(activityIndicatorView)
activityIndicatorView.center = view.center
activityIndicatorView.startAnimating()
```

To stop the spinner.

```swift
activityIndicatorView.removeFromSuperview()
```

### Touches

To implement a long tap-and-hold gesture.

```swift
press = UILongPressGestureRecognizer(target: self, action: "doSomething:")
press.minimumPressDuration = 0.5
press.numberOfTapsRequired = 0
press.numberOfTouchesRequired = 1
press.delegate = self
self.view.addGestureRecognizer(press)
```

### Notifications

To register a notification.

```swift
NSNotificationCenter.defaultCenter().addObserver(
	self,
   selector: "doSomething:",
   name: someKey,
   object: nil)
```

To invoke a notification.

```swift
NSNotificationCenter.defaultCenter().postNotificationName(mySpecialNotificationKey, object: nil)
```

To define the target action for a notification.

```swift
func doSomething(notification: NSNotification) {
}
```

### App Delegate setup

```swift
let rootViewController = SomeViewController()
let frame = UIScreen.mainScreen().bounds
self.window = UIWindow(frame: frame)
let navController = UINavigationController(rootViewController: rootViewController)
self.window?.rootViewController = navController
self.window?.backgroundColor = UIColor.whiteColor()
self.window?.makeKeyAndVisible()
```

### Delegates

To define the protocol for a delegate.

```swift
protocol NewDelegate {
	func doSomething()
}
```

To set up a delegate in the parent class.

```swift
var delegate: NewDelegate?
...
self.delegate?.doSomething()
```
To set up a delegate in the target class:

```swift
// Set up target class to inherit NewDelegate
newObject.delegate = self
/// Implement the protocol methods
func doSomething() {
}
```


# <a name="patterns"/>Design patterns

## <a name="singleton"/>Singleton

This example works in most versions of Swift. For variations, see this [hpique/SwiftSingleton](https://github.com/hpique/SwiftSingleton) Github repo.

```swift
class SomeClass {
    class var sharedInstance: SomeClass {
        struct Static {
            static let instance: SomeClass = SomeClass()
        }
        return Static.instance
    }
}
```