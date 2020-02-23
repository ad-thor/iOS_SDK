# Applins SDK

- [Before You Start](#start)
- [SDK Set Up Use CocoaPods](#step1)
- [Integration SDK](#step2)
  - [GDPR](#gdpr)
  - [COPPA](#coppa)
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
- Target iOS 8.0 or higher

## <a name="step1">Import the Mobile Ads SDK </a>

**Cocoapods (preferred)**

- Add the following line of code to the project of Podfile file : pod 'ApplinsSDK' 
- Running a 'pod install' command

**Manual download**

- You can also [download](https://github.com/ad-thor/iOS_SDK/blob/master/ApplinsSDK.framework.zip) a copy of the SDK framework directly, unzip the file, and import the framework into your project in Xcode.

## <a name="step2">Integration SDK</a>

### <a name="gdpr">GDPR </a> 
* Use this interface to upload consent from affected users.
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
	
	
   [[Applins shareSDK] uploadConsentValue:@"yes" consentType:@"GDPR" complete:^(BOOL state) {
    }];
```
* Warning:
	1.If SDK don't gather the user informatian ,you probably get no fill.
	2.It is recommended that obtaining the user's consent before SDK initialization.

### <a name="coppa">Child-Oriented(COAPPA)</a> 
* In order to comply with the provisions of the Children's Online Privacy Protection Act (COPPA), we provide the setIsChildDirected interface.	Developers can use this interface to indicate that your content is child-oriented. We will stop personalized advertising and put in advertisements suitable for children，which may result in no filling.
```
     //child-oriented
     [[Applins shareSDK] setIsChildDirected:NO];
```     
* Warning
	1.If SDK don't gather the user informatian ,you probably get no fill.
	2.It is recommended that obtaining the user's consent before SDK initialization.	
	
### <a name="init">SDK initializion</a> 
- Add the `NSAllowsArbitraryLoads` to your app's Info.plist file to disable ATS restrictions.

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

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

  // Initialize Applins SDK
  [[Applins shareSDK] initSDK:@"Your Slot ID"];
  return YES;
}

@end
```


### <a name="native">Adding the Native Ad API in iOS</a>
We recommend that you add a AD view which inherits from ALSNativeAd. So the SDK could send impression and track without developer's concern.

```
@interface NativeADView : ALSNativeAd
- (void)setValuesWith:(ALSNativeAdModel *)model;
@end
```
Add an interface to assign AD model to AD view.
```
@implementation NativeADView

- (void)setValuesWith:(ALSNativeAdModel *model){
}
@end
```
Here're the interfaces to get ads.
```
 /**
 We recommend the AD View inherits ALSNative.
 Get the nativeModel assigned to AD view and setting up layout in it.
 
 @param slot_id         Native AD ID
 @param delegate        Set Delegate of Ad event(<ALSNativeAdDelegate>)
 @param WHRate          Set Image Rate
 @param isTest          Use test advertisement or not
 @param success         The request is successful Block, return Native Element Ad
 @param failure         The request failed Block, retuen error
 */
- (void)getNativeAD:(NSString *)slot_id
                      delegate:(id)delegate
           imageRate:(ALSImageWHRate)WHRate
                        isTest:(BOOL)isTest
                       success:(void (^)(ALSNativeAdModel *nativeModel))success
                       failure:(void (^)(NSError *error))failure;


/**
 Preload native ADs with image
 Using inheritance ALSNativeAd advertising View customize layout, in prior to add to the parent View will return to the frame and successful nativeModel assigned to a custom View.
 
 @param slot_id         Native AD ID
 @param delegate        Set Delegate of Ad event(<ALSNativeAdDelegate>)
 @param WHRate          Set Image Rate
 @param preloadImage    preload AD images if afferent YES
 @param isTest          Use test advertisement or not
 @param success         The request is successful Block, return Native Element Ad
 @param failure         The request failed Block, retuen error
 */
- (void)preloadNativeAD:(NSString *)slot_id
               delegate:(id)delegate
              imageRate:(ALSImageWHRate)WHRate
           preloadImage:(BOOL)preloadImage
                 isTest:(BOOL)isTest
                success:(void (^)(ALSNativeAdModel *nativeModel))success
                failure:(void (^)(NSError *error))failure;
/**
 Get Keywords Element Native ADs
 Using inheritance ALSNativeAd advertising View customize layout, in prior to add to the parent View will return to the frame and successful nativeModel assigned to a custom View.
 
 @param slot_id         Native AD ID
 @param delegate        Set Delegate of Ad event(<ALSNativeAdDelegate>)
 @param WHRate          Set Image Rate
 @param cat             ad type
 @param keyWords        Set Ad Keywords
 @param isTest          Use test advertisement or not
 @param success         The request is successful Block, return Native Element Ad
 @param failure         The request failed Block, retuen error
 */
- (void)getNativeADwithCatogaryOrKeywords:(NSString *)slot_id
                      delegate:(id)delegate
           imageRate:(ALSImageWHRate)WHRate
                         adcat:(NSInteger)cat
                      keyWords:(NSArray *)keyWords
                        isTest:(BOOL)isTest
                       success:(void (^)(ALSNativeAdModel *nativeModel))success
                       failure:(void (^)(NSError *error))failure;


/**
 Get Multiterm Element Native ADs
 Using inheritance ALSNativeAd advertising View customize layout, in prior to add to the parent View will return to the frame and successful nativeModel assigned to a custom View.
 
 @param slot_id         Native AD ID
 @param num             Ad numbers
 @param delegate        Set Delegate of Ad event(<ALSNativeAdDelegate>)
 @param WHRate          Set Image Rate
 @param isTest          Use test advertisement or not
 @param success         The request is successful Block, return Native Element Ad
 @param failure         The request failed Block, retuen error
 */
-(void)getMultiNativeADs:(NSString *)slot_id
               adNumbers:(NSInteger)num
                delegate:(id)delegate
                imageRate:(ALSImageWHRate)WHRate
                   isTest:(BOOL)isTest
                  success:(void (^)(NSArray *nativeArr))success
                  failure:(void (^)(NSError *error))failure;

```

### <a name="nativevideo">Adding the Native Video Ad API in iOS</a>

```
/**
 Get Native video Ad
 Call this interface to get Native Video AD.
 
 @param slot_id         Native Video slot ID
 @param delegate        Set Delegate of Ads event (<ALSNativeVideoDelegate>)
 @param WHRate          Set Image Rate
 @param isTest          Use test advertisement or not
 */
- (void)getNativeVideoAD:(NSString*)slot_id
                delegate:(id)delegate
               imageRate:(ALSImageWHRate)WHRate
                  isTest:(BOOL)isTest;
			  
#ALSNativeVideoDelegate Callback Delegate
/**
 * Advertisement load success.
 */
-(void)ALSNativeVideoLoadSuccess:(ALSNativeVideoModel *)nativeVideoModel{
	//save model
	self.nativeVideoModel = nativeVideoModel
	//add mediaview, width:height = 1.77:1
    	ALSMediaView *mediaView = [[ALSMediaView alloc] initWithFrame:CGRectMake(x, y, width, width/1.77)];
    	mediaView.EnableAutoPlay = NO;   //auto control play/pause
    	mediaView.EnableWWANPlay = YES;  //play video via 3g/4g
    	[mediaView setNativeVideoAd:nativeVideoModel];
    	[self.view addSubview:self.mediaView];
    	//add title
    	self.nvTitleLable.text = nativeVideoModel.title;
    	//add desc
    	self.nvDescLable.text = nativeVideoModel.desc;
    	//add logo
    	self.nvLogo.image = self.nativeVideoModel.ADsignImage;
    	//add click
     	UITapGestureRecognizer *jumpGesture = [[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(clickAD)];
     	[self.mediaView addGestureRecognizer:jumpGesture];
}

- (void)clickAD{
   	[self.nativeVideoModel clickToPresentOnParentVC:self animated:YES completion:nil];
}

/**
 * Advertisement load failed.
 */
-(void)ALSNativeVideoLoadFailed:(NSError *)error;
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

#ALSAdViewDelegate interfaces related to interstitial, for more detail please check ALSAdViewDelegate in ALSADMRAIDView.h

//banner ad
- (void)ALSLoadBannerSuccess:(ALSADMRAIDView*)adView{
        [self.view addSubview adView];
}
	
//error while request ads. (share the same error delegate interface with interstitial)
- (void)ALSAdView:(ALSADMRAIDView*)adView loadADFailedWithError:(NSError*)error{

}

//click ad
- (void)ALSAdViewClicked:(ALSADMRAIDView *)adView{

}

//mraid ad show
- (void)ALSAdViewShow:(ALSADMRAIDView*)adView {

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
- (void)preloadInterstitialAd:(NSString *)slotid delegate:(id)delegate isTest:	(BOOL)isTest;

/**
Show interstitial ad
Call this method after preload Interstitial ad success
*/
- (void)showInterstitialAD;

/**
Check interstitial ad to be Ready
Call this method before show ad
*/
- (BOOL)isInterstitialReady;


ALSAdViewDelegate interfaces related to interstitial, for more detail please check ALSAdViewDelegate in ALSADMRAIDView.h
//interstitial is ready, call mraidInterstitialShow to show it.
- (void)ALSLoadInterstitialSuccessWithSlot:(NSString *)slot {
	[[Applins shareSDK] showInterstitialAD];
}

//error while request ads. (share the same error delegate interface with banner)
- (void)ALSAdView:(ALSADMRAIDView*)adView loadADFailedWithError:(NSError*)error{

}

//click ad
- (void)ALSAdViewClicked:(ALSADMRAIDView *)adView{

}

//mraid ad show
- (void)ALSAdViewShow:(ALSADMRAIDView*)adView {

}

```

###  <a name="rewardedvideo">Adding the RewardedVideo Ad API in iOS</a>

```     
/**
Get RewardVideo Ad
First you should call (preloadRewardedVideoAD:delegate:) method get RewardVideo Ad！Then On his return to the success of the proxy method invokes the （showRewardedVideo） method
 	
@param slot_id         Rewarded Video slot ID
@param delegate        Set Delegate of Ads event (<ALSRewardVideoDelegate>)
*/
- (void)preloadRewardedVideoAD:(NSString *)slot_id delegate:(id)delegate;

/**
show RewardVideo      // We recommendation use [ 	showRewardedVideoWithCustomViewController: ] interface !
*/
- (void)showRewardedVideo;

/**
show RewardVideo
@param viewController The view controller on which the interstitial will display
*/
- (void)showRewardedVideoWithCustomViewController:(UIViewController *)viewController;

/**
ALS Reward video is ready to play
@return YES:you can call show rewardvideo interface / NO:don't call show rewardvideo interface 
*/
- (BOOL)isRewardedVideoReady;


#RewardVideoDelegate delegate callback interface
- (void)ALSRewardedVideoLoadSuccess {
	if([[Applins shareSDK] isRewardedVideoReady])
		[[Applins shareSDK] showRewardedVideo];
	NSLog(@"rewarded vidoe load success, call showRewardedVideo");
}                       
- (void)ALSRewardedVideoStart {
	NSLog(@"rewarded video starts to play");
}
- (void)ALSRewardedVideoFinish {
	NSLog(@"rewarded video complete-this is called after a full video view,before end card is shown");
} 
- (void)ALSRewardedVideoClicked {
	NSLog(@"rewarded video clicked");
}
- (void)ALSRewardedVideoWillJumpToAppStore {
	NSLog(@"users click reward video and leave application");
} 
- (void)ALSRewardedVideoJumpFailed {
	NSLog(@"reward video click and failed jumping to App Store");
}
- (void)ALSRewardVideoLoadingFailed:(NSError *)error {
	NSLog(@"rewarded video request failed ");
}
- (void)ALSRewardVideoClosed {
	NSLog(@"rewarded video ad closed-this can be triggered by closing the end card");
}
- (void)ALSRewardedName:(NSString *)rewardName rewardedAmount:(NSString *)rewardedAmount customParams:(NSString*) customParams {
        NSLog(@"give reward to the users interface");
}
    
```


### <a name="Appwall">Adding the Appwall Ad API in iOS</a>

```
/**
Get AppWall ViewController
You should Call preloadAppWall method. Then Call showAppWallViewController method show Appwall.

@param slot_id       Native AD ID
@param customColor   If you want set custom UI,you should create ALSCustomColor object
@param delegate      Set Delegate of Ads event (<ALSAppWallDelegate>)
@param isTest        Use test advertisement or not
@param success       The request is successful Block
@param failure       The request failed Block, retuen error
*/
- (void)preloadAppWall:(NSString *)slot_id
           customColor:(ALSCustomColor *)customColor
              delegate:(id)delegate
                isTest:(BOOL)isTest
               success:(void(^)())success
                failure:(void(^)(NSError *error))failure;
			 
/**
Get App Wall ViewController

@return AppWallViewController
*/
- (UIViewController *)showAppWallViewController;

ALSAppWallDelegate interfaces related to Appwall, for more detail please check ALSAppWallDelegate in ALSADExternalDelegate.h
     
/**
* User click the advertisement.
*/
-(void)ALSAppWallDidClick:(ALSNativeAd *)nativeAd;
/**
* Advertisement landing page will show.
*/
-(void)ALSAppWallDidIntoLandingPage:(ALSNativeAd *)nativeAd;
/**
* User left the advertisement landing page.
*/
-(void)ALSAppWallDidLeaveLandingPage:(ALSNativeAd *)nativeAd;
/**
* Leave App
*/
-(void)ALSAppWallWillLeaveApplication:(ALSNativeAd *)nativeAd;
/**
* User close the advertisement.
*/
-(void)ALSAppWallClosed;
/**
* Jump failure
*/
-(void)ALSAppWallJumpfail:(ALSNativeAd*)nativeAd;

```

###  <a name="Splash">Adding the Splash Ad API in iOS</a>
* You need to add your own launch screen first. Then preload and show Splash AD immediately after opening the app.
```
/**
Preload Splash Ad
Call this interface preload Splash AD.
 
@param slotid          Splash slot ID
@param delegate      Set Delegate of Ads event
@param window          Set UIWindow
@param launchImage          Set LaunchImage
@param customAdView        Set bottom custom view if needed
@param waitAdTime        Set loading time, ad will not show if not ready during waitAdTime
@param isTest          Use test advertisement or not
 */
- (void)preloadaAndShowSplashAd:(NSString *)slotid delegate:(id)delegate window:(UIWindow*)window launchImage:(UIImage*)launchImage customAdView:(UIView*)view waitAdTime:(float)waitTime isTest:(BOOL)isTest;

/**
Delegate
*/
- (void)ALSSplashAdFailed:(NSError*)error;
- (void)ALSSplashAdClicked;
- (void)ALSSplashAdJumpedFailed;
- (void)ALSSplashAdIsShow;
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
