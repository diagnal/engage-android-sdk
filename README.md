
[![N|Solid](http://i.imgur.com/iz9YsTS.png)](https://diagnal.com)
# Engage Android SDK #
Engage is real-time targeted marketing campaigns that drive user acquisition and conversion.
It promotes content to users and access customer sentiment.
Engage Android SDK allows easy integration of Engage to your android application in a few steps.

Engage Android SDK [class reference documents](https://diagnal-engage.s3.amazonaws.com/engage-android-sdk-docs/engage-core/index.html) and  [release notes.](https://diagnal-engage.s3.amazonaws.com/engage-android-sdk-docs/release-notes.txt)
Engage Android SDK requires API 17 (Android 4.2) or higher. 

## Getting Started
### Step 1: Install the Library
##### Method 1
Engage Android SDK library is in the Maven central repository. The easiest way to integrate Engage Android SDK library is by simply declaring it as dependency in your `build.gradle` file.

```java
dependencies {
    compile 'com.diagnal.engage:engage:1.3.6'
}
```

##### Method 2
The Engage Android SDK is also distributed as `aar` package. You can obtain the latest Engage Android SDK aar file from https://github.com/diagnal/engage-android-sdk/

Include aar file as a dependency in you project. 
The steps for adding an aar dependency can be found at https://developer.android.com/studio/projects/android-library.html#AddDependency
### Step 2:  Initialize the Client
The entry point of the SDK is through the `Engage` class. We recommend initializing the client in `onCreate()` your Application subclass.
```java
Engage.initialize(applicationContext, ENGAGE_ACCOUNT_ID, ENGAGE_PROJECT_ID);
```
`ENGAGE_ACCOUNT_ID` and `ENGAGE_PROJECT_ID` can be obtained from your [Enage Dashboard](https://engage.diagnal.com/settings )
## Usage
### Identify User

The `identify` method is how you associate your users and their actions to a recognizable `userId` and `Traits`.

```java
 Engage.identify(new Traits(userId)
                .setName(name)
                .setAge(age)
                .setEmail(email)
                .setGender(gender)
                .setRegistrationStatus(registrationStatus)
        );
```
If `Traits` is initialized with the default constructor or `null` is passed in `Engage.identify`, user will be treated as anonymous.

Example `identify` call:

```java
 Engage.identify(new Traits("4981498")
                .setName("Jithin")
                .setAge(27)
                .setGender(Traits.GENDER.MALE)
                .setRegistrationStatus(Traits.STATUS.REGISTERED)
        );
```

### Track

The `track` method lets you record any actions your users perform. 

```java
Engage.track("eventName", properties);
```

The `track`  method has the following parameters:

| Parameter                 | Type                    | Description
| ------------------------- | ----------------------- | -------------
| `eventName`               | String             	 | The name of the event you’re tracking.
| `properties`    			| Bundle                  | A dictionary of options.

Example `track` call:

```java
Bundle properties = new Bundle();
properties.putString("content_title", "How to Create a Tracking Plan");
properties.putString("content_id", "1234");

Engage.track("play_content", properties);
```
#### Using built-in events
Engage Android SDK pre-built Events gives you the ability to track the specific actions and events in your app that are most commonly used.
For example, the content view event can be logged as follows:
```java
 ContentEventCreator eventCreator = new ContentEventCreator("series_2332");
 eventCreator.putTitle("Game of thrones")
    .putSource("Details page")
    .putType("series");
 Engage.track(eventCreator.onContentView());
```

Engage Android SDK support the following event creators: 

| Event Creator                 | Description
| ----------------------------- | --------------------------------
| `AdvertisementEventCreator` 	|Event creator for Advertisement playback events
| `ContentEventCreator` 	|Event creator for Media Content events
| `DownloadEventCreator` 	|Event creator for Content Download events
| `PlayerEventCreator` 		|Event creator for Media Playback events
| `PurchaseEventCreator` 	|Event creator for Purchase events
| `SearchEventCreator` 		|Event creator for Search events
| `UserEventCreator` 		|Event creator for User events

#### Reporting Deep Links
Engage allows reporting your Deep Links from external campaigns like SMS/Email/Facebook.
###### Step 1: Deep link integration
Follow https://developer.android.com/training/app-links/deep-linking.html to capture deep links in your app.
###### Step 2: Reporting deep link to Engage
Once the deep link URL is available, simply call  ```Engage.onDeepLinkReceived(url);``` for conversion of your deep link campaign url.
### Campaigns
Campaigns are triggered by the SDK on events that are defined in the [campaign page](https://engage.diagnal.com/campaigns).
You can register a  `CampaignTriggerListener` to know when a campaign is triggered and obtain an action-string based on user interaction with the campaign.

```java
Engage.setCampaignTriggerListener(new CampaignTriggerListener() {
            @Override
            public boolean onCampaignTriggered(EngageCampaign engageCampaign) {
                //Return false if you don't want to show the campaign dialog
                return true;
            }
			@Override
            public void onCampaignAction(EngageCampaign engageCampaign) {
                //Do post actions based on the actionString
                String actionString = engageCampaign.getActionString();
            }
        });
```
## Messaging

Engage wraps the Firebase and Huawei messaging services to provide a simple and robust method to implement remote messaging.
To listen to push notifications or remote data messages, you just need to register with the Engage Messenger in a few simple steps.

#### Step 1:  Add firebase in your project
To add Firebase to your app you'll need a Firebase project and a Firebase configuration file for your app.
1. Create a Firebase project in the [Firebase console](https://console.firebase.google.com/), if you don't already have one. If you already have an existing Google project associated with your mobile app, click Import Google Project. Otherwise, click Create New Project.
2. Click Add Firebase to your Android app and follow the setup steps. If you're importing an existing Google project, this may happen automatically and you can just [download the config file](http://support.google.com/firebase/answer/7015592).
3. When prompted, enter your app's package name. It's important to enter the package name your app is using; this can only be set when you add an app to your Firebase project.
4. At the end, you'll download a google-services.json file. You can [download this file](http://support.google.com/firebase/answer/7015592) again at any time.
5. If you haven't done so already, copy this into your project's module folder, typically app/.
6. Add rules to your root-level build.gradle file, to include the google-services plugin and the Google's Maven repository:
```java
 buildscript {
    dependencies {
        classpath 'com.google.gms:google-services:3.1.0' // google-services plugin
    }
}
allprojects {
    repositories {
       maven {
            url "https://maven.google.com" // Google's Maven repository
        }
    }
}
```
7. Then, in your module Gradle file (usually the app/build.gradle), add the apply plugin line at the bottom of the file to enable the Gradle plugin:
```java
 apply plugin: 'com.android.application'
 // .....

 // ADD THIS AT THE BOTTOM
 apply plugin: 'com.google.gms.google-services'
```
Getting a "Could not find" error? Make sure you have the latest Google Repository in the Android SDK manager
#### Step 2:  Add Huawei push in your project
1. Follow the Huawei HMS guide to add your application in the Huawei AppGallery. And enable the push notification service
https://developer.huawei.com/consumer/en/service/hms/catalog/huaweipush_v3.html
2. Once the app is created, download the `agconnect-services.json` file as put it in your app folder
3. Add the following repository in `allprojects` section of your main gradle file:  
```xml
maven {url 'http://developer.huawei.com/repo/'}
```
4. Add the Huawei App id in your manifest file:
```xml
<meta-data
   android:name="com.huawei.hms.client.appid"
   android:value="appid=xxxxxxxx" />
```
5. Add the following rules in your Proguard/R8 file:
```java
-ignorewarnings
-keepattributes *Annotation*
-keepattributes Exceptions
-keepattributes InnerClasses
-keepattributes Signature
-keepattributes SourceFile,LineNumberTable
-keep class com.hianalytics.android.**{*;}
-keep class com.huawei.updatesdk.**{*;}
-keep class com.huawei.hms.**{*;}
```
6. Set `Messenger.getInstance().setForceFirebaseMesssaging` to `true` if you want to  prioritize Firebase push over HMS on supported Huawei devices

> NOTE: If you don't need Huawei push support you need to perform only  steps 3 and 5

#### Step 3:  Get an instance of Messenger
```java
  Messenger messenger = Messenger.getInstance();
```
#### Step 4:  Register for remote messages
```java
messenger.setRemoteMessageListener(new RemoteMessageListener() {

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
                // Get HMS message data. https://developer.huawei.com/consumer/en/doc/development/HMS-References/push-RemoteMessage-cls
                remoteMessage.getHmsRemoteMessage();

                // Get Firebase message data.https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/RemoteMessage
                remoteMessage.getFirebaseRemoteMessage()
        }

        @Override
        public void onPushNotificationReceived(NotificationData data) {
		//Engage push notification campaign
        }

        @Override
        public void onTokenRefresh(Messenger.Type type, String token) {
		//Firebase instance id token or HMS push token
        }
    });
```

#### Step 5:  Show the notification
The simplest way to build a notification from `NotificationData` is to call `NotificationData.getSimpleNotificationBuilder(context)`. 

```java
NotificationManager notificationManager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
//MainActivity is the one to be opened when user clicks the notification
Intent intent = new Intent(getApplicationContext(), MainActivity.class);
NotificationCompat.Builder builder = notificationData.getSimpleNotificationBuilder(getApplicationContext(), intent);
notificationManager.notify(id, builder.build());
messenger.onNotificationReceived(notificationData);
```
Don't forget to call `messenger.onNotificationReceived(notificationData)` to let the Engage SDK know that the notification is shown.

#### Step 6:  Notification open event
Once the component(`Activity/Service/Receiver`) set in the Notification is opened, you should call `messenger.onNotificationOpened(notificationData)` to let the Engage SDK know that user interacted with the notification.
You can obtain `NotificationData` from the Intent within the component using `EXTRA_NOTIFICATION_DATA`
```java
NotificationData data = getIntent().getParcelableExtra(NotificationData.EXTRA_NOTIFICATION_DATA); 
if (data != null) {
  if (Engage.hasInitailized()) {
    Messenger.getInstance().onNotificationOpened(data);
    //You'll get the appropriate `NotificationAction` based on the user interaction with the notification.
    String action = data.getNotificationAction().getAction()
  }
}

```