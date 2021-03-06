
# AdMob Plugin Pro #

Present AdMob Ads in Mobile App/Games natively from JavaScript. 

Highlights:
- [x] Support Banner Ad and Interstitial Ad.
- [x] Multiple banner size, also support custom size.
- [x] Fixed and overlapped mode.
- [x] Most flexible, put banner at any position with overlap mode.
- [x] Auto fit on orientation change.
- [x] Latest iOS SDK v6.11.1, 
- [x] Latest Android Google play services r19.
- [x] Compatible with Intel XDK and Crosswalk.
- [x] Easy-to-use APIs. Can display Ad with single line of Js code.
- [x] Actively maintained, prompt support.

Tips: (According to history stat data in past 2 months)
- [x] Using AdMob Plugin Pro, will get higher fill rate.
- [x] Using Interstitial, will earn better profit, RPM 10 times higher than Banner. 

Glossary:
- Fill rate: The number of times ads are shown on your app, divided by the number of times your app requests to show an ad.
- RPM: Revenue per 1000 impressions (RPM). RPM represents the estimated earnings you'd accrue for every 1000 impressions you receive.
- Intel XDK: Intel's HTML5 App IDE and service, to build mobile app in clould, allow using 3rd-party plugin from web.
- Adobe PhoneGap Build: Adobe service, to publish plugin and build app in cloud, only allow using plugins reviewed and approved.

News:
- Recommended by Telerik in Verified Plugins Marketplace. [read more ...](http://plugins.telerik.com/plugin/admob)
- Recommended by William SerGio in code project (20 Jun 2014), [read more ...](http://www.codeproject.com/Articles/788304/AdMob-Plugin-for-Latest-Version-of-PhoneGap-Cordov)
- Recommended by Arne in Scirra Game Dev Forum (07 Aug, 2014), [read more ...](https://www.scirra.com/forum/plugin-admob-ads-for-crosswalk_t111940)
- Recommended by Intel XDK team (08/22/2014), [read more ...](https://software.intel.com/en-us/html5/articles/adding-google-play-services-to-your-cordova-application)

## How to Use ##

Add the plugin to your cordova project with [Cordova CLI](https://cordova.apache.org/docs/en/edge/guide_cli_index.md.html#The%20Command-Line%20Interface):
```c
cordova create <project_folder> com.<company_name>.<app_name> <AppName>
cd <project_folder>
cordova platform add android
cordova platform add ios

cordova plugin add com.google.cordova.admob

// copy the demo html file to www
rm -r www/*; cp plugins/com.google.cordova.admob/test/index.html www/

// connect device or run in emulator
cordova prepare; cordova run android; cordova run ios;

// or import into Xcode / eclipse
```

If use with Intel XDK:
Project -> CORDOVA 3.X HYBRID MOBILE APP SETTINGS -> PLUGINS AND PERMISSIONS -> Third-Party Plugins ->
Add a Third-Party Plugin -> Get Plugin from the Web, input:
```
Name: AdMobPluginPro
Plugin ID: com.google.cordova.admob
[x] Plugin is located in the Apache Cordova Plugins Registry
```


## Quick Start Example Code ##

>Step 1: Prepare your AdMob Ad Unit Id for your banner and interstitial

```javascript
var ad_units = {
	ios : {
		banner: 'ca-app-pub-6869992474017983/4806197152',
		interstitial: 'ca-app-pub-6869992474017983/7563979554'
	},
	android : {
		banner: 'ca-app-pub-6869992474017983/9375997553',
		interstitial: 'ca-app-pub-6869992474017983/1657046752'
	}
};
// select the right Ad Id according to platform
var admobid = ( /(android)/i.test(navigator.userAgent) ) ? ad_units.android : ad_units.ios;
```

> Step 2: Create a banner with single line of javascript

```javascript
// it will display smart banner at top center, using the default options
if(AdMob) AdMob.createBanner( admobid.banner );

// or, show a banner at bottom
if(AdMob) AdMob.createBanner( {
	adId:admobid.banner, 
	position:AdMob.AD_POSITION.BOTTOM_CENTER, 
	autoShow:true} );

// or, show a rect ad at bottom in overlap mode
if(AdMob) AdMob.createBanner( {
	adId:admobid.banner, 
	adSize:'MEDIUM_RECTANGLE', 
	overlap:true, 
	position:AdMob.AD_POSITION.BOTTOM_CENTER, 
	autoShow:true} );

// or, show any size at any position
if(AdMob) AdMob.createBanner( {
	adId:admobid.banner, 
	adSize:'CUSTOM',  width:200, height:200, 
	overlap:true, 
	position:AdMob.AD_POSITION.POS_XY, x:100, y:200, 
	autoShow:true} );

```

> Step 3: Prepare an interstitial, and show it when needed

```javascript
// preppare and load ad resource in background, e.g. at begining of game level
if(AdMob) AdMob.prepareInterstitial( {adId:admobid.interstitial, autoShow:false} );

// show the interstitial later, e.g. at end of game level
if(AdMob) AdMob.showInterstitial();
```

## Full Example Code ##
Check the [test/index.html] (https://github.com/floatinghotpot/cordova-admob-pro/blob/master/test/index.html).

## API Overview ##

### Methods ###
```javascript```
AdMob.setOptions(options);

AdMob.createBanner(adId/options, success, fail);
AdMob.removeBanner();
AdMob.showBanner(position);
AdMob.showBannerAtXY(x, y);
AdMob.hideBanner();

AdMob.prepareInterstitial(adId/options, success, fail);
AdMob.showInterstitial();
```

### Events ###
> **Syntax**: document.addEventListener(event_name, callback);

```javascript
// for both banner and interstitial
'onAdFailLoad'
'onAdLoaded'
'onAdPresent'
'onAdLeaveapp'
'onAdDismiss'

// deprecated, for banner Ad
'onBannerFailedToReceive'
'onBannerReceive'
'onBannerPresent'
'onBannerLeaveApp'
'onBannerDismiss'
    
// deprecated, for interstitial Ad    
'onInterstitialFailedToReceive'
'onInterstitialReceive'
'onInterstitialPresent'
'onInterstitialLeaveApp'
'onInterstitialDismiss'
```

## Methods ##

### AdMob.setOptions(options) ###

> **Purpose**: Set default values for other methods. All the option items are optional, will use default value if missing.

**Params**:
- **options**, *json object*, mapping key to value.

key/value for param **options**:
- **license**, *string*, set the license key, to remove the 2% Ad traffic sharing
- **bannerId**, *string*, set the default Ad unit Id for banner, like 'ca-app-pub-xxx/xxx'
- **interstitialId**, *string*, set the defualt Ad unit Id for interstitial, like 'ca-app-pub-xxx/xxx'
- **adSize**, *string*, banner Ad size, Default:'SMART_BANNER'. it can be: (see the screenshots for effects)
```javascript
'SMART_BANNER', // recommended. auto fit the screen width, auto decide the banner height
'BANNER', 
'MEDIUM_RECTANGLE', 
'FULL_BANNER', 
'LEADERBOARD', 
'SKYSCRAPER', 
'CUSTOM', // custom banner size with given width and height, see param 'width' and 'height'
```
- **width**, *integer*, banner width, valid when set *adSize*:'CUSTOM'. Default: 0
- **height**, *integer*, banner height, valid when set *adSize*:'CUSTOM'. Default: 0
- **overlap**, *boolean@, allow banner overlap webview, or else will push webview up or down to avoid overlap. Default:false
- **position**, *integer*, position of banner Ad, Default:TOP_CENTER. Value can be one of: 
```javascript
AdMob.AD_POSITION.NO_CHANGE  	= 0,
AdMob.AD_POSITION.TOP_LEFT 		= 1,
AdMob.AD_POSITION.TOP_CENTER 	= 2,
AdMob.AD_POSITION.TOP_RIGHT 	= 3,
AdMob.AD_POSITION.LEFT 			= 4,
AdMob.AD_POSITION.CENTER 		= 5,
AdMob.AD_POSITION.RIGHT 		= 6,
AdMob.AD_POSITION.BOTTOM_LEFT 	= 7,
AdMob.AD_POSITION.BOTTOM_CENTER	= 8,
AdMob.AD_POSITION.BOTTOM_RIGHT 	= 9,
AdMob.AD_POSITION.POS_XY 		= 10, // use the given X and Y, see params 'x' and 'y'
```
- **x**, *integer*, x in pixels. Valid when *overlap*:true and *position*:AdMob.AD_POSITION.POS_XY. Default: 0
- **y**, *integer*, y in pixels. Valid when *overlap*:true and *position*:AdMob.AD_POSITION.POS_XY. Default: 0
- **isTesting**, *boolean*, set to true, to receiving test ad for testing purpose
- **autoShow**, *boolean*, auto show interstitial ad when loaded, set to false if hope to control the show timing with prepareInterstitial/showInterstitial
- **orientationRenew**, *boolean*, re-create the banner on web view orientation change (not screen orientation), or else just move the banner. Default:true.
- **offsetTopBar**, *boolean*, offset position of banner and webview to avoid overlap by status bar (iOS7+)
- **bgColor**, *string*, background color of parent view, value may be color name like 'black', 'white', etc, or '#RRGGBB'
- **adExtras**, *json object*, set extra color style for Ad.
```javascript
{
	color_bg: 'AAAAFF',
	color_bg_top: 'FFFFFF',
	color_border: 'FFFFFF',
	color_link: '000080',
	color_text: '808080',
	color_url: '008000'
}
```

Example Code:
```javascript
var defaultOptions = {
    license: 'username@gmail.com/xxxxxxxxxxxxxxx',
	bannerId: admobid.banner,
	interstitialId: admobid.interstitial,
	adSize: 'SMART_BANNER',
	width: 360, // valid when set adSize 'CUSTOM'
	height: 90, // valid when set adSize 'CUSTOM'
	position: AdMob.AD_POSITION.BOTTOM_CENTER,
	x: 0,		// valid when set position to POS_XY
	y: 0,		// valid when set position to POS_XY
	isTesting: true,
	autoShow: true
};
AdMob.setOptions( defaultOptions );
```

### AdMob.createBanner(adId/options, success, fail) ###

> **Purpose**: create a banner Ad.

**Param**
- **adId**, *string*, the Ad unit Id for banner.
- **options**, *json object*, see the keys in **AdMob.setOptions**
- **success**, *function*, callback when success, can be null or missing.
- **fail**, *function*, callback when fail, can be null or missing.

Extra key/value for param **options**
- **adId**, *string*, Ad unit Id for this banner.
- **success**, *function*, callback when success.
- **error**, *function*, call back when fail.

Example Code:
```javascript
AdMob.createBanner( admobid.banner );

AdMob.createBanner({
	adId: admobid.banner,
	position: AdMob.AD_POSITION.BOTTOM_CENTER,
	autoShow: true,
	success: function(){
	},
	error: function(){
		alert('failed to create banner');
	}
});
```
## AdMob.showBanner(position) ##

> **Purpose**: show banner at given position. It can also be used to move banner to given position.  It's not needed to removeBannr and create a new one.

Params:
- **position**, *integer*, see description in **AdMob.setOptions()**

## AdMob.showBannerAtXY(x, y) ##

> **Purpose**: show banner at given position with (x,y). 

Params:
- **x**, *integer*, in pixels. Offset from screen left.
- **y**, *integer*, in pixels. Offset from screen top.

### AdMob.hideBanner() ###

> **Purpose**: hide the banner, remove it from screen, but can show it later. 

### AdMob.removeBanner() ###

> **Purpose**: destroy the banner, remove it from screen. 

You can create another banner if different size, need remove the old one.

## AdMob.prepareInterstitial(adId/options, success, fail) ##

> **Purpose**: prepare an interstitial Ad for showing.

Params:
- **adId**, *string*, Ad unit Id for the full screen Ad.
- **options**, *string*, see **AdMob.setOptions()*
- **success**, *function*, callback when success, can be null or missing.
- **fail**, *function*, callback when fail, can be null or missing.

Extra key/value for param **options**
- **adId**, *string*, Ad unit Id for this interstitial.
- **success**, *function*, callback when success.
- **error**, *function*, call back when fail.

> Note: it will take some time to get Ad resource before it can be showed. You may buffer the Ad by calling **requestInterstitial**, and show it later.

## AdMob.showInterstitial() ##

> **Purpose**: show interstitial Ad when it's ready.

## AdMob.isInterstitialReady(callback) ##

> **Purpose**: check to confirm interstitial Ad is ready to show.

Example Code:
```javascript
// prepare and aut show
AdMob.prepareInterstitial({
	adId: admobid.interstitial,
	autoShow: true
});

// prepare at beginning of a game level
AdMob.prepareInterstitial({
	adId: admobid.interstitial,
	autoShow: false
});
// check and show it at end of a game level
AdMob.isInterstitialReady(function(isready){
	if(isready) AdMob.showInterstitial();
});
```

## Events ##

All following events will come with a data param, with properties:
* data.adNetwork, the Ad network name, like 'AdMob', 'Flurry', 'iAd', etc.
* data.adType, 'banner' or 'interstitial'
* data.adEvent, the event name

'onAdFailLoad'
> Triggered when failed to receive Ad. 
```javascript
document.addEventListener('onAdFailLoad',function(data){
	console.log( data.error + ',' + data.reason );
	if(data.adType == 'banner') AdMob.hideBanner();
	else if(data.adType == 'interstitial') interstitialIsReady = false;
});
```

'onAdLoaded'
> Triggered when Ad received.
```javascript
document.addEventListener('onAdLoaded',function(data){
	AdMob.showBanner();
});
AdMob.createBanner({
	adId: admobid.banner,
	autoShow: false
});
```

'onAdPresent'
> Triggered when Ad will be showed on screen.

'onAdLeaveApp'
> Triggered when user click the Ad, and will jump out of your App.

'onAdDismiss'
> Triggered when dismiss the Ad and back to your App.

## Notice for Android Proguard ##

To prevent ProGuard from stripping away required classes, add the following lines in the <project_directory>/platform/android/proguard-project.txt file:

```
-keep class * extends java.util.ListResourceBundle {
    protected Object[][] getContents();
}

-keep public class com.google.android.gms.common.internal.safeparcel.SafeParcelable {
    public static final *** NULL;
}

-keepnames @com.google.android.gms.common.annotation.KeepName class *
-keepclassmembernames class * {
    @com.google.android.gms.common.annotation.KeepName *;
}

-keepnames class * implements android.os.Parcelable {
    public static final ** CREATOR;
}

-keep public class com.google.cordova.admob.**

```
