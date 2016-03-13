- I dont think you should use type alias too much, but here is a good example: (I wonder what scope type alias works under)
- Use regular variable outside classes if you want to define global variables
```swift
//TODO research more about typealias use cases



typealias returnedFunctionType = (Int) -> String

func myFuncThatReturnsAFunc() -> returnedFunctionType {//I think type aliases just makes return types more readable
    return { number in
        return "The lucky number is \(number)"
    }
}

let returnedFunction = myFuncThatReturnsAFunc()

func test(){
    returnedFunction(5) // The lucky number is 5
}

```