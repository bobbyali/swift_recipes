# Swift iOS Code Recipes
This page contains minimal code snippets for programatically implementing aspects of iOS applications using the Swift programming language.

The idea of these recipes is to provide a compact reference document for quickly looking up how to do something using Swift. It assumes that you already know the basics of programming and iOS development. Explanations are left out in order to keep the pages as compact as possible. You'll also need to modify them to suit your purposes. (If you don't know how to use these, you'll want to study some introductory tutorials e.g. [Apple's Swift documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/), and [Ray Wenderlich's iOS development tutorial](http://www.raywenderlich.com/1797/ios-tutorial-how-to-create-a-simple-iphone-app-part-1).)

The recipes provided here are far from comprehensive - they simply cover those that I've come across whilst I was learning iOS development during an [online Swift/iOS bootcamp](http://www.hacker-dad.com/sprint-2-ios-development/). Likewise, there's no guarantee that these are correct or examples of best practice. If you see something that needs improving, please let me know. Corrections and pull requests would be greatly appreciated. 

The code recipes are split into the following categories:

* [Swift language reference](#swift) - a very quick summary of the language.
* [iOS development reference](#ios) - basic GUI features that you could create with Interface Builder.
	* [GUI Layout](#gui_layout)
	* [GUI Events](#gui_events)
* [Design patterns reference](#patterns) - simple Swift implementations of the main Gang Of Four patterns.
	* [Singleton](#singleton)

# <a name="swift"/>Swift language basics





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

Segues (have to use IB to set up segue identifiers).

```swift

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