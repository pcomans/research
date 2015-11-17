
//consider using structs 


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