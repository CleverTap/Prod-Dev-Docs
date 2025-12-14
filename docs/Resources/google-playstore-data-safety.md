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

Developers will now have a transparent way to show users if and how they collect, share, and protect user data before installing an app.\
Google has introduced a new *Data safety* section in the Play Console. Any data shared with a third party must be declared on the  [Google Play Console](https://play.google.com/console/u/0/developers/app/app-content/summary) using the newly-introduced data safety section. 

This data will be displayed on the Google Play Store by late April 2022 and help the users make a more informed choice before installing an application. 

<Image title="Data_Play_Safety_Example.png" alt={3058} className="border" border={true} src="https://files.readme.io/27ea0fd-Data_Play_Safety_Example.png" />

For more information about the new declaration required in the *Data safety* section of the Play Console, refer to [Google Play's Data safety help](https://support.google.com/googleplay/android-developer/answer/10787469).

> 📘 Note
>
> This document is not legal advice. It is provided for your information and convenience. We strongly recommend that you must work closely with legal and other professional advisors to determine how you can comply with Google Play Console - Data Safety guidelines.

# Data Safety

A user data type is any sensitive or personal information that can identify a user. Google will require app owners to display information about all the user data types collected in the app. You can pass the data types to CleverTap as either [User Profile](doc:concepts-user-profiles) properties or [Events](https://developer.clevertap.com/docs/concepts-events). 

After you log in to [Google Play Console](https://play.google.com/console/u/0/developers/app/app-content/summary), go to *App Content* > *Data Safety* and answer questions about your data sharing. These questions are categorized into three sub-sections as follows:

## Data Collection and Security

This section contains questions about data collection, security, and handling practices. 

<Image title="google_data_play_three_questions.png" alt={1129} className="border" border={true} src="https://files.readme.io/4c157c1-google_data_play_three_questions.png" />

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        <p>Question</p>
      </th>

      <th>
        <p>Developer Response</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p>Does your app collect or share any of the required user data types?</p>
      </td>

      <td>
        <ul><li>Yes - Answer <em>Yes</em> if you collect or share any <a href="https://developer.clevertap.com/docs/google-playstore-app-privacy#data-type">data types</a>.</li><li>No - Answer <em>No</em> if your app does not collect or share any user data types.</li></ul>
      </td>
    </tr>

    <tr>
      <td>
        <p>Is all of the user data collected by your app encrypted in transit?</p>
      </td>

      <td>
        <ul><li>Yes - Answer <em>Yes</em> if you encrypt all the data sent to your servers and other third parties. CleverTap SDK encrypts all data in transit.	</li><li>No -  Answer <em>No</em> if your app or a third party does not encrypt the collected user data.</li></ul>
      </td>
    </tr>

    <tr>
      <td>
        <p>Do you provide a way for users to request that their data is deleted?</p>
      </td>

      <td>
        <ul><li>Yes - Answer <em>Yes</em> if your app allows users to request the deletion of their data. CleverTap provides a way to request the deletion of data. Refer to <a href="https://developer.clevertap.com/docs/delete-user-profile-api">Delete User API</a></li><li>No - Answer <em>No</em> if you do not provide your users with an option to delete their data.</li></ul>
      </td>
    </tr>
  </tbody>
</Table>

> 📘 Third-party Data Share
>
> CleverTap does not share data with any third parties.

## Data Types

Through this section, the app owner can declare all the data types collected by the app. 

<Image title="data_safety_data_types.png" alt={1136} className="border" border={true} src="https://files.readme.io/bff9559-data_safety_data_types.png" />

> 📘 Data Shared with CleverTap
>
> CleverTap does not collect any of the data types by default. However, you can configure your app to share data types with CleverTap.

The following table will help you with privacy declaration for data types:  

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Category
      </th>

      <th>
        Data Type
      </th>

      <th>
        Description
      </th>

      <th>
        Is Shared with CleverTap by Default?
      </th>

      <th>
        Is Optional Sharing Possible with CleverTap?
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **Location** 
      </td>

      <td>
        Approximate location
      </td>

      <td>
        User or device physical location accurate to within an area greater than or equal to three square kilometers, such as the city a user is in, or location provided by Android’s ACCESS\_COARSE\_LOCATION permission.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data type, respond accordingly. CleverTap Android SDK starting v3.1.9 and above (released in May 2018) has stopped tracking approximate location by default. Approximate location is captured only if the *Device Network Information Reporting* is enabled.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Precise location
      </td>

      <td>
        User or device physical location accurate to within an area less than three square kilometers, such as location provided by Android’s ACCESS\_FINE\_LOCATION permission.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.Precise Location is captured only if the user integrates the CleverTap GeoFencing SDK.
      </td>
    </tr>

    <tr>
      <td>
        **Personal info** 
      </td>

      <td>
        Name
      </td>

      <td>
        The way users refer to themselves, such as their first or last name, or nickname.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Email address
      </td>

      <td>
        The user’s email address.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        User IDs
      </td>

      <td>
        Identifiers that relate to an identifiable person. For example, an account ID, account number, or account name.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Address
      </td>

      <td>
        The user’s address, such as a mailing or home address.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Phone number
      </td>

      <td>
        The user’s phone number.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Race and ethnicity
      </td>

      <td>
        Information about the user’s race or ethnicity.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Political or religious beliefs
      </td>

      <td>
        Information about the user’s political or religious beliefs.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Sexual orientation
      </td>

      <td>
        Information about the user’s sexual orientation.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other info
      </td>

      <td>
        Any other personal information, such as date of birth, gender identity, veteran status, and so on.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Financial info** 
      </td>

      <td>
        User payment info
      </td>

      <td>
        Information about the user’s financial accounts, such as credit card number.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Purchase history
      </td>

      <td>
        Information about purchases or transactions a user has made.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Credit score
      </td>

      <td>
        Information about the user’s credit score.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other financial info
      </td>

      <td>
        Any other financial information such as user salary or debts.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Health and fitness** 
      </td>

      <td>
        Health info
      </td>

      <td>
        Information about the user's health, such as medical records or symptoms.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Fitness info
      </td>

      <td>
        Information about the user's fitness, such as exercise or other physical activity.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Messages** 
      </td>

      <td>
        Emails
      </td>

      <td>
        The user’s emails, including the email subject line, sender, recipients, and the content of the email.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        SMS or MMS
      </td>

      <td>
        The user’s text messages including the sender, recipients, and the content of the message.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other in-app messages
      </td>

      <td>
        Any other types of messages. For example, in instant messages or chat content.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Photos or videos** 
      </td>

      <td>
        Photos
      </td>

      <td>
        The user’s photos.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*No\*\*.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Videos
      </td>

      <td>
        The user’s videos.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*No\*\*.
      </td>
    </tr>

    <tr>
      <td>
        **Audio files** 
      </td>

      <td>
        Voice or sound recordings
      </td>

      <td>
        The user’s voice such as a voicemail or a sound recording.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*No\*\*.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Music files
      </td>

      <td>
        The user’s music files.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*No\*\*.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other audio files
      </td>

      <td>
        Any other user-created or user-provided audio files.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*No\*\*.
      </td>
    </tr>

    <tr>
      <td>
        **Files and docs** 
      </td>

      <td>
        Files and docs
      </td>

      <td>
        The user’s files or documents, or information about their files or documents such as file names.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Calendar** 
      </td>

      <td>
        Calendar events
      </td>

      <td>
        Information from a user’s calendar such as events, event notes, and attendees.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Contacts** 
      </td>

      <td>
        Contacts
      </td>

      <td>
        Information about the user’s contacts such as contact names, message history, and social graph information such as usernames, contact recency, contact frequency, interaction duration, and call history.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **App activity** 
      </td>

      <td>
        App interactions
      </td>

      <td>
        Information about how users interact with the app. For example, the number of times they visit a page or the sections that they tap on.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        In-app search history
      </td>

      <td>
        Information search performed by the user in your app.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Installed apps
      </td>

      <td>
        Information about the apps installed on the user's device.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other user-generated content
      </td>

      <td>
        Any other user-generated content not listed here, or in any other section. For example, user bios, notes, or open-ended responses.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other actions
      </td>

      <td>
        Any other user activity or actions in-app not listed here such as gameplay, likes, and dialog options.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Web browsing** 
      </td>

      <td>
        Web browsing history
      </td>

      <td>
        Information about the websites that a user has visited.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **App info and performance**
      </td>

      <td>
        Crash logs
      </td>

      <td>
        Crash log data from user's app. For example, the number of times your app has crashed, stack traces, or other information directly related to a crash.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Diagnostics
      </td>

      <td>
        Information about the performance of your app. For example battery life, loading time, latency, framerate, or any technical diagnostics.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        Other app performance data
      </td>

      <td>
        Any other app performance data not listed here.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Device or other IDs** 
      </td>

      <td>
        Device or other IDs
      </td>

      <td>
        Identifiers that relate to an individual device, browser,\
        or an app. For example, an IMEI number, MAC address, Widevine Device ID, Firebase installation ID, or advertising identifier.
      </td>

      <td>
        **No**
      </td>

      <td>
        * \*Yes\*\*.\
          If you have configured to transmit this data to CleverTap, respond accordingly.
      </td>
    </tr>
  </tbody>
</Table>

## Data Usage and Handling

Data types collected by an application may or may not be shared. Any data transmitted to CleverTap or any other third party by the app will be considered shared data. For example, if a user adds a phone number to the app and records this information, this is collected data. However, if the app shares the phone number with CleverTap or any other third party, then the phone number is considered collected and shared data.

<Image title="data_safety_data_usage_handling.png" alt={1143} className="border" border={true} src="https://files.readme.io/59fc505-data_safety_data_usage_handling.png" />

For every selected data type, declare if it is collected, shared, or both. 

If your app collects data, declare the following:

* Purpose of collecting the data type  - Select the appropriate reasons for collecting this data, such as *App functionality, Analytics, Developer Communications*, and so on.
* Is the collected data type ephemeral - Is the data processed temporarily, or if it resides in the app permanently?
* User Permission - If the user can allow or disallow data collection.

<Image title="data_safety_data_usage_handling_Collected.png" alt={1061} border={true} src="https://files.readme.io/c647b08-data_safety_data_usage_handling_Collected.png">
  Data Collection
</Image>

If your app shares data with a third party, declare the purpose of sharing this data. Select the appropriate reasons for sharing this data.

<Image title="data_safety_data_usage_handling_Shared.png" alt={1016} border={true} src="https://files.readme.io/ef76e51-data_safety_data_usage_handling_Shared.png">
  Data Shared
</Image>

If your app collects data and also shares data with a third party, declare the purpose of collecting and sharing this data. 

<Image title="data_safety_data_usage_handling_Collected_and_Shared.png" alt={1193} border={true} src="https://files.readme.io/1df295c-data_safety_data_usage_handling_Collected_and_Shared.png">
  Data Collected and Shared
</Image>

# After completion

Until July 20, 2022, you can temporarily publish app updates regardless of whether Google finds issues with the information you have disclosed. If there are no issues, your app will be approved, and you need not do anything. If there are issues, revert your Data safety form's status to *Draft* in Play Console to publish your app update. Google will also send the developer account owner an email and an inbox message in Play Console and show this information on the *Policy status* (*Policy* > *Policy status*) page.

After July 20, 2022, all apps will be required to have completed the Data safety form accurately to disclose their data collection and sharing practices (including apps that do not collect any user data).

If you still need further information, please create a support ticket from the CleverTap dashboard. We would be happy to help you with any other queries.

# Privacy Links

For more information about privacy links, refer to [CleverTap's Privacy Policy](https://clevertap.com/privacy-policy/).

## FAQs

Q. What information does CleverTap Android SDK collect automatically? 

CleverTap tracks the following information:

* Device model
* Cellular provider
* Network carrier
* Country code and timezone
* Wi-Fi and Bluetooth information
* Device width, height, and name

The [CleverTap Android SDK](https://github.com/CleverTap/clevertap-android-sdk) is open-source, and all the device information captured is available in our [Android Github Repository](https://github.com/CleverTap/clevertap-android-sdk/blob/17d6b9fde0cf3452d3ee4d593b4bfe9865e81bac/clevertap-core/src/main/java/com/clevertap/android/sdk/DeviceInfo.java).
