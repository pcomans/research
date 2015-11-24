//“ this is what closures look like:”

let aClosure : Void -> Int = { return 1 }

aClosure() // returns 1

//“In Objective-C, closures are known as blocks. For this reason, several methods that belong to Cocoa and Cocoa Touch contain the word block, where you should provide a closure”

// In this code, aViewController and anotherViewController
// are both UIViewControllers.

// Slide up a view controller, and then when the slide animation is
// finished, change its view's background color to yellow.

aViewController.presentViewController(anotherViewController, animated: true) {
    // This closure is run after the animation is finished
    anotherViewController.view.backgroundColor = UIColor.yellowColor()
}


Blocks/Closures
Basically this involves passing in a function/method/message to the child, whilst maintaining the focus of the parent. So it would be a method that the child can call, and local variables within the closure are based on local variables where the closure is defined.

```swift
import Cocoa
var parent:Parent=Parent()
parent.go()  //Note that this outputs I am Parent
class Parent {
    var string="I am Parent"
    var child = Child()
    func parentMethod()->NSString {
        return string
    }
    func go()->NSString {
        return child.runThis(parentMethod)
    }
}
class Child {
    var string="I am Child"
    func runThis(closure: () -> NSString)->NSString {
        return closure()
    }
}
```