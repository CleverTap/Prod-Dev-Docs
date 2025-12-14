---
title: Google Play Console - Data Safety
excerpt: Understand the impact of the new Data Safety section in Google Play Console
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Overview

Developers will now have a transparent way to show users if and how they collect, share, and protect user data before installing an app.  
Google has introduced a new _Data safety_ section in the Play Console. Any data shared with a third party must be declared on the  [Google Play Console](https://play.google.com/console/u/0/developers/app/app-content/summary) using the newly-introduced data safety section. 

This data will be displayed on the Google Play Store by late April 2022 and help the users make a more informed choice before installing an application. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/27ea0fd-Data_Play_Safety_Example.png",
        "Data_Play_Safety_Example.png",
        3058
      ],
      "border": true
    }
  ]
}
[/block]

For more information about the new declaration required in the _Data safety_ section of the Play Console, refer to [Google Play's Data safety help](https://support.google.com/googleplay/android-developer/answer/10787469).

> 📘 Note
> 
> This document is not legal advice. It is provided for your information and convenience. We strongly recommend that you must work closely with legal and other professional advisors to determine how you can comply with Google Play Console - Data Safety guidelines.

# Data Safety

A user data type is any sensitive or personal information that can identify a user. Google will require app owners to display information about all the user data types collected in the app. You can pass the data types to CleverTap as either [User Profile](doc:concepts-user-profiles) properties or [Events](https://developer.clevertap.com/docs/concepts-events). 

After you log in to [Google Play Console](https://play.google.com/console/u/0/developers/app/app-content/summary), go to _App Content_ > _Data Safety_ and answer questions about your data sharing. These questions are categorized into three sub-sections as follows:

## Data Collection and Security

This section contains questions about data collection, security, and handling practices. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4c157c1-google_data_play_three_questions.png",
        "google_data_play_three_questions.png",
        1129
      ],
      "border": true
    }
  ]
}
[/block]

| <p>Question</p>                                                              | <p>Developer Response</p>                                                                                                                                                                                                                                                                                                                                                       |
| :--------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>Does your app collect or share any of the required user data types?</p>   | <ul><li>Yes - Answer <em>Yes</em> if you collect or share any <a href="https://developer.clevertap.com/docs/google-playstore-app-privacy#data-type">data types</a>.</li><li>No - Answer <em>No</em> if your app does not collect or share any user data types.</li></ul>                                                                                                        |
| <p>Is all of the user data collected by your app encrypted in transit?</p>   | <ul><li>Yes - Answer <em>Yes</em> if you encrypt all the data sent to your servers and other third parties. CleverTap SDK encrypts all data in transit.	</li><li>No -  Answer <em>No</em> if your app or a third party does not encrypt the collected user data.</li></ul>                                                                                                      |
| <p>Do you provide a way for users to request that their data is deleted?</p> | <ul><li>Yes - Answer <em>Yes</em> if your app allows users to request the deletion of their data. CleverTap provides a way to request the deletion of data. Refer to <a href="https://developer.clevertap.com/docs/delete-user-profile-api">Delete User API</a></li><li>No - Answer <em>No</em> if you do not provide your users with an option to delete their data.</li></ul> |

> 📘 Third-party Data Share
> 
> CleverTap does not share data with any third parties.

## Data Types

Through this section, the app owner can declare all the data types collected by the app. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bff9559-data_safety_data_types.png",
        "data_safety_data_types.png",
        1136
      ],
      "border": true
    }
  ]
}
[/block]

> 📘 Data Shared with CleverTap
> 
> CleverTap does not collect any of the data types by default. However, you can configure your app to share data types with CleverTap.

The following table will help you with privacy declaration for data types:  

[block:parameters]
{
  "data": {
    "h-0": "Category",
    "h-1": "Data Type",
    "h-2": "Description",
    "h-3": "Is Shared with CleverTap by Default?",
    "h-4": "Is Optional Sharing Possible with CleverTap?",
    "0-0": "**Location** ",
    "0-1": "Approximate location",
    "0-2": "User or device physical location accurate to within an area greater than or equal to three square kilometers, such as the city a user is in, or location provided by Android’s ACCESS_COARSE_LOCATION permission.",
    "0-3": "**No**",
    "0-4": "**Yes**.  \nIf you have configured to transmit this data type, respond accordingly. CleverTap Android SDK starting v3.1.9 and above (released in May 2018) has stopped tracking approximate location by default. Approximate location is captured only if the _Device Network Information Reporting_ is enabled.",
    "1-0": "",
    "1-1": "Precise location",
    "1-2": "User or device physical location accurate to within an area less than three square kilometers, such as location provided by Android’s ACCESS_FINE_LOCATION permission.",
    "1-3": "**No**",
    "1-4": "**Yes**.Precise Location is captured only if the user integrates the CleverTap GeoFencing SDK.",
    "2-0": "**Personal info** ",
    "2-1": "Name",
    "2-2": "The way users refer to themselves, such as their first or last name, or nickname.",
    "2-3": "**No**",
    "2-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "3-0": "",
    "3-1": "Email address",
    "3-2": "The user’s email address.",
    "3-3": "**No**",
    "3-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "4-0": "",
    "4-1": "User IDs",
    "4-2": "Identifiers that relate to an identifiable person. For example, an account ID, account number, or account name.",
    "4-3": "**No**",
    "4-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "5-0": "",
    "5-1": "Address",
    "5-2": "The user’s address, such as a mailing or home address.",
    "5-3": "**No**",
    "5-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "6-0": "",
    "6-1": "Phone number",
    "6-2": "The user’s phone number.",
    "6-3": "**No**",
    "6-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "7-0": "",
    "7-1": "Race and ethnicity",
    "7-2": "Information about the user’s race or ethnicity.",
    "7-3": "**No**",
    "7-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "8-0": "",
    "8-1": "Political or religious beliefs",
    "8-2": "Information about the user’s political or religious beliefs.",
    "8-3": "**No**",
    "8-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "9-0": "",
    "9-1": "Sexual orientation",
    "9-2": "Information about the user’s sexual orientation.",
    "9-3": "**No**",
    "9-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "10-0": "",
    "10-1": "Other info",
    "10-2": "Any other personal information, such as date of birth, gender identity, veteran status, and so on.",
    "10-3": "**No**",
    "10-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "11-0": "**Financial info** ",
    "11-1": "User payment info",
    "11-2": "Information about the user’s financial accounts, such as credit card number.",
    "11-3": "**No**",
    "11-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "12-0": "",
    "12-1": "Purchase history",
    "12-2": "Information about purchases or transactions a user has made.",
    "12-3": "**No**",
    "12-4": "**Yes**.If you have configured to transmit this data to CleverTap, respond accordingly.",
    "13-0": "",
    "13-1": "Credit score",
    "13-2": "Information about the user’s credit score.",
    "13-3": "**No**",
    "13-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "14-0": "",
    "14-1": "Other financial info",
    "14-2": "Any other financial information such as user salary or debts.",
    "14-3": "**No**",
    "14-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "15-0": "**Health and fitness** ",
    "15-1": "Health info",
    "15-2": "Information about the user's health, such as medical records or symptoms.",
    "15-3": "**No**",
    "15-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "16-0": "",
    "16-1": "Fitness info",
    "16-2": "Information about the user's fitness, such as exercise or other physical activity.",
    "16-3": "**No**",
    "16-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "17-0": "**Messages** ",
    "17-1": "Emails",
    "17-2": "The user’s emails, including the email subject line, sender, recipients, and the content of the email.",
    "17-3": "**No**",
    "17-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "18-0": "",
    "18-1": "SMS or MMS",
    "18-2": "The user’s text messages including the sender, recipients, and the content of the message.",
    "18-3": "**No**",
    "18-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "19-0": "",
    "19-1": "Other in-app messages",
    "19-2": "Any other types of messages. For example, in instant messages or chat content.",
    "19-3": "**No**",
    "19-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "20-0": "**Photos or videos** ",
    "20-1": "Photos",
    "20-2": "The user’s photos.",
    "20-3": "**No**",
    "20-4": "**No**.",
    "21-0": "",
    "21-1": "Videos",
    "21-2": "The user’s videos.",
    "21-3": "**No**",
    "21-4": "**No**.",
    "22-0": "**Audio files** ",
    "22-1": "Voice or sound recordings",
    "22-2": "The user’s voice such as a voicemail or a sound recording.",
    "22-3": "**No**",
    "22-4": "**No**.",
    "23-0": "",
    "23-1": "Music files",
    "23-2": "The user’s music files.",
    "23-3": "**No**",
    "23-4": "**No**.",
    "24-0": "",
    "24-1": "Other audio files",
    "24-2": "Any other user-created or user-provided audio files.",
    "24-3": "**No**",
    "24-4": "**No**.",
    "25-0": "**Files and docs** ",
    "25-1": "Files and docs",
    "25-2": "The user’s files or documents, or information about their files or documents such as file names.",
    "25-3": "**No**",
    "25-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "26-0": "**Calendar** ",
    "26-1": "Calendar events",
    "26-2": "Information from a user’s calendar such as events, event notes, and attendees.",
    "26-3": "**No**",
    "26-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "27-0": "**Contacts** ",
    "27-1": "Contacts",
    "27-2": "Information about the user’s contacts such as contact names, message history, and social graph information such as usernames, contact recency, contact frequency, interaction duration, and call history.",
    "27-3": "**No**",
    "27-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "28-0": "**App activity** ",
    "28-1": "App interactions",
    "28-2": "Information about how users interact with the app. For example, the number of times they visit a page or the sections that they tap on.",
    "28-3": "**No**",
    "28-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "29-0": "",
    "29-1": "In-app search history",
    "29-2": "Information search performed by the user in your app.",
    "29-3": "**No**",
    "29-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "30-0": "",
    "30-1": "Installed apps",
    "30-2": "Information about the apps installed on the user's device.",
    "30-3": "**No**",
    "30-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "31-0": "",
    "31-1": "Other user-generated content",
    "31-2": "Any other user-generated content not listed here, or in any other section. For example, user bios, notes, or open-ended responses.",
    "31-3": "**No**",
    "31-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "32-0": "",
    "32-1": "Other actions",
    "32-2": "Any other user activity or actions in-app not listed here such as gameplay, likes, and dialog options.",
    "32-3": "**No**",
    "32-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "33-0": "**Web browsing** ",
    "33-1": "Web browsing history",
    "33-2": "Information about the websites that a user has visited.",
    "33-3": "**No**",
    "33-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "34-0": "**App info and performance**",
    "34-1": "Crash logs",
    "34-2": "Crash log data from user's app. For example, the number of times your app has crashed, stack traces, or other information directly related to a crash.",
    "34-3": "**No**",
    "34-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "35-0": "",
    "35-1": "Diagnostics",
    "35-2": "Information about the performance of your app. For example battery life, loading time, latency, framerate, or any technical diagnostics.",
    "35-3": "**No**",
    "35-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "36-0": "",
    "36-1": "Other app performance data",
    "36-2": "Any other app performance data not listed here.",
    "36-3": "**No**",
    "36-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly.",
    "37-0": "**Device or other IDs** ",
    "37-1": "Device or other IDs",
    "37-2": "Identifiers that relate to an individual device, browser,  \nor an app. For example, an IMEI number, MAC address, Widevine Device ID, Firebase installation ID, or advertising identifier.",
    "37-3": "**No**",
    "37-4": "**Yes**.  \nIf you have configured to transmit this data to CleverTap, respond accordingly."
  },
  "cols": 5,
  "rows": 38,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]

## Data Usage and Handling

Data types collected by an application may or may not be shared. Any data transmitted to CleverTap or any other third party by the app will be considered shared data. For example, if a user adds a phone number to the app and records this information, this is collected data. However, if the app shares the phone number with CleverTap or any other third party, then the phone number is considered collected and shared data.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/59fc505-data_safety_data_usage_handling.png",
        "data_safety_data_usage_handling.png",
        1143
      ],
      "border": true
    }
  ]
}
[/block]

For every selected data type, declare if it is collected, shared, or both. 

If your app collects data, declare the following:

- Purpose of collecting the data type  - Select the appropriate reasons for collecting this data, such as _App functionality, Analytics, Developer Communications_, and so on.
- Is the collected data type ephemeral - Is the data processed temporarily, or if it resides in the app permanently?
- User Permission - If the user can allow or disallow data collection.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c647b08-data_safety_data_usage_handling_Collected.png",
        "data_safety_data_usage_handling_Collected.png",
        1061
      ],
      "border": true,
      "caption": "Data Collection"
    }
  ]
}
[/block]

If your app shares data with a third party, declare the purpose of sharing this data. Select the appropriate reasons for sharing this data.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ef76e51-data_safety_data_usage_handling_Shared.png",
        "data_safety_data_usage_handling_Shared.png",
        1016
      ],
      "border": true,
      "caption": "Data Shared"
    }
  ]
}
[/block]

If your app collects data and also shares data with a third party, declare the purpose of collecting and sharing this data. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1df295c-data_safety_data_usage_handling_Collected_and_Shared.png",
        "data_safety_data_usage_handling_Collected_and_Shared.png",
        1193
      ],
      "border": true,
      "caption": "Data Collected and Shared"
    }
  ]
}
[/block]

# After completion

Until July 20, 2022, you can temporarily publish app updates regardless of whether Google finds issues with the information you have disclosed. If there are no issues, your app will be approved, and you need not do anything. If there are issues, revert your Data safety form's status to _Draft_ in Play Console to publish your app update. Google will also send the developer account owner an email and an inbox message in Play Console and show this information on the _Policy status_ (_Policy_ > _Policy status_) page.

After July 20, 2022, all apps will be required to have completed the Data safety form accurately to disclose their data collection and sharing practices (including apps that do not collect any user data).

If you still need further information, please create a support ticket from the CleverTap dashboard. We would be happy to help you with any other queries.

# Privacy Links

For more information about privacy links, refer to [CleverTap's Privacy Policy](https://clevertap.com/privacy-policy/).

## FAQs

Q. What information does CleverTap Android SDK collect automatically? 

CleverTap tracks the following information:

- Device model
- Cellular provider
- Network carrier
- Country code and timezone
- Wi-Fi and Bluetooth information
- Device width, height, and name

The [CleverTap Android SDK](https://github.com/CleverTap/clevertap-android-sdk) is open-source, and all the device information captured is available in our [Android Github Repository](https://github.com/CleverTap/clevertap-android-sdk/blob/17d6b9fde0cf3452d3ee4d593b4bfe9865e81bac/clevertap-core/src/main/java/com/clevertap/android/sdk/DeviceInfo.java).