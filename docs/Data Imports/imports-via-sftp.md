---
title: Imports via SFTP
excerpt: ''
deprecated: false
hidden: false
link:
  new_tab: false
  url: https://docs.clevertap.com/docs/imports-via-sftp
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Overview

You can import user and event data from third-party SFTP clients such as Bigquery, Hana, Salesforce, etc. to the CleverTap SFTP server. After establishing the connection with the CleverTap SFTP, you can directly import such data to our dashboard automatically at regular intervals. You can then use the power of our analytics, segmentation, and experiences to enhance engagement and retention.

# Prerequisites

The following are the prerequisites for importing data via SFTP:

* You must have an SFTP client to import data into CleverTap's SFTP server. 
* Ensure that your plan includes the SFTP feature.

# Establish Connection with SFTP Server

The CleverTap SFTP server needs to recognize the SFTP client to start receiving files. To establish this secured file transfer connection:

1. Go to *Settings* > *Partner* > *Import* and click **SFTP** . 

<Image title="Import Data via SFTP" alt={2716} align="center" border={true} src="https://files.readme.io/f8dafbf-Import_via_SFTP.png">
  Import Data via SFTP
</Image>

2. Select the *Saved Keys* tab and click **+ Add key**. On clicking, *Add new import key* popup opens.

<Image title="Add New Import Key" alt={492} align="center" border={true} src="https://files.readme.io/cdd0fcc-AWS_Data_Import_credentials_add.png">
  Add New Import Key
</Image>

3. Enter the following details:

| Field                     | Description                                                                        |
| :------------------------ | :--------------------------------------------------------------------------------- |
| Username                  | Prepopulated field that displays your CleverTap Account ID.                        |
| Name your import key      | Enter the name for your import key.                                                |
| Paste your SSH public key | Enter the SSH key pair generated and shared with Clevertap in the public key file. |

After our SFTP server receives the SSH public key, a user is created for you. After successful authentication, you can seamlessly start transferring data files. You can refer to the server URL at the bottom of the *Saved Keys* tab for connecting to the SFTP URL. 

# Import Files via SFTP

To import files, use any SFTP client and execute the following commands from the command prompt to upload your files:

1. `sftp -i ~/.ssh/id_rsa username@serverURL`
2. `put filename.csv`
3. `put file.manifest`

You receive an email notification for the following:

* When the file is picked up for processing.
* When it takes longer than expected to process the file, you receive an email notification for file processing every 2 hours till the time file is processed.
* Upon successful file upload and processing.
* When the import fails for some reason, you receive a failure email with a CSV file attachment containing failed records' details. You can download this CSV file and check it for errors. 

> 📘 Private Beta Release
>
> The above feature of sending an email notification to users at different milestones is released in private Beta.

Moreover, you can start using this data in our engagement channels.\
The following is an example of an error file. The last column in this file displays all the errors.  

| time\_stamp | identity                                      | cookie\_id        | id\_name    | id\_email                                 | id\_phone      | is\_married | age | Failure Description       |
| :---------- | :-------------------------------------------- | :---------------- | :---------- | :---------------------------------------- | :------------- | :---------- | :-- | :------------------------ |
| 1574668407  | [johndoe@gmail.com](mailto:johndoe@gmail.com) | dummy\_cookie-001 | JohnDoe-001 | [johndoe@abc.com](mailto:johndoe@abc.com) | \+918876543211 | Y           | 21  | Phone number is not valid |
| 1574668408  | [janedoe@gmail.com](mailto:janedoe@gmail.com) | dummy\_cookie-002 | JaneDoe-003 | [janedoe@abc.com](mailto:janedoe@abc.com) | \+918876543212 | N           | 22  | Phone number is not valid |

For more information about CSV and manifest files, refer to [Sample CSV Files for Upload](doc:imports-via-sftp#sample-csv-files-for-upload) and [Sample Manifest File for Upload](doc:imports-via-sftp#sample-manifest-file-for-upload) sections.

# Sample CSV Files for Upload

You can upload profiles or events data to CleverTap with an SFTP client. All files (Events and Profiles) must be uploaded in CSV format. 

> ❗️ Order of File Upload
>
> A manifest file upload must follow each CSV file upload to map your data upload with CleverTap properties. The data file processing will start only after the manifest file is uploaded.

## Events CSV file

The events CSV file contains all the event details that you want to send to CleverTap. The events file has the following fields:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        <p>Column Header</p>
      </th>

      <th>
        <p>Description</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p>ts</p>
      </td>

      <td>
        <ul><li>This field is optional.</li><li> Supports the String data type.</li><li>Accepts only the Epoch timestamp values.</li> <li>Denotes the timestamp when the event occurred.</li><li>If this information is not present in the file, it captures the timestamp when CleverTap processed the record.</li></ul>
      </td>
    </tr>

    <tr>
      <td>
        <p>ObjectId/Identity/guid/fbid</p>
      </td>

      <td>
        <ul><li>This field is mandatory.</li><li>Uniquely identifies the user who performed the event.</li></ul>
      </td>
    </tr>
  </tbody>
</Table>

The following is an example of an events CSV file:

| event\_name | time\_stamp | identity                              | cookie\_id  | item\_name        | item\_price | item\_delivery\_date | status\_delivered |
| :---------- | :---------- | :------------------------------------ | :---------- | :---------------- | :---------- | :------------------- | :---------------- |
| Purchased   | 1578503667  | [abc@gmail.com](mailto:abc@gmail.com) | cookie\_123 | Awesome Ring - 10 | 123         | $D\_1589274432       | true              |
| Purchased   | 1578503667  | [abc@gmail.com](mailto:abc@gmail.com) | cookie\_123 | Awesome Ring - 11 | 123         | $D\_1589274432       | false             |
| Purchased   | 1578503667  | [abc@gmail.com](mailto:abc@gmail.com) | cookie\_123 | Awesome Ring - 12 | 123         | $D\_1589274432       | 1                 |
| Purchased   | 1578503667  | [abc@gmail.com](mailto:abc@gmail.com) | cookie\_123 | Awesome Ring - 13 | 123         | $D\_1589274432       | 0                 |

> 📘 Data File Timestamp
>
> The data file's timestamp column(ts) must have an epoch format.\
> If any other column has date-time values, mention these in the $D\_epoch format.

## Profiles CSV file

The profiles CSV file contains all the user profile details you want to send to CleverTap.\
The Profiles file has the following fields:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        <p>Column Header</p>
      </th>

      <th>
        <p>Description</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p>ObjectId/Identity/guid</p>
      </td>

      <td>
        <ul><li>This field is mandatory.</li><li>Uniquely identifies the user.</li></ul>
      </td>
    </tr>

    <tr>
      <td>
        <p>ts</p>
      </td>

      <td>
        <ul><li>This field is optional.</li><li>Indicates the timestamp of the file upload. </li><li>If this information is not given in the file, it indicates the timestamp when CleverTap when CleverTap processed the file.</li></ul>
      </td>
    </tr>
  </tbody>
</Table>

The following is an example of a profile CSV file:

| time\_stamp | identity                                      | cookie\_id        | id\_name    | id\_email                                 | id\_phone      | id\_phone.operation | is\_married | age | Playlist | Playlist.operation | MSG-SMS | MSG-DNDSMS |
| :---------- | :-------------------------------------------- | :---------------- | :---------- | :---------------------------------------- | :------------- | :------------------ | :---------- | :-- | :------- | :----------------- | :------ | :--------- |
| 1574668407  | [johndoe@gmail.com](mailto:johndoe@gmail.com) | dummy\_cookie-001 | JohnDoe-001 | [johndoe@abc.com](mailto:johndoe@abc.com) | \+918876543211 |                     | true        | 21  | Rock     | $add               | true    | false      |
| 1574668408  | [janedoe@gmail.com](mailto:janedoe@gmail.com) | dummy\_cookie-002 | JaneDoe-002 | [janedoe@abc.com](mailto:janedoe@abc.com) | \+918876543212 |                     | false       | 22  | Pop      | $remove            | false   | true       |
| 1574668409  | [johnroe@gmail.com](mailto:johnroe@gmail.com) | dummy\_cookie-003 | JohnRoe-003 | [johnroe@abc.com](mailto:johnroe@abc.com) | \+918876543213 | $set                | 1           | 25  |          |                    | 1       | 0          |
| 1574668410  | [janeroe@gmail.com](mailto:janeroe@gmail.com) | dummy\_cookie-004 | JaneRoe-004 | [janeroe@abc.com](mailto:janeroe@abc.com) | \+918876543213 | $delete             | 0           | 28  |          |                    | 0       | 1          |
|             | [janeroe@gmail.com](mailto:janeroe@gmail.com) | dummy\_cookie-005 | JaneRoe-005 | [janeroe@abc.com](mailto:janeroe@abc.com) | \+918876543213 |                     |             | 32  |          |                    |         |            |

### Operations for Profile Properties

You can use the following operations on profile properties: 

* $add - This operation will add value to the property array. For example,  you can add rock songs as  Playlist values.  
* $remove- This operation will remove the value from the property array. For example, you can remove Pop from the existing Playlist.
* $set - Use this operation to replace the current value. In our example, the phone number will be set as "+918876543213."
* $delete - This operation removes the property. For example, you can completely remove the property id\_phone for Jane Roe and all the values in it. 

### Communication Preferences for Profiles

The following are the communication preferences available:

* MSG-sms: This field in the file helps to know if the user wants to be notified via an SMS or not. 
* MSG-email: This field in the file helps to know if the user wants to be notified via email or not.
* MSG-whatsapp: This field in the file helps to know if the user wants to be notified via WhatsApp channel or not.
* MSG-push: This field in the file helps to know if the user wants to be notified via a push notification or not.
* MSG-dndSMS: This field in the file helps to know if the user has enabled DND for SMS notifications.
* MSG-dndEmail: This field in the file helps to know if the user has enabled DND for email notifications. 

> 📘 Note
>
> To add or remove multiple values in a property, create a separate row for each value.

# Sample Manifest File for Upload

The manifest file is a JSON file that maps the custom attributes in your CSV files with CleverTap properties. The format for the manifest file is `anyfilename.manifest`. Every manifest file must have a unique name. The following keys are mandatory in a manifest file:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        <p>Expected keys</p>
      </th>

      <th>
        <p>Description</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p>type</p>
      </td>

      <td>
        <p>The event/profile/(any other type of data that we may support in the future).</p>
      </td>
    </tr>

    <tr>
      <td>
        <p>fileName</p>
      </td>

      <td>
        <p>The name of the data file.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p>clientEmail</p>
      </td>

      <td>
        <p>This is a mandatory field.  We send the update mails to this email ID.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p>columns</p>
      </td>

      <td>
        <p>This is an optional field. </p><ul><li>ctName - This is an optional field. The actual property name in CleverTap.</li><li>dataType - This is an optional field. The supported dataTypes are STRING, INTEGER, FLOAT, BOOLEAN, and LONG. If a dataType is not defined, then it is considered STRING.</li></ul>
      </td>
    </tr>
  </tbody>
</Table>

> 📘 Manifest Note
>
> If a column is not mapped to a CleverTap property name, the data is processed with the column name as the property name.

## Manifest File for Events

Create a manifest file to map your custom events to CleverTap Events. The following is a sample manifest file for events:

```json
{
    "fileName": "events.csv",
    "type": "event",
    "columns": {
        "event_name": {
            "ctName": "evtName",
                "dataType": "STRING"
        },
		"time_stamp": {
			"ctName": "ts",
      			"dataType": "INTEGER"
		},
		"identity": {
			"ctName": "identity",
      			"dataType": "STRING"
		},
		"cookie_id": {
			"ctName": "objectId",
      			"dataType": "STRING"
		},
		"item_name": {
			"ctName": "Name",
      			"dataType": "STRING"
		},
		"item_price": {
			"ctName": "Price",
      			"dataType": "FLOAT"
		},
		"item_delivery_date": {
			"ctName": "DeliveryDate",
      			"dataType": "STRING"
		},
    "status_delivered": {
      "ctName": "DeliveryStatus",
            "dataType": "BOOLEAN"
    }
},
    "clientEmail": "admin@clientdomain.com"
  
}
```

> 📘 Charged Event
>
> In the case of SFTP import, CleverTap does not support importing items array for the charged event.

## Manifest File for Profiles

Create a manifest file to map the details of your user profiles to CleverTap Profiles. The following is a sample manifest file for profiles:

```json
{
	"fileName": "profiles.csv",
	"type": "profile",
	"columns": {
		"time_stamp": {
			"ctName": "ts",
      			"dataType": "INTEGER"   
		},
		"identity": {
			"ctName": "identity",
       			"dataType": "STRING"
		},
		"cookie_id": {
			"ctName": "objectId",
       			"dataType": "STRING"
		},
		"id_name": {
			"ctName": "Name",
       			"dataType": "STRING"
		},
		"id_email": {
			"ctName": "Email",
       			"dataType": "STRING"
		},
		"id_phone" : {
			"ctName": "Phone",
       			"dataType": "STRING"
		},
		"is_married": {
			"ctName": "Married",
       			"dataType": "BOOLEAN"
		},
		"age": {
			"ctName": "Age",
       			"dataType": "INTEGER"
	  },
    "MSG-SMS": {
      "ctName": "MSG-sms",
            "dataType": "BOOLEAN"
    },
    "MSG-DNDSMS": {
      "ctName": "MSG-dndSMS",
            "dataType": "BOOLEAN"
    }
},
    "clientEmail": "admin@clientdomain.com"
}
```

# Troubleshooting

There may be instances where you see some errors during file upload. The following table lists the errors that you may encounter and the steps to resolve them:

| Error Message                                                                                       | Resolution                                                                                                                                        |
| :-------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| No data file found                                                                                  | The data file is not uploaded. Upload the data file again and create a new manifest file for it.                                                  |
| File couldn't be read                                                                               | The file is not in the expected format. Upload the file again with the correct format, that is, CSV for data files and manifest file for mapping. |
| Internal infra error                                                                                | Upload the file again followed by a new manifest file.                                                                                            |
| Incorrect details in manifest file                                                                  | This error file is emailed to you. Check the manifest file for incorrect details.                                                                 |
| Atleast one of identity / fbId / gpId / objectId is mandatory under 1024 chars to identify the user | Check that a minimum of one ID is present.                                                                                                        |
| [Internal error] Error while importing data                                                         | This is an internal error. Try importing the data again.                                                                                          |
| Event name is mandatory                                                                             | The event name is blank or the column name is not available.                                                                                      |
| Event name belongs to CleverTap restricted system events                                            | Do not use the event names of the existing system events.                                                                                         |
| Cannot have more than 100 event properties                                                          | A maximum of 100 event properties is allowed. Check that the event properties do not exceed it.                                                   |
| Profile operation must be one of $add, $set, $remove, $delete                                       | Add a valid profile operation.                                                                                                                    |

The status for each file is sent via email. 

# Receive Imports Notification

The data files are picked up for ingestion in the order in which they are uploaded. You can configure a webhook to receive notifications for the following:

* When the file is picked up for processing.
* When it takes longer than expected to process the file, you receive an email notification for file processing every 2 hours till the time file is processed.
* Upon successful file upload and processing.
* When the import fails for some reason, you receive a failure email with a CSV file attachment containing failed records' details. You can download this CSV file and check it for errors. 

To configure a webhook, click **Receive Notifications** and choose an already configured webhook from the list. 

<Image title="Receive Notifications for Data Import via SFTP" alt={1439} align="center" border={true} src="https://files.readme.io/26bd2b6-Receive_notifcations.png">
  Receive Notifications for Data Import
</Image>

# View imports

After a successful import, you can view your imported files on the CleverTap dashboard.\
Click *Settings* > *Import* > *Activity log* tab to check for your uploaded files. This tab displays your uploaded files and their status. 

# Supported Date Time Format

CleverTap supports multiple Date Time formats, as shown in the following table:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Format
      </th>

      <th>
        Description
      </th>

      <th>
        Example
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        dd/MM/yyyy HH:mm:ss
      </td>

      <td>
        * \*dd\*\*: Indicates days from 01 to 31  
        * \*MM\*\*: Indicates months from 01 to 12  
        * \*yyyy\*\*: Indicates 4-digit year  
        * \*HH\*\*: Indicates 2-digit hours from 00 to 23  
        * \*mm\*\*: Indicates 2-digit minutes from 00 to 59  
        * \*ss\*\*: Indicates 2-digit seconds from 00 to 59
      </td>

      <td>
        14/04/2023 01:46:23, 14/04/2023
      </td>
    </tr>

    <tr>
      <td>
        MM/dd/yyyy HH:mm:ss
      </td>

      <td>
        * \*MM\*\*: Indicates months from 01 to 12  
        * \*dd\*\*: Indicates days from 01 to 31  
        * \*yyyy\*\*: Indicates 4-digit year  
        * \*HH\*\*: Indicates 2-digit hours from 00 to 23  
        * \*mm\*\*: Indicates 2-digit minutes from 00 to 59  
        * \*ss\*\*: Indicates 2-digit seconds from 00 to 59
      </td>

      <td>
        04/14/2023 01:46:23, 04/14/2023
      </td>
    </tr>

    <tr>
      <td>
        yyyy/dd/MM HH:mm:ss
      </td>

      <td>
        * \*yyyy\*\*: Indicates 4-digit year  
        * \*dd\*\*: Indicates days from 01 to 31  
        * \*MM\*\*: Indicates months from 01 to 12  
        * \*HH\*\*: Indicates 2-digit hours from 00 to 23  
        * \*mm\*\*: Indicates 2-digit minutes from 00 to 59  
        * \*ss\*\*: Indicates 2-digit seconds from 00 to 59
      </td>

      <td>
        2023/14/04 01:46:23, 2023/14/04
      </td>
    </tr>

    <tr>
      <td>
        yyyy/MM/dd HH:mm:ss
      </td>

      <td>
        * \*yyyy\*\*: Indicates 4-digit year  
        * \*MM\*\*: Indicates months from 01 to 12  
        * \*dd\*\*: Indicates days from 01 to 31  
        * \*HH\*\*: Indicates 2-digit hours from 00 to 23  
        * \*mm\*\*: Indicates 2-digit minutes from 00 to 59  
        * \*ss\*\*: Indicates 2-digit seconds from 00 to 59
      </td>

      <td>
        2023/04/14 01:46:23, 2023/04/14
      </td>
    </tr>

    <tr>
      <td>
        dd MMM yyyy HH:mm:ss
      </td>

      <td>
        * \*dd\*\*: Indicates days from 01 to 31  
        * \*MMM\*\*: Indicates months from JAN-DEC  
        * \*yyyy\*\*: Indicates 4-digit year  
        * \*HH\*\*: Indicates 2-digit hours from 00 to 23  
        * \*mm\*\*: Indicates 2-digit minutes from 00 to 59  
        * \*ss\*\*: Indicates 2-digit seconds from 00 to 59
      </td>

      <td>
        14 APR 2023 01:46:23, 14 APR 2023 
      </td>
    </tr>

    <tr>
      <td>
        Unix
      </td>

      <td>
        Tracks time as a running total of seconds.
      </td>

      <td>
        1681460089
      </td>
    </tr>
  </tbody>
</Table>

> 📘 Key Points to Remember
>
> * CleverTap supports uploading more than one field of type DATE with different date formats 
> * If the timestamp is missing in any of the fileds of type DATE, CleverTap adds the default timestamp of 00:00:01 to the date value included in the file when uploading it to the dashboard. For example, if the format defined  in the manifest file for a particular DATE field is `dd/MM/yyyy HH:mm:ss` and the file includes the value as `14/04/2023`, then the CleverTap stores the value as `14/04/2023 00:00:01` to the dashboard.

# FAQs

**Q. What is the maximum file size for upload?**\
A. You can upload a maximum of 5 GB for each data file. 

**Q. How much time does it take to complete the file processing?**\
A. It will take up to 2 hours to complete the processing for a maximum upload of a 5 GB data file.

**Q. Is segmentation possible on the uploaded data?**\
A. Yes.

**Q. Is engagement possible from the uploaded data?**\
A. Yes. You can use our engagement channels to engage with your users. 

**Q. Do you support live campaigns on the uploaded data?**\
A. Yes. However, the support is available only for Push, Email, and SMS engagement channels.

**Q. Can we do uploads at regular intervals?**\
A. Yes. You can write a script to connect to CleverTap SFTP and transfer data daily when it is ready.

**Q. Can we upload any event or profile file without the manifest file?**\
A. No. Every data file is recognized by a unique manifest file. Every manifest file should have a unique name.

**Q. Can we upload any event or profile file without the column headers in the CSV file?**\
A. Column headers are mandatory for a file. Without the column headers, the file will go into an error state.

**Q. What are the mandatory columns in the event and profile data files?**\
A. The column headers mandatory are ObjectId.
