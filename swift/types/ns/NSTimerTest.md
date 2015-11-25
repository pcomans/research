import Foundation
//untested but looks good
class Testing {
    
    var timer:NSTimer?
    func test(){
        timer = NSTimer.scheduledTimerWithTimeInterval(0.01, target: self, selector: "tester:", userInfo: somethingToPass, repeats: false)
    }
    
    func tester(timer: NSTimer){
        let theStringToPrint = timer.userInfo as! String
        print(theStringToPrint)
    }
}



another one:
NSTimer.scheduledTimerWithTimeInterval(delay, target: self, selector: "onFlip", userInfo: nil, repeats: false)


perform_selector in swift: (this can be used to call methods/functions by a string)
https://github.com/tokorom/performSelector-swift

