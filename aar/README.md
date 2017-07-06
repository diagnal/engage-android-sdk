[![N|Solid](http://i.imgur.com/iz9YsTS.png)](https://diagnal.com)
# Engage Android SDK #
Engage is real-time targeted marketing campaigns that drive user acquisition and conversion.
It promotes content to users and access customer sentiment.

Engage Android SDK allows easy integration of Engage to your android application in a few steps.
Engage Android SDK requires API 17 (Android 4.2) or higher. 

## Getting Started
### Step 1: Install the Library
The Engage Android SDK is distributed as `aar` package. You can obtain the latest Engage Android SDK aar file from https://github.com/diagnal/engage-android-sdk/

Include aar file as a dependency in you project. 
The steps for adding an aar dependency can be found at https://developer.android.com/studio/projects/android-library.html#AddDependency
### Step 2:  Initialize the Client
The entry point of the SDK is through the `Engage` class. We recommend initializing the client in the `onCreate()` your Application subclass.
```javascript
Engage.initialize(applicationContext, ENGAGE_CLIENT_ID, ENGAGE_PROJECT_ID);
```
`ENGAGE_CLIENT_ID` and `ENGAGE_PROJECT_ID` can be obtained from your [Enage Dashboard](http://engage.diagnal.com )
## Usage
### Identify User

The `identify` method is how you associate your users and their actions to a recognizable `userId` and `Traits`.

```javascript
 Engage.identify(new Traits(userId)
                .setName(name)
                .setAge(age)
                .setCity(city)
                .setCountry(country)
                .setEmail(email)
                .setGender(gender)
                .setRegistrationStatus(registrationStatus)
                .setNetwork(isp)
        );
```
If `Traits` is initialized with the default constructor, user will be treated as anonymous.

Example `identify` call:

```javascript
 Engage.identify(new Traits("4981498")
                .setName("Jithin")
                .setAge(27)
                .setGender(Traits.GENDER.MALE)
                .setRegistrationStatus("subscribed")
        );
```

### Track

The `track` method lets you record any actions your users perform. You can see a track example in the guide or find details on the track method payload.

```javascript
Engage.track("eventName",properties);
```

The `track`  method has the following parameters:

| Parameter                 | Type                    | Description
| ------------------------- | ----------------------- | -------------
| `eventName`               | String             	  | The name of the event you’re tracking.
| `properties`    			| Bundle                  | A dictionary of options.

Example `track` call:

```javascript
Bundle properties = new Bundle();
properties.putString("content_title", "How to Create a Tracking Plan");
properties.putString("content_id", "1234");

Engage.track("play_content", properties);
```
### Campaigns
Campaigns are triggered by the SDK on events that are defined in the campaign page.
You can register a  `CampaignTriggerListener` to know when a campaign is triggered and obtain an action-string based on user interaction with the campaign.

```javascript
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

## Extensions
In the interest of keeping our SDK lightweight and to allow users selectively use extended Engage SDK features we use an extension based architecture.

For example, **Notification Campaign** support can be implemented by adding `engage-firebase`
extension.

Engage Extensions can be used by simply adding them as project dependencies. 
The available Engage Android Extensions can be found at https://github.com/diagnal/engage-android-sdk/tree/master/aar/extensions
