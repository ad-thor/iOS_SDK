# Mopub Mediation Integration Guide

## <a name="start">Before You Start</a>  

* Support native, banner, interstitial (Fullscreen) and rewarded video.
* Support iOS 8.0+.
* The Mopub account is needed. 
* Make sure you have correctly integrated Mopub Rewarded Video or Interstitial(Fullscreen) or Banner or Native Mediation into your application.
* Please make sure you have signed up on Applins Platform to get your own slot id for test.
* [Download Adapter](https://github.com/ad-thor/iOS_SDK/blob/master/iOS_ApplinsSDK_Adapter%20%20_For_Mopub.zip)

## <a name="Docking">Integration</a>

### Step 1. Download the adpater

1.First, download applinsSDK adapter for Mopub. [Download Adapter](https://github.com/ad-thor/iOS_SDK/blob/master/iOS_ApplinsSDK_Adapter%20%20_For_Mopub.zip)

2.Please put “ApplinsSDK.framework” and the ad formats you’ll use into Mopub's Project.[Download ApplinsSDK](https://github.com/ad-thor/iOS_SDK/blob/master/ApplinsSDK.framework.zip)

### Step 2. Put the ApplinsSDK folder into the project


 <img src="https://github.com/ad-thor/iOS_SDK/blob/master/img/mopub_adapter.png" width = "500" height = "350" alt="图片名称" align=center />

### Step 3.Add a Network on Mopub

![image](https://user-images.githubusercontent.com/13117454/35846618-565c845e-0b52-11e8-8397-639a0c0e4a3b.png)
![image](https://user-images.githubusercontent.com/13117454/35846744-d152ca9c-0b52-11e8-8b88-687e550b2cc1.png)
![image](https://user-images.githubusercontent.com/13117454/35846757-d92dab06-0b52-11e8-8cdf-b8b61533517e.png)

### Step 4.Configuration on Mopub

<img src="https://github.com/ad-thor/iOS_SDK/blob/master/img/mopub_configuration.png" width = "500" height = "350" alt="图片名称" align=center />

```
'slotid':'number'
```

> The Number is the slot ID on Applins Platform

===
### Step 5.Get mopub Ad unit id
![image](https://user-images.githubusercontent.com/13117454/35846889-59f39fde-0b53-11e8-8c13-af823f31350a.png)
![image](https://user-images.githubusercontent.com/13117454/35846900-65d2c8fc-0b53-11e8-9729-fb94b4764b06.png)
![image](https://user-images.githubusercontent.com/13117454/35846910-77dbd8cc-0b53-11e8-97c0-6e90a9bccbd8.png)
### Step 6.Use Ad unit id
![image](https://user-images.githubusercontent.com/13117454/35846975-b426c9cc-0b53-11e8-90f3-d6f0fd06b8b1.png)
===  

## <a name="Docking">Swift</a>
Please import adapters in Swift-OC Bridging Header.
```
#import "ALSBannerCustomEvent.h"
#import "ALSInterstitialCustomEvent.h"
#import "ALSNativeCustomEvent.h"
#import "ALSNativeRenderer.h"
#import "ALSNativeAdAdapter.h"
#import "ALSBaseAdapterConfiguration.h"
```

## <a name="Docking">Native</a>
