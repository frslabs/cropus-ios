# Cropus-iOS SDK

![version](https://img.shields.io/badge/version-v1.6.0-blue)

The Cropus SDK is used to capture and crop the signature. This SDK is useful to add signature to any digitally created documents.

You can find the release history at [Changelog](CHANGELOG.md)

‼ ATTENTION ‼ → BREAKING CHANGE introduced at Octus SDK `v1.4.9`. We have introduced a new license format. If you are using versions prior to `v1.4.9` and intend to update to `v1.4.9` or above please contact `support@frslabs.com` for an updated license.

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

## Minimum Requirements

- Xcode 13.0
- iOS 13.0+
- Swift 5.0

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
platform :ios, '13.0'
target '<Your Target Name>' do
use_frameworks!
pod 'Cropus', '1.5.3'
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
3. In terminal enter below command to install the pod 

   pod install or pod update or pod install --repo-update.

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
       if imageResolution == "BOTH" || imageResolution == "HIGH"{
           let highResolutionImage = getImageFromDocumentDirectory(resultString: results.getHighResolutionPath!)
       }
       if imageResolution == "BOTH" || imageResolution == "LOW"{
           let lowResolutionImage = getImageFromDocumentDirectory(resultString: results.getLowResolutionPath!)
       }
       scanner.dismiss(animated: true)
    }
    
    func cropusScanner(_ scanner: CropusScannerController, didFailWithError error: String) {
        print(error)
        scanner.dismiss(animated: true)
    }
   func getImageFromDocumentDirectory(resultString : String) -> UIImage {
        var croppedImage = UIImage()
        let fileManager = FileManager.default
        let fileArray = resultString.components(separatedBy: "/")
        let finalFileName = fileArray.last
        let path = (NSSearchPathForDirectoriesInDomains(.documentDirectory, .userDomainMask, true)[0] as NSString).appendingPathComponent(finalFileName!)
        if fileManager.fileExists(atPath: path) {
            croppedImage = UIImage(contentsOfFile: path)!
        } else {
             print("No Image")
        }
       return croppedImage
    }
}
```

2. Invoke Cropus SDK

```swift
    // ...
    
    override func viewDidLoad(_ animated: Bool) {
        let scanner = CropusScannerController(showInstruction: true, delegate:self)
        scanner.modalPresentationStyle = .fullScreen
        scanner.licenceKey = "CROPUS_LICENCE_KEY"
        //scanner.setLowResMaxImageSize = 15 // Set low resolution image max size
        scanner.setOutputImageFormat = "jpg" //Output image format either "jpg" or "png" by default result will be in png format.
        scanner.setOutputImageResolution = "BOTH" // Output image resolution either "BOTH","LOW","HIGH" by default result is in "HIGH" resolution image.
        self.present(scanner, animated: true)
    }
    
    // ...    
```
## Forus Result

```swift

    High Resolution image Output:  getImageFromDocumentDirectory(resultString: results.getHighResolutionPath!) //High resolution Output if selected in input side
    Low Resolution image Output:   getImageFromDocumentDirectory(resultString: results.getLowResolutionPath!) // Low resolution Output if selected in input side
    Both Resolution : getImageFromDocumentDirectory(resultString: results.getHighResolutionPath!) & getImageFromDocumentDirectory(resultString: results.getLowResolutionPath!)
```  

## Cropus Error Codes

Following error codes will be returned on the `onCropusFailure` method of the callback

| CODE | DESCRIPTION                  |
| ---- | ---------------------------- |
| 803  | Camera permission denied    |
| 804  | Cropus interrupted            |
| 805  | Cropus SDK License has expired             |
| 806  | Cropus SDK License is invalid             |
| 807  | Invalid input parameters passed    |
| 809  | Unable to save the cropped image        |
| 810  | Transaction failed/ Ping failed        |

 Sets the Cropus SDK apiCredentials . Obtain the appropriate api credentials through a REST API call , for details about the REST API, contact `support@frslabs.com`
  
 
## Help
For any queries/feedback , contact us at `support@frslabs.com` 
