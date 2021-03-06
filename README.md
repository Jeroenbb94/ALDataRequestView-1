# ALDataRequestView

[![Version](https://img.shields.io/cocoapods/v/ALDataRequestView.svg?style=flat)](http://cocoapods.org/pods/ALDataRequestView)
[![License](https://img.shields.io/cocoapods/l/ALDataRequestView.svg?style=flat)](http://cocoapods.org/pods/ALDataRequestView)
[![Platform](https://img.shields.io/cocoapods/p/ALDataRequestView.svg?style=flat)](http://cocoapods.org/pods/ALDataRequestView)

A simple way to show:

* Loading view while a producer or observable is started
* Reload view when a producer or observable is failed
* Empty view when a producer or observable returns an empty array or an object which inherits the `Emptyable` protocol and `isEmpty` is true

## Usage

To run the example project, clone the repo, and run `pod install` from the Example directory first.

### ReactiveCocoa & RxSwift
Simply call 

```swift
attachToDataRequestView(dataRequestView)
```
on your signalProducer or observable. 

Make sure you implement the `ALDataRequestViewDataSource`:

```swift
func loadingViewForDataRequestView(dataRequestView: ALDataRequestView) -> UIView?
func reloadViewControllerForDataRequestView(dataRequestView: ALDataRequestView) -> ALDataReloadType?
func emptyViewForDataRequestView(dataRequestView: ALDataRequestView) -> UIView?
```

#### Examples
##### RxSwift

```swift
let URLRequest = NSURLRequest(URL: NSURL(string: "http://httpbin.org/status/400")!)
NSURLSession.sharedSession().rx_data(URLRequest).attachToDataRequestView(dataRequestView!).subscribe()
```
##### ReactiveCocoa

```swift
let URLRequest = NSURLRequest(URL: NSURL(string: "http://httpbin.org/status/400")!)
NSURLSession.sharedSession()
    .rac_dataWithRequest(URLRequest)
    .flatMap(.Latest, transform: { (data, response) -> SignalProducer<NSData, NSError> in
        if let httpResponse = response as? NSHTTPURLResponse where httpResponse.statusCode > 299 {
            return SignalProducer(error: NSError(domain: "", code: httpResponse.statusCode, userInfo: nil))
        }
        return SignalProducer(value: data)
    })
    .attachToDataRequestView(dataRequestView!)

```


## Installation

ALDataRequestView is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "ALDataRequestView"
```

## Author

Antoine van der Lee, info@avanderlee.com

## License

ALDataRequestView is available under the MIT license. See the LICENSE file for more info.
