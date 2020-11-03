# cropus-ios SDK

![version](https://img.shields.io/badge/version-v1.0.0-blue)

The Cropus SDK is used to capture and crops the signature. This SDK is useful to add signature to any digitally created documents.

You can find the release history at [Changelog](CHANGELOG.md)

# Table Of Content

- [Prerequisite](#prerequisite)
- [Requirements](#requirements)
- [Permission](#permission)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Cropus Error Codes](#cropus-error-codes)
- [Help](#help)

## Prerequisite

You will need a valid license and Netrc credentials to use the Cropus SDK, which can be obtained by contacting support@frslabs.com. 

Once you have the license , follow the below instructions for a successful integration of Cropus SDK onto your iOS Application.

## Requirements

- Swift 5.0
- iOS 12.0+

## Permission

In Info.plist file add following code to allow your application to access iPhone's camera:
``<key>NSCameraUsageDescription</key>
<string>Allow access to camera</string>``

## Installation

### Cocoapods


You can use [CocoaPods](http://cocoapods.org/) to install `cropus` by adding it to your `Podfile`:

```ruby
source 'https://gitlab.com/frslabs-public/ios/cropus-ios.git'
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '12.0'
target '<Your Target Name>' do
use_frameworks!
pod 'Cropus', '1.0.0'
end
```

###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install cropus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine cropus-ios.repo.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod install or pod update.

4. Connect with physical device to build and run Cropus, It will not build/run in simulator due to camera dependency.

To get the full benefits import `Cropus` wherever you import UIKit

``` swift
import UIKit
import Cropus
```

## Getting Started

### Swift

1. Initialize the input parameters and import delegate CropusControllerDelegate

```swift
class YourViewController: UIViewController,CropusControllerDelegate {

    func cropusScanner(_ scanner: CropusScannerController, didFinishScanningWithResults results: cropusScannerResults) {
        print(results.croppedImage)
        scanner.dismiss(animated: true)
    }
    
    func cropusScanner(_ scanner: CropusScannerController, didCancel cancel: String) {
        print("cancelled")
        scanner.dismiss(animated: true)
    }
    
    func cropusScanner(_ scanner: CropusScannerController, didFailWithError error: String) {
        print(error)
        scanner.dismiss(animated: true)
    }
  
}
```

2. Invoke Cropus SDK

```swift
    // ...
    
    override func viewDidLoad(_ animated: Bool) {
        let scanner = CropusScannerController(delegate: self)
        scanner.modalPresentationStyle = .fullScreen
        scanner.licenceKey = "CROPUS_LICENCE_KEY"
        self.present(scanner, animated: true)
    }
    
    // ...    
```
## Cropus Error Codes

Following error codes will be returned on the `onCropusFailure` method of the callback

| CODE | DESCRIPTION                  |
| ---- | ---------------------------- |        
| 805  | Cropus SDK License has expire             |
| 806  | Cropus SDK License is invalid             |

 Sets the Cropus SDK apiCredentials . Obtain the appropriate api credentials through a REST API call , for details about the REST API, contact `support@frslabs.com`
  
 
## Help
For any queries/feedback , contact us at `support@frslabs.com` 
