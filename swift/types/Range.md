import Foundation
//NSRange does not support unicode like emojis etc
//Swift has ranges
func test(){
    let someText = "hello world"
    let range = someText.startIndex.advancedBy(3)..<someText.endIndex.advancedBy(-3)//create a range
    let result = someText.substringWithRange(range)
    print(result)//"lo wo"
}


```swift
let nsRange = NSMakeRange(4, 12)
nsRange.length//12
nsRange.location//4
let range:Range<Int> = nsRange.toRange()!/*convert NSRange to Range<Int>*/

range.first//4 /*the first index*/
range.last//15 /*not sure what this is*/
range.startIndex//4  /*the first index*/
range.endIndex//16 /*the last index */

range.underestimateCount()//12 /*the length of the range*/
range.count//12/*the length of the range*/
range.contains(15)//true /*assert if a value is within the range: true*/

let newRange = Range<Int>(start:4,end:12)
newRange

let anotherRange = (4..<12)


func equals<T:Comparable>(a:Range<T>,_ b:Range<T>)->Bool {
    return a.startIndex == b.startIndex && a.endIndex == b.endIndex;
}

equals(newRange,anotherRange)
```