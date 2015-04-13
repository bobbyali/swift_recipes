# Swift iOS Code Recipes
This page contains minimal code snippets for programatically implementing aspects of iOS applications using the Swift programming language.

The idea of these recipes is to provide a compact reference document for quickly looking up how to do X using Swift. It assumes that you already know the basics of programming and iOS development. Explanations are left out in order to keep the pages as compact as possible. For tutorials on how to use these, you should look at  [Apple's Swift documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/) (for an intro to Swift) and a good iOS app development tutorial such as this one by [Ray Wenderlich](http://www.raywenderlich.com/1797/ios-tutorial-how-to-create-a-simple-iphone-app-part-1).

The recipes provided here are by no means comprehensive - they simply cover those that I've come across whilst learning iOS development.

The code recipes are split into the following categories:

* [Swift language basics](#swift)
* [iOS development](#ios)
* [Design patterns](#patterns)

# <a name="swift"/>Swift language basics





# <a name="ios"/>iOS development

## GUI Layout 

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

		var screenSize: CGRect = UIScreen.mainScreen().bounds

### Collection View

        let layout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()
        layout.sectionInset = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
        layout.itemSize = CGSize(width: 120, height: 90)

		collectionView = UICollectionView(frame: self.view.frame, collectionViewLayout: layout)
        collectionView!.dataSource = self
        collectionView!.delegate = self
        collectionView!.registerClass(UICollectionViewCell.self, forCellWithReuseIdentifier: reuseIdentifier)
        collectionView!.backgroundColor = UIColor.blackColor()
        self.view.addSubview(collectionView!)

### Table View

        tableView = UITableView(frame: self.view.frame, style: .Plain)
        self.view.addSubview(tableView)
        tableView.delegate = self
        tableView.dataSource = self
        tableView.registerClass(UITableViewCell.self, forCellReuseIdentifier: cellIdentifier)        

### Image View
	
		var frame = self.view.frame
        someImage = UIImageView(frame: CGRect(x: 0, y: 0, width: frame.size.width, height: frame.size.height))


### Transitions

	Pushes

        let vc = SettingsViewController()
        self.navigationController?.pushViewController(vc, animated: true)

    Segues (have to use IB to set up segue identifiers)


### Constraints with Masonry Snap

        let padding = UIEdgeInsetsMake(10, 10, 10, -50)
        let superview = self.view
        
        someView.snp_makeConstraints { make in
            make.top.equalTo(superview.snp_top).with.offset(padding.top)
            make.left.equalTo(superview.snp_left).with.offset(padding.left)
        }


## GUI Events

### Gray Activity Indicator View

        var activityIndicatorView = UIActivityIndicatorView(activityIndicatorStyle: .Gray)
        view.addSubview(activityIndicatorView)
        activityIndicatorView.center = view.center
        activityIndicatorView.startAnimating()

        activityIndicatorView.removeFromSuperview()

### Touches

        press = UILongPressGestureRecognizer(target: self, action: "doSomething:")
        press.minimumPressDuration = 0.5
        press.numberOfTapsRequired = 0
        press.numberOfTouchesRequired = 1
        press.delegate = self
        self.view.addGestureRecognizer(press)


### Notifications

        NSNotificationCenter.defaultCenter().addObserver(
            self,
            selector: "doSomething:",
            name: someKey,
            object: nil)

		func doSomething(notification: NSNotification) {	        
	    }

### App Delegate setup

        let rootViewController = SomeViewController()
        let frame = UIScreen.mainScreen().bounds
        self.window = UIWindow(frame: frame)
        let navController = UINavigationController(rootViewController: rootViewController)
        self.window?.rootViewController = navController
        self.window?.backgroundColor = UIColor.whiteColor()
        self.window?.makeKeyAndVisible()

### Delegates

		protocol NewDelegate {
		    func doSomething()
		}

		In class:

			var delegate: NewDelegate?

			self.delegate?.doSomething()

		In target class:

			Inherit from NewDelegate
			newObject.delegate = self
			Implement the protocol methods






# <a name="patterns"/>Design patterns

