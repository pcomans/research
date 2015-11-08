Undefined num of params in a method:
```swift
func average(numbers: Double...) -> Double {// calculate average
    let total = numbers.reduce(0.0, combine: {$0 + $1})
    return total / Double(numbers.count)
}
```