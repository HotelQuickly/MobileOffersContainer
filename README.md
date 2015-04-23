# Mobile Offers Hybrid-Mobile Web Application's Container for Emulation or Mobile Devices

### Installation instructions

Apache Cordova:

* [Cordova](http://cordova.apache.org/)

### Explanation of repo files/folders

MobileOffersContainer is like any ionic-cordova hybrid app, except for two caveats:

1) It has no actual meaningful web assets in its www folder so that the app its cordova build-process creates is "web-less".
2) Its functional web assets are fetched remotely via setting the src attribute of the content tag in MobileOffersContainer/config.xml to a remote instance from which the actual web application portion of the MobileOffers hybrid app is retrieved.

Currently, that tag in above-mentioned xml reads:

```
<content src="https://mobile-offers-staging.hotelquickly.com/?partner=buzzebees" />
```

Here is a useful shell script for what is possible (and necessary) from within MobileOffersContainer for cordova-ionic build processes, for creating an iOS or Android app based on this configuration:

```
#!/bin/zsh
ionicMake() {
  cordova platform rm ios ; cordova platform rm android ; cordova platform add ios ; cordova platform add android ; ionic build ios ; ionic build android
}

ionicMakeIos() {
  cordova platform rm ios ; cordova platform add ios ; ionic build ios
}

ionicMakeDroid() {
  cordova platform rm android ; cordova platform add android ; ionic build android
}

ionicEmuIos() {
  ionicMakeIos; ionic emulate ios
}

ionicEmuDroid() {
  ionicMakeDroid; ionic emulate android
}
```

This gives anyone with a current local working copy of MobileOffersContainer to:

-build both the iOS and Android apps from a clean slate
-build only the iOS app from a clean slate
-build only the Droid app from a clean slate

-and finally to-

-emulate the iOS app in xcode's iOS Simulator via ionic
-emulate the Droid app in Android Studio's Simulator via ionic

xcode and Android Studio must of course both be installed and running properly in one's local environment for any of the local emulation processes to work, and emulation is known to be extremely unreliable for testing any mobile app, let alone a hybrid app, as was discovered through many rounds of QA prior, so it is advised to simply run the resultant apps in devices.

One can do that in xcode by opening the following file within MobileOffersContainer and running it as one would any iOS project in xcode:

MobileOffersContainer/platforms/ios/MobileOffers.xcodeproj

Likewise, one can do that in android studio (or better yet, genymotion) by opening the following file within MobileOffersContainer and running it as one would any droid apk (or simply dragging it onto a device):

MobileOffersContainer/platforms/android/ant-build/CordovaApp-debug.apk

### Ordinary Cordova build process from config.xml

Ordinarily, cordova uses a config.xml in a web project to build from. However, discrepencies were noticed between the iOS and Android build processes as relate to external containers created by the iOS and Android teams, thus it cannot unfortunately/entirely be assumed to be a strong subtyping (see: Liskov substitution principle) between a cordova-based iOS xcodeproj or Android apk in MobileOffersContainer and any external iOS or Android "containers" that were to be/would be created as external to MobileOffersContainer.

For this reason, it is strongly advised that partners take from the platforms dir within this repo in order to integrate MobileOffers into their applications as needed vs. using MobileOffersContainer as a "base of operations" from which to externalize publicly partner-accessible container code, since cordova building an iOS app or a Droid app and Android and iOS developers using their normalised processes of development to build apps are two different things (with different IDEs, different libraries of objects, and different build paths).

In any case, there is now a swappable config.xml in MobileOffersContainer. The currently active config.xml at the time of this README's writing is for the purposes of building droid containers, but/and there are two files that reference these two different apps' worth of build processes here:

```
MobileOffersContainer/config-droid.xml
MobileOffersContainer/config.ios.xml
```

A future improvement would be to merge these two configs into a single config using the ios-package and android-package attribute values per cordova documentation, but as it stands currently, simply do:

```
cp config-droid.xml config.xml
```

-or-

```
cp config-ios.xml config.xml
```

...prior to running any of the build processes (i.e. in the above-provided shell script for your local environment).
