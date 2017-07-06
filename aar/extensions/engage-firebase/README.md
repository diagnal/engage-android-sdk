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
### Messaging

Engage Firebase extension wraps the Firebase messaging services to provide a simple and robust method to implement remote messaging.

To listen to push notifications or remote data messages, you just need to register with the Engage Messenger in a few simple steps.
 
#### Step 1:  Get an instance of `Messenger`
```javascript
  Messenger messenger = Messenger.getInstance();
```
#### Step 2:  Register for remote messages
```javascript
messenger.setRemoteMessageListener(new RemoteMessageListener() {
        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
		//Remote data message or notification from Firebase console
        }

        @Override
        public void onPushNotificationReceived(NotificationData data) {
		//Engage push notification campaign
        }

        @Override
        public void onTokenRefresh(String token) {
		//Firebase instance id token
        }
    });
```

#### Step 3:  Show the notification
The simplest way to build a notification from `NotificationData` is to call `NotificationData.getSimpleNotificationBuilder(context)`. 

```javascript
NotificationCompat.Builder builder = notificationData.getSimpleNotificationBuilder(context);
context.getSystemService(Context.NOTIFICATION_SERVICE);
notificationManager.notify(id, notificationBuilder.build());
```
Don't forget to call `messenger.onNotificationReceived(notificationData)` to let the Engage SDK know that the notification is shown.

#### Step 4:  Notification open event
Once the component(`Activity/Service/Receiver`) set in the Notification is opened, you should call `messenger.onNotificationOpened(notificationData)` to let the Engage SDK know that user interacted with the notification.

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