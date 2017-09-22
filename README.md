[![N|Solid](http://i.imgur.com/iz9YsTS.png)](https://diagnal.com)
# Engage Android SDK #
Engage is real-time targeted marketing campaigns that drive user acquisition and conversion.
It promotes content to users and access customer sentiment.
Engage Android SDK allows easy integration of Engage to your android application in a few steps.

Engage Android SDK [class reference documents.](https://diagnal-engage.s3.amazonaws.com/engage-sdk-docs/engage-core/index.html)
Engage Android SDK requires API 17 (Android 4.2) or higher. 

## Getting Started
### Step 1: Install the Library
The Engage Android SDK is distributed as `aar` package. You can obtain the latest Engage Android SDK aar file from https://github.com/diagnal/engage-android-sdk/

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
                .setRegistrationStatus("subscribed")
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

Engage wraps the Firebase messaging services to provide a simple and robust method to implement remote messaging.
To listen to push notifications or remote data messages, you just need to register with the Engage Messenger in a few simple steps.
 
#### Step 1:  Get an instance of Messenger
```java
  Messenger messenger = Messenger.getInstance();
```
#### Step 2:  Register for remote messages
```java
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

```java
NotificationCompat.Builder builder = notificationData.getSimpleNotificationBuilder(context);
context.getSystemService(Context.NOTIFICATION_SERVICE);
notificationManager.notify(id, notificationBuilder.build());
```
Don't forget to call `messenger.onNotificationReceived(notificationData)` to let the Engage SDK know that the notification is shown.

#### Step 4:  Notification open event
Once the component(`Activity/Service/Receiver`) set in the Notification is opened, you should call `messenger.onNotificationOpened(notificationData)` to let the Engage SDK know that user interacted with the notification.
