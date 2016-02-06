Selectors are usualy used when using NSTimer etc (there is an overview of all apple methods that uses a selector somewhere in your docs)

selectors can be used to check if a method exists:
```swift
class Worker : NSObject
{
    func work() {Swift.print("work") }
    func eat(food: AnyObject) { }
    func sleep(hours: Int, minutes: Int) { }
}

let worker = Worker()

let canWork = worker.respondsToSelector(Selector("work"))
```