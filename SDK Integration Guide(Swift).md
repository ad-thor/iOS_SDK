# Applins SDK

- [Before You Start](#start)
- [SDK Set Up Use CocoaPods](#step1)
- [Integration SDK](#step2)
  - [Data Privacy](#privacy)
   	- [Https](#https)
  	- [GDPR(optional)](#gdpr)
  	- [COPPA(optional)](#coppa)
  - [Initialization](#init)
  - [Native](#native)
  - [Native Video](#nativevideo)
  - [Banner](#banner)
  - [Dynamic Interstitial](#interstitial)
  - [Rewarded Video](#rewardedvideo)
  - [AppWall](#Appwall)
  - [Splash](#Splash)
- [SDK Demo Download](#sdkDemo)
- [Error Code For SDK](#error)
## <a name="start">Prerequisites</a>

- Use Xcode 10.0 or higher
- Target iOS 9.0 or higher

## <a name="step1">Import the Mobile Ads SDK </a>

**Cocoapods (preferred)**

- Add the following line of code to the project of Podfile file : pod 'ApplinsSDK' 
- Running a 'pod install' command

**Manual download**

- You can also [download](https://github.com/ad-thor/iOS_SDK/blob/master/ApplinsSDK.framework.zip) a copy of the SDK framework directly, unzip the file, and import the framework into your project in Xcode.

## <a name="step2">Integration SDK</a>

### <a name="privacy">Data Privacy(Optional) </a> 
#### <a name="https">Https </a> 
* Use https to ensure data security
```
Applins.shareSDK().setSchemaHttps();
```

#### <a name="gdpr">GDPR(Optional) </a> 
* If there are no special privacy requirements, you can skip this chapter. Use this interface to upload consent from affected users.
```
/**
 For GDPR

 @param consentValue  yes/no/other
 @param consentType   content type is the agreement name you signed with users
 @param complete      state
 */
- (void)uploadConsentValue:(NSString *)consentValue
	       consentType:(NSString *)consentType
		  complete:(void(^)(BOOL state))complete;
	
	
    Applins.shareSDK().uploadConsentValue("yes", consentType: "GDPR") { success in
    }
```

* Warning:
	1.If SDK don't gather the user informatian ,you probably get no fill.
	2.It is recommended that obtaining the user's consent before SDK initialization.

#### <a name="coppa">Child-Oriented(COPPA) (Optional)</a> 
* If there are no special privacy requirements, you can skip this chapter.
* In order to comply with the provisions of the Children's Online Privacy Protection Act (COPPA), we provide the setIsChildDirected interface.	Developers can use this interface to indicate that your content is child-oriented. We will stop personalized advertising and put in advertisements suitable for children，which may result in no filling.
```
     //child-oriented
     Applins.shareSDK().setIsChildDirected(false)
```
* Warning
	1.If SDK don't gather the user informatian ,you probably get no fill.
	2.It is recommended that obtaining the user's consent before SDK initialization.	
	
### <a name="init">SDK initializion</a> 
- Add the `NSAllowsArbitraryLoads` to your app's Info.plist file to disable ATS restrictions.Because there are still some third-party advertisements click or impression url using http protocol.
```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

* Under "Build Settings->Other Linker Flags" add: "-ObjC"

* Initialize Applins SDK in your didFinishLaunchingWithOptions method.

```
#import <ApplinsSDK/ApplinsSDK.h>
…
@main
class AppDelegate: UIResponder, UIApplicationDelegate
{
	func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool 	
	{
        	// Initialize Applins SDK
  		Applins.shareSDK().initSDK("Your Slot ID")
        	return true
	}
}
@end
```

### <a name="banner">Adding the Banner Ad API in iOS</a>

Applies SDK supports three ad sizes banner to be used in your APP.

| Ad format             | Size    | Reconmmandation                           |
| --------------------- | ------- | ----------------------------------------- |
| ALSBannerSizeW320H50  | 320x50  | highly reconmmanded for mobile phone APP  |
| ALSBannerSizeW320H100 | 320x100 | recommanded for tablets or larger devices |
| ALSBannerSizeW300H250 | 300x250 | recommanded for tablets or moblie phone   |

```
/**
 Get Banner Ad View
 
 @param slotid          Banner AD ID
 @param delegate        Set Delegate of Ad event(<ALSAdViewDelegate>)
 @param size          requre Ad Size
 @param isTest          Use test advertisement or not
 */

- (void)getBannerAD:(NSString*)slotid delegate:(id)delegate adSize:(ALSBannerSize)size isTest:(BOOL)isTest;

    Applins.shareSDK().getBannerAD("31840716", delegate: self, adSize: ALSBannerSizeW320H50, isTest: false)
    //banner delegate
    func alsLoadBannerSuccess(_ adView: ALSADMRAIDView!) {
        self.view.addSubview(adView)
        adView.frame = CGRectMake(25, 500, 320, 50)
    }
    
    //banner and interstiail delegate
    func alsAdView(_ adView: ALSADMRAIDView!, loadADFailedWithError error: Error!) {
        NSLog("%@%@", "loadADFailedWithError: " , error.localizedDescription)
    }
    
    func alsAdViewShow(_ adView: ALSADMRAIDView!) {
        NSLog("%@%@", "impression ad slotid: " , adView.slot)
    }
    
    func alsAdViewClicked(_ adView: ALSADMRAIDView!) {
        NSLog("%@%@", "click ad slotid: " , adView.slot)
    }

```


### <a name="interstitial">Adding Dynamic Interstitial Ad API in iOS</a>

```
/**
Preload Interstitial Ad
Call this interface preload Interstitial AD.

@param slotid          interstital slot ID
@param delegate        Set Delegate of Ads event (<ALSAdViewDelegate>)
@param isTest          Use test advertisement or not
*/

    Applins.shareSDK().preloadInterstitialAd("YOUR SLOT ID", delegate: self, isTest: false)


    //interstitial delegate
    //interstitial is ready, call mraidInterstitialShow to show.
    func alsLoadInterstitialSuccess(withSlot slot: String!) {
        if Applins.shareSDK().isInterstitialReady(){
            Applins.shareSDK().showInterstitialAD()
        }
    }
    
    //banner and interstiail delegate
    func alsAdView(_ adView: ALSADMRAIDView!, loadADFailedWithError error: Error!) {
        NSLog("%@%@", "loadADFailedWithError: " , error.localizedDescription)
    }
    
    func alsAdViewShow(_ adView: ALSADMRAIDView!) {
        NSLog("%@%@", "impression ad slotid: " , adView.slot)
    }
    
    func alsAdViewClicked(_ adView: ALSADMRAIDView!) {
        NSLog("%@%@", "click ad slotid: " , adView.slot)
    }

```

###  <a name="rewardedvideo">Adding the RewardedVideo Ad API in iOS</a>

```     
/**
Get RewardVideo Ad
First you should call (preloadRewardedVideoAD:delegate:) method get RewardVideo Ad！Then On his return to the success of the delegate method invokes the （showRewardedVideo） method
 	
@param slot_id         Rewarded Video slot ID
@param delegate        Set Delegate of Ads event (<ALSRewardVideoDelegate>)
*/
Applins.shareSDK().preloadRewardedVideoAD("34159155", delegate: self)

    //rewarded video delegate
    //rewarded video is ready to show
    func alsRewardedVideoLoadSuccess(){
        if Applins.shareSDK().isRewardedVideoReady(){
            Applins.shareSDK().showRewardedVideo()
        }
    }
    //rewared video load failed
    func alsRewardVideoLoadingFailed(_ error: Error!) {
        NSLog("%@", error.localizedDescription)
    }
    
    func alsRewardedVideoStart() {
        NSLog("%@", "alsRewardedVideoStart")
    }
    
    func alsRewardedVideoFinish() {
        NSLog("%@", "alsRewardedVideoFinish")
    }
    
    func alsRewardedVideoClicked() {
        NSLog("%@", "alsRewardedVideoClicked")
    }
    
    //reward user in the function
    func alsRewardedName(_ rewardName: String!, rewardedAmount: String!, customParams: String!) {
        NSLog("%@%@%@%@", "RewardedItmeName:", rewardName, " ,rewardedAmount:", rewardedAmount)
    }
```

### <a name="sdkDemo">SDK Demo Download</a>
1. [Download Demo](https://github.com/ad-thor/iOS_SDK/blob/master/ApplinsDemo.zip).
2. Run 'pod update' in terminal.

## <a name="error">Error Code For SDK</a>
Error codes could be obtained from delegate method.

| Error Code     | Description                                   |
| -------------- | --------------------------------------------- |
| 10000 | Initializtion failed                          |
| 10002 | Initializtion data error                      |
| 10003 | Slot ID shouldn't be empty                                   |
| 10004 | Slot ID closed                                   |
| 10005 | The slot IDs should belong to the same APP |
| 20000 | Http status error                                  |
| 20001 | No network                                        |
| 20002 | Fail to get network status                              |
| 20003 | Webview loading html timeout                           |
| 30001 | Response data error                                  |
| 40001 | AD Material download error                              |
| 40004 | AD Material is downloading                            |
| 40005 | AD Material is not found                          |
| 40007 | Get ads failed. eg:no Ads                          |
| 50000 | Initialization interface is not called                         |
| 50001 | Preload interface is not called                          |
| 50005 | interstitial request has been sent or interstitial is ready            |
| 50007 | Interstitial isn't ready                              |
| 60002 | Parsing data error                                  |
| 80000 | Rewarded video cache error                              |
