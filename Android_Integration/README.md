#AMR Android Integration

* [Requirements](#requirements)  
* [Install](#install)
 * [Gradle](#gradle)
 * [Libs](#libs)
 * [Manifest](#manifest)
* [Usage](#usage)
 * [Initialization](#initialization)
 * [Requesting Native Ads](#requesting-native-ads)
 * [Requesting FullScreen Ads](#requesting-fullScreen-ads)
 * [Requesting Video Ads](#requesting-video-ads)
* [Misc](#misc)
 * [Proguard](#proguard)


#Requirements

* Android 2.3 (Gingerbread - API Version 9) or later.
* Zone Ids provided in Admost Mediation Dashboard.

Add the following permissions to your application AndroidManifest.xml file:

```sh
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

Optionally but necessary to improve user targeting:

```sh
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

#Install

###Gradle

Add the following lines to your module dependencies

```sh
compile fileTree(include: ['*.jar'], dir: 'libs')
compile project(':admost-sdk')
compile 'com.nostra13.universalimageloader:universal-image-loader:1.9.5'
compile 'com.facebook.android:audience-network-sdk:4.11.0'
compile 'com.google.android.gms:play-services-ads:8.4.0'
compile 'com.amazon.android:mobile-ads:5.7.1.1'
compile 'net.pubnative:library:2.0.1'
compile 'com.flurry.android:analytics:6.2.0'
compile 'com.flurry.android:ads:6.2.0'
compile 'com.inmobi.monetization:inmobi-ads:5.3.0'
compile('com.mopub:mopub-sdk:4.6.1@aar') { transitive = true }
compile 'com.supersonic.sdk:mediationsdk:6.4.4@jar'
```

And Repositories as follows;

```sh
repositories {
    flatDir {
        dirs 'libs'
    }
    mavenCentral()
    maven { url "https://jitpack.io" }
    maven { url "https://dl.bintray.com/supersonic/android-sdk" }
}
```

###Libs

Copy the following libraries to the libs folder of your app.

```sh
admost-sdk.aar
MobFox-Android-SDK-Core-1.5.1-RC1.jar
AdFalconAndroidSDK3.1.0.jar
loopme-sdk-4.8.0.jar
Chartboost.jar
Adcolony.jar
NativeXMonetizationSDK_v5.5.6.1.jar
Applovin-sdk-6.1.5.jar

////// Vungle ////////
Vungle-publisher-adaptive-id-3.3.4.jar
Dagger-1.2.2.jar
Javax.inject-1.jar
```

Only admost-sdk.aar is compulsory for mediation system. If you don’t add any one of the network to your app, the SDK will detect that the ad network library isn't there and fail gracefully; the request will go to the next network in the mediation waterfall.

###Manifest

Add the following lines to your application manifest file

```sh
<service android:name="admost.sdk.AdMostImpressionService" />

<!-- ADMOB-->
<activity	android:name="com.google.android.gms.ads.AdActivity" android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize" android:theme="@android:style/Theme.Translucent" />

<!-- INMOBI -->
<activity android:name="com.inmobi.rendering.InMobiAdActivity"  android:configChanges="keyboardHidden|orientation|keyboard|smallestScreenSize|screenSize"  android:theme="@android:style/Theme.Translucent.NoTitleBar" 		  android:hardwareAccelerated="true" />

<!-- AMAZON -->
<activity android:name="com.amazon.device.ads.AdActivity" android:configChanges="keyboardHidden|orientation|screenSize"/>

<!-- FLURRY -->
<activity 	android:name="com.flurry.android.FlurryFullscreenTakeoverActivity"
android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize" />

<!-- ADFALCON-->
<activity android:name="com.noqoush.adfalcon.android.sdk.ADFBrowser" android:configChanges="keyboard|keyboardHidden|orientation|uiMode|screenLayout|screenSize|smallestScreenSize" android:hardwareAccelerated="true">
</activity>

<activity android:name="com.noqoush.adfalcon.android.sdk.ADFActivity" android:configChanges="keyboard|keyboardHidden|orientation|uiMode|screenLayout|screenSize|smallestScreenSize" android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" android:hardwareAccelerated="true">
</activity>

<activity android:name="com.noqoush.adfalcon.android.sdk.ADFCanvas" android:configChanges="keyboard|keyboardHidden|orientation|uiMode|screenLayout|screenSize|smallestScreenSize" android:theme="@android:style/Theme.Translucent.NoTitleBar" android:hardwareAccelerated="true">
</activity>

<!-- LOOPME -->
<activity android:name="com.loopme.AdActivity" android:theme="@android:style/Theme.Translucent"
android:configChanges="orientation|keyboardHidden|screenSize"  android:hardwareAccelerated="true"/>
<activity android:name="com.loopme.AdBrowserActivity" />

<!-- MOBFOX -->
<activity android:name="com.mobfox.sdk.interstitialads.InterstitialActivity"/>

<!-- MOPUB -->
<activity android:name="com.mopub.mobileads.MoPubActivity"
	android:configChanges="keyboardHidden|orientation|screenSize" />
<activity android:name="com.mopub.mobileads.MraidActivity"
	android:configChanges="keyboardHidden|orientation|screenSize" />
<activity android:name="com.mopub.common.MoPubBrowser"
	android:configChanges="keyboardHidden|orientation|screenSize" />
<activity android:name="com.mopub.mobileads.MraidVideoPlayerActivity"
	android:configChanges="keyboardHidden|orientation|screenSize" />

<!-- ADCOLONY -->
<activity android:name="com.jirbo.adcolony.AdColonyOverlay"
	android:configChanges="keyboardHidden|orientation|screenSize"
	android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" />
<activity android:name="com.jirbo.adcolony.AdColonyFullscreen"
	android:configChanges="keyboardHidden|orientation|screenSize"
	android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" />
<activity android:name="com.jirbo.adcolony.AdColonyBrowser"
	android:configChanges="keyboardHidden|orientation|screenSize"
	android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" />

<!-- FACEBOOK -->
<activity android:name="com.facebook.ads.InterstitialAdActivity"
	android:configChanges="keyboardHidden|orientation|screenSize" />

<!-- CHARTBOOST -->
<activity android:name="com.chartboost.sdk.CBImpressionActivity"
	android:configChanges="keyboardHidden|orientation|screenSize"
	android:excludeFromRecents="true"
	android:hardwareAccelerated="true"
	android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" />

<!-- VUNGLE -->
<activity android:name="com.vungle.publisher.FullScreenAdActivity"
	android:configChanges="keyboardHidden|orientation|screenSize"
	android:theme="@android:style/Theme.NoTitleBar.Fullscreen" />

<!-- MEDIABRIX -->
<activity
   android:name="com.mediabrix.android.service.AdViewActivity"
   android:configChanges="orientation|screenSize|keyboard"
   android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" >
</activity>
<service
   android:name="com.mediabrix.android.service.MediaBrixService" >
</service>

<!-- NATIVEX -->
  <activity
      android:name="com.nativex.monetization.activities.InterstitialActivity"
      android:configChanges="orientation|screenSize"
      android:hardwareAccelerated="true"
      android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" />
  <activity
      android:name="com.nativex.videoplayer.VideoActivity"
      android:configChanges="orientation|screenSize" />

<!-- SUPERSONIC -->
  <activity
      android:name="com.supersonicads.sdk.controller.ControllerActivity"
      android:configChanges="orientation|screenSize"
      android:hardwareAccelerated="true" />
  <activity
      android:name="com.supersonicads.sdk.controller.InterstitialActivity"
      android:configChanges="orientation|screenSize"
      android:hardwareAccelerated="true"
      android:theme="@android:style/Theme.Translucent" />
  <activity
      android:name="com.supersonicads.sdk.controller.OpenUrlActivity"
      android:configChanges="orientation|screenSize"
      android:hardwareAccelerated="true"
      android:theme="@android:style/Theme.Translucent" />
```

#Usage

Admost Mediation library is a lean yet complete library that allows you to request and show ads.

>Basic integration steps are:
>1. Initialization
>2. Requesting Native Ads
>3. Requesting Fullscreen Ads

###1) Initialization

Initialize the Admost Mediation SDK in your application’s first Activity. This starts pre-caching and prepares the SDK to display ads.

```sh
AdMostConfiguration.Builder configuration = new AdMostConfiguration.Builder(this);
configuration.initIds(AdMostAdNetwork.NATIVEX, NATIVEX_APP_ID);
configuration.initIds(AdMostAdNetwork.ADCOLONY, ADCOLONY_ID, ADCOLONY_REWARDED, ADCOLONY_INTERSTITIAL);
configuration.initIds(AdMostAdNetwork.VUNGLE, Statics.VUNGLE_ID);
configuration.initIds(AdMostAdNetwork.CHARTBOOST, Statics.CHARTBOOST_ID, Statics.CHARTBOOST_SIGNATURE);
configuration.initIds(AdMostAdNetwork.INMOBI, Statics.INMOBI_ACCOUNT_ID);
configuration.initIds(AdMostAdNetwork.FLURRY, Statics.FLURRY_API_KEY);
AdMost.getInstance().init(configuration.build());
```

It is compulsory to initialize Inmobi, Flurry, Chartboost, Vungle, Nativex  and Adcolony when they are used in the mediation waterfall. Otherwise, ignore these lines.

###2) Requesting Native Ads

You will need to create a AdmostView. Add all the required parameters to it and start it with a listener for extra tracking.

```sh
int TYPE = AdMostManager.getInstance().AD_BANNER; 
// AD_LEADERBOARD for 90dp height, AD_MEDIUM_RECTANGLE for big banners (height : 250dp)
// You can use the following layouts by default, or define other layouts for your own designs
int layout = admost.sdk.R.layout.admost_native;
if (TYPE == AdMostManager.getInstance().AD_BANNER)
layout = admost.sdk.R.layout.admost_native_50;
AdMostLog.setLogEnabled(true);
AdMostViewBinder binder = new AdMostViewBinder.Builder(layout)
        .titleId(admost.sdk.R.id.ad_headline)
        .textId(admost.sdk.R.id.ad_body)
        .callToActionId(admost.sdk.R.id.ad_call_to_action)
        .iconImageId(admost.sdk.R.id.ad_app_icon)
        .mainImageId(admost.sdk.R.id.ad_image)
        .backImageId(admost.sdk.R.id.ad_back)
        .attributionId(admost.sdk.R.id.ad_attribution)
        .isFixed(false)
        .isCloseable(false)//(type & AD_CLOSEABLE) != 0
        .build();

// ZONE_ID : Your ZONE_ID defined on admost mediation panels.

ADMOST_MEDIATION_VIEW = new AdMostView(ACTIVITY, << ZONE_ID>>, TYPE,  new AdMostViewListener() {
    @Override
    public void onLoad(String network, int position) {
        if (network.equals(AdMostAdNetwork.NO_NETWORK)) {
             // No Banner Found
        } else {
            // Ad Loaded, You can get ad view by calling ADMOST_MEDIATION_VIEW.getView(postion)
            // Calling ADMOST_MEDIATION_VIEW.getView method multiple times will not cause any side effect.
        }
    }
}, binder);

// Add the following line to get an ad from the admost mediation system.
int POSITION = 0;
ADMOST_MEDIATION_VIEW.getView(POSITION);
```

You have to call destroy method on ondestroy() method of the activity.

```sh
ADMOST_MEDIATION_VIEW.destroy();
```

###3) Requesting FullScreen Ads

```sh
AdMostInterstitial AD_INTERSTITIAL = new AdMostInterstitial(ACTIVITY, << ZONEID>>, new AdMostAdListener() {
	@Override
	public void onAction(int value) {
		if (value == AdMostAdListener.FAILED) {
			// Failed
		} else {
			// if it is auto loaded then no need to call show. Show will automatically triggered.
			AD_INTERSTITIAL.show();
		}
	}
});

AD_INTERSTITIAL.refreshAd(AUTO_LOAD);
```

AUTO_LOAD (boolean) : Set true, if you want to show the fullscreen ad as soon as it is ready. Otherwise, you need to call show() method manually.

You have to call destroy method on ondestroy() method of the activity.

```sh
AD_INTERSTITIAL.destroy();
```

###4) Requesting Video Ads

```sh
AdMostInterstitial AD_VIDEO = new AdMostInterstitial(ACTIVITY, << ZONEID>>, new AdMostAdListener() {
	@Override
	public void onAction(int value) {
		if (value == AdMostAdListener.LOADED) {
	AdMostLog.log(value + " MainActivity LOADED");
} else if (value == AdMostAdListener.COMPLETED) {
	AdMostLog.log(value + " MainActivity COMPLETED");
} else if (value == AdMostAdListener.FAILED) {
	AdMostLog.log(value + " MainActivity FAILED");
} else if (value == AdMostAdListener.CLOSED) {
	AdMostLog.log(value + " MainActivity CLOSED");
}
	}
});

AD_VIDEO.refreshAd(AUTO_LOAD);
```

AUTO_LOAD (boolean) : Set true, if you want to show the fullscreen ad as soon as it is ready. Otherwise, you need to call show() method manually.

In video ads, AMR sdk has to be aware of the activities lifecycle. So please use the following methods of AMR in your activities which the video ads are called.

```sh
@Override
public void onStart() {
	super.onStart();
	AdMost.getInstance().onStart(this);
}
@Override
public void onResume() {
	super.onResume();
	AdMost.getInstance().onResume(this);
}
@Override
public void onPause() {
	super.onPause();
	AdMost.getInstance().onPause(this);
}
@Override
public void onStop() {
	super.onStop();
	AdMost.getInstance().onStop(this);
}
@Override
public void onDestroy() {
	super.onDestroy();
	AdMost.getInstance().onDestroy(this);
}
```

#Misc

###Proguard

If you are using Proguard, add these lines to your Proguard file

```sh
# ADMOST 
-keep class admost.sdk.** { *; }
-dontwarn admost.sdk.**
# ADMOB
-dontwarn com.google.android.gms.ads.**
-keep public class com.google.android.gms.ads.** {public *;}
-keep public class com.google.ads.** {public *;}
-keep class com.google.android.gms.ads.** {*;}
-keep class com.google.android.gms.common.GooglePlayServicesUtil {*;}
# AMAZON
-dontwarn com.amazon.**
-keep class com.amazon.** {*;}
# CHARTBOOST
-dontwarn org.apache.http.**
-dontwarn com.chartboost.sdk.impl.**
-keep class com.chartboost.sdk.** { *; }
-keepattributes *Annotation*
# INMOBI
-keep class com.inmobi.** {*;}
-dontwarn com.inmobi.**
# FLURRY
-keep class com.flurry.** { *; }
-dontwarn com.flurry.**
# VUNGLE
-keep class com.vungle.** { public *; }
-keep class javax.inject.*
-keepattributes *Annotation*
-keepattributes Signature
-keep class dagger.*
# MOPUB 
-keepclassmembers class com.mopub.** { public *; }
-keep public class com.mopub.**
-keep public class android.webkit.JavascriptInterface {}
-keep class * extends com.mopub.mobileads.CustomEventBanner {}
-keep class * extends com.mopub.mobileads.CustomEventInterstitial {}
-keep class * extends com.mopub.nativeads.CustomEventNative {}
# FACEBOOK 
-dontwarn com.facebook.ads.**
-dontnote com.facebook.ads.**
-keep class com.facebook.** { *; }
-keepattributes Signature
# MOBFOX
-dontwarn com.mobfox.**
-keep class com.mobfox.** {*;}
# LOOPME 
-dontwarn com.loopme.**
-keep class com.loopme.** {*;}
# ADFALCON 
-dontwarn com.noqoush.**
-keep class com.noqoush.** {*;}
# SUPERSONIC
-dontwarn com.supersonic.**
-keep class com.supersonic.** {*;}
```