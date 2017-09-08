[![N|Solid](http://i.imgur.com/iz9YsTS.png)](https://diagnal.com)
# Engage Firebase Extension #
Engage Firebase Extension wraps Firebase Messaging and analytics to provide methods for logging events and setting user properties.

## Getting Started
### Step 1: Install the Engage Core Library
Engage Firebase is an extension of Engage Android SDK and has dependency on `engage-core` library.
Follow the instructions from https://github.com/diagnal/engage-android-sdk/ to install and initialize the `engage-core` library

### Step 2:  Install the Engage Firebase Library
 The Engage Firebase extension is distributed as `aar` package. You can obtain the latest aar file from https://github.com/diagnal/engage-android-sdk/tree/master/aar/extensions/engage-firebase

Include aar file as a dependency in you project. The steps for adding an aar dependency can be found at https://developer.android.com/studio/projects/android-library.html#AddDependency

## Usage

### Analytics

Engage Firebase Analytics  wraps Google's Firebase Analytics and methods for logging events and setting user properties. The events and properties logged with Analytics will be also forwarded to to Engage SDK.

To start with Engage Firebase Analytics, get an instance of the `Analytics` class.
 
```javascript
Analytics analytics = Analytics.getInstance(appContext);
```
`Analytics` class has all the methods from `com.google.firebase.analytics.FirebaseAnalytics` along with underlying Engage functionalities.

Example for logging events:

```javascript
Bundle properties = new Bundle();
properties.putString("property1", "val1");
properties.putString("property2", "val2");

analytics.logEvent("eventName", properties);
```