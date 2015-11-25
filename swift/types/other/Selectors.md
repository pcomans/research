Selectors are usualy used when using NSTimer etc

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