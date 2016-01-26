```swift
func test(){
    var array:Array = ["a","c","b"]
    array.sortInPlace(>)
    /*
    
    not working
    
    array.sort({
        $0.name < $1.name
    })
    */
}

func doSomethingToNumber(aNumber: Int, thingToDo: (Int)->Int) -> Int {
    // we've received some function as a parameter, which we refer to as
    // 'thingToDo' inside this function.

    // call the function 'thingToDo' using 'aNumber', and return the result
    return thingToDo(aNumber);
}

```

Better example:

```swift
/**
 * we've received some function as a parameter, which we refer to as
 */
func doSomethingToNumber(aNumber: Int, _ thingToDo: (Int)->Int) -> Int {
    return thingToDo(aNumber);// calls the function 'thingToDo' using 'aNumber', and return the result
}

func someMethod(aNum:Int)->Int{
    return aNum + aNum
}

doSomethingToNumber(4, someMethod)//8
```

NOTE: 
You can also wrap the entire argument and make it optional like:

```swift
(thingToDo: (Int)->Int)? = nil/*this is similar to doing: a:String? = nil*/
```