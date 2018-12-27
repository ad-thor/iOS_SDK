# Applins SDK

- [Before You Start](#start)
- [SDK Set Up Use CocoaPods](#step1)
- [SDK Set Up Manually](#step2)
  - [Native](#native)
  - [Native Video](#nativevideo)
  - [Banner](#banner)
  - [Dynamic Interstitial](#interstitial)
  - [Rewarded Video](#rewardedvideo)
  - [AppWall](#Appwall)
- [SDK Demo Download](#sdkDemo)
## <a name="start">Prerequisites</a>

- Use Xcode 9.0 or higher
- Target iOS 8.0 or higher

## <a name="step1">Import the Mobile Ads SDK </a>

**Cocoapods (preferred)**

- Add the following line of code to the project of Podfile file : pod 'ApplinsSDK' 
- Running a 'pod install' command

**Manual download **

- You can also [download](https://github.com/zero-sdk/iOS_SDK/blob/master/ApplinsSDK.framework.zip) a copy of the SDK framework directly, unzip the file, and import the framework into your project in Xcode.

## <a name="step1">Initialize mobile ads </a>

- Add the `NSAllowsArbitraryLoads`, `NSAllowsArbitraryLoadsForMedia`, and `NSAllowsArbitraryLoadsInWebContent` exceptions to your app's Info.plist file to disable ATS restrictions.

```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    <key>NSAllowsArbitraryLoadsForMedia</key>
    <true/>
    <key>NSAllowsArbitraryLoadsInWebContent</key>
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
- 
  Use this interface to upload consent from affected users.


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
```

### <a name="native">Adding the Native Ad API in iOS</a>

```
 /**
 We recommend use ALSNative Interface！！！
 Using inheritance ALSNativeAd advertising View customize layout, in prior to add to the parent View will return to the frame and successful nativeModel assigned to a custom View.
 
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
    	mediaView.EnableAutoPlay = NO;   //play video via 3g/4g
    	mediaView.EnableWWANPlay = YES;  //auto control play/pause
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

###  <a name="rewardedvideo">Adding the RewardVideo Ad API in iOS</a>

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

### <a name="sdkDemo">SDK Demo Download</a>
1. [Download Demo](https://github.com/zero-sdk/iOS_SDK/blob/master/ApplinsDemo.zip).
2. Run 'pod update' in Finder.

