# Admob Mediation Integration Guide




## <a name="start">Before You Start</a>


* Please make sure you have correctly integrated Admob Rewarded Video or Interstitial or Banner Mediation into your application. 
* Support banner, interstitial and rewarded video;
* Support both of 32 bit and 64 bit operating systems;
* Support iOS 8.0+;
* Please make sure you have signed up on Applins Platform to get your own slot id for test. If you haven't signed up, please contact us.
* Please make sure you have added an app and at least one ad slot in Applins Platform
* [click here to download the mediation adapter](https://github.com/ad-thor/iOS_SDK/raw/master/iOS_ApplinsSDK_Adapter%20_For_Admob.zip)




## <a name="step 2">Set Up Applins as your ad sources in Admob</a>

#### 1. Create apps and ad unit id in Admob console, as below

> ADD APP

![image](https://github.com/ad-thor/iOS_SDK/blob/master/img/admob_new_app.jpeg)

> ADD AD UNIT 

![image](https://user-images.githubusercontent.com/7203578/32546656-73167126-c445-11e7-818e-a20e7ea49670.png)

-------



#### 2. Create Mediation Group, as below

![image](https://github.com/ad-thor/iOS_SDK/blob/master/img/admob_mediation_group.jpeg)

------



#### 3. Create the Ad Unit in your Admob dashboard that corresponds to your Applins slot ID 

![image](https://github.com/ad-thor/iOS_SDK/blob/master/img/admob_ad_units.png)

-------



####  4. Add Applins custom event in Admob console, as below

![image](https://github.com/ad-thor/iOS_SDK/blob/master/img/admob_custom_event.jpeg)

![image](https://github.com/ad-thor/iOS_SDK/blob/master/img/admob_custom_event2.jpeg)

```
Class Name: GADMBannerApplins (banner)
Parameter: 260 (Applins slot id) 
```

-------



#### 5. Copy adapter files into your code folder

Add files to your project !!!
You should Call Applins init Interface in ApplicationdidFinishLaunchingWithOptions

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      [[Applins shareSDK] initSDK:slotID];
      return YES;
}
```


#### 6. Swift
Please Import adapter files in your OC-Swift Bridging Header.
```
#import "GADMBannerApplins.h"
#import "GADMInterstitialApplins.h"
#import "GADMRewardedVideoAdapterApplins.h"
```

Done!
You are now all set to deliver Applins Ads within your application!
