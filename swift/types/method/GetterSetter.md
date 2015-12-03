
NOTE: consider using structs 


```swift
var x:Int

var xTimesTwo:Int {
    set {
       x = newValue / 2
    }
    get {
        return x * 2
    }
}


var diameter: Double {
  return radius * 2
}

//also:

var diameter: Double {
  get {
    return radius * 2
  }
}
```


**Overriding variables**

```swift
class A{
    var x:CGFloat = 0
}
class B:A{
    override var x:CGFloat {get{return super.x}set{super.x = newValue}}
}


let b:B = B()
b.x = 5
b.x//outputs 5
```

**private setter public getter variable swift**

private(set) public var readonlyProperty: Int

//for none framework code:
private(set) internal var readonlyProperty: Int