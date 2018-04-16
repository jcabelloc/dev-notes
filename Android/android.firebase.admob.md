# Firebase and Admob for Android Notes

### Set up
* Create a Firebase Project
    * Go to "Add Firebase to your Android app"
    * Register the package name in the Firebase Project
    * Download the google-services.json file
    * Copy that file to the project/app directory
    * Modify the gradle files according to google instructions(Version can be different)

* Sign up for an AdMob account and register an app.
    * Set up an unpublished app

* Link the app to a Firebase project (In Admob)

* Import the Mobile Ads SDK in the gradle file
```
implementation 'com.google.firebase:firebase-ads:15.0.0'
```

* Initialize the SDK
```java
    //...
    MobileAds.initialize(this, "YOUR_ADMOB_APP_ID");
    //...
```

* Create an Ad Unit (In AdMob)
    * For instance banners

* Implementing Banners
    * Add AdView to the layout (See google docs)
    * Always test with test ads (use test ad unit id)
    * Load an ad (*)

* Load an ad
```java
    //...
    AdView mAdView;
    mAdView = findViewById(R.id.adView);
    AdRequest adRequest = new AdRequest.Builder().build();
    mAdView.loadAd(adRequest);
    //...
```



