---
title: Imports via SFTP
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
## Overview

You can upload your user data to Clevertap and then use this data to analyze, engage, and retain users. You can then use the power of our analytics, segmentation, experiences to enhance engagement and retention. 

In addition, you can upload profile data such as name, email address, and event data such as product purchased and the purchase amount. Once you have established a connection with our servers, you can upload data automatically at regular intervals.

# Pre-requisites

You must have an SFTP client to be able to import data into CleverTap's SFTP server. 

# Step 1 - Start Import

Go to Settings> Partner Data> Import and click the SFTP button. 

<Image title="AWS_Data_Import_button.png" alt={1187} border={true} src="https://files.readme.io/c6ca9a4-AWS_Data_Import_button.png">
  Import Data
</Image>

# Step 2 - Add Credentials

Our SFTP server needs to recognize your SFTP client to start receiving files. To establish this secured file transfer connection, an SSH key pair needs to be generated & the public key file should be shared with Clevertap. 

Once our SFTP server receives the SSH public key, a user is created for you. After authentication, you can seamlessly start transferring data-files. This will be a passwordless connection. The user-name & server URL is available in the Saved Keys section. 

Add new credentials by clicking the +Credentials button from the Credentials tab. The Add new credentials window appears. 

<Image title="AWS_Data_Import_credentials_add.png" alt={492} className="border" border={true} src="https://files.readme.io/cdd0fcc-AWS_Data_Import_credentials_add.png" />

Enter your credentials in the following boxes and click Save:

* Username -   Your account ID is your username. It will be populated automatically.
* Key name - It is the name of your key. It can be any name to identify your key.
* SSH key - It is your machine's SSH public key. This is the key of the machine that uploads data to Clevertap.

Your credentials will be verified and saved under the Credentials tab. 

> 📘 Limit
>
> You can add up to 10 credentials.

Refer to the server URL displayed in the notes of the Credentials tab for connecting to the SFTP URL. 

# Step 3 - Upload CSV files

You can upload Profiles or Events data to CleverTap with an SFTP client. Each upload must be followed with a manifest file to map your data upload with CleverTap properties. 

> 📘 File uploads
>
> All files (Events and Profiles) must be uploaded in the CSV format. The manifest file is a JSON file that provides metadata for the uploaded CSV data file. The processing of the data file will start only after the manifest file is uploaded. Every manifest file must have a unique name.

## Events CSV file

The Events CSV file contains all the event details that you want to send to CleverTap. 

The Events file must have the following mandatory fields:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Column Header
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        ts
      </td>

      <td>
        Timestamp of the event
      </td>
    </tr>

    <tr>
      <td>
        ObjectId/Identity/guid/fbid
      </td>

      <td>
        Identity of the user
      </td>
    </tr>
  </tbody>
</Table>

Following is an example for an Events CSV file:

<Table align={["left","left","left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        event_name
      </th>

      <th>
        time_stamp
      </th>

      <th>
        identity
      </th>

      <th>
        cookie_id
      </th>

      <th>
        item_name
      </th>

      <th>
        item_price
      </th>

      <th>
        item_delivery_date
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Purchased
      </td>

      <td>
        1578503667
      </td>

      <td>
        [abc@gmail.com](mailto:abc@gmail.com)
      </td>

      <td>
        cookie\_123
      </td>

      <td>
        Awesome Ring - 10
      </td>

      <td>
        123
      </td>

      <td>
        $D\_1589274432
      </td>
    </tr>

    <tr>
      <td>
        Purchased
      </td>

      <td>
        1578503667
      </td>

      <td>
        [abc@gmail.com](mailto:abc@gmail.com)
      </td>

      <td>
        cookie\_123
      </td>

      <td>
        Awesome Ring - 11
      </td>

      <td>
        123
      </td>

      <td>
        $D\_1589274432
      </td>
    </tr>

    <tr>
      <td>
        Purchased
      </td>

      <td>
        1578503667
      </td>

      <td>
        [abc@gmail.com](mailto:abc@gmail.com)
      </td>

      <td>
        cookie\_123
      </td>

      <td>
        Awesome Ring - 12
      </td>

      <td>
        123
      </td>

      <td>
        $D\_1589274432
      </td>
    </tr>

    <tr>
      <td>
        Purchased
      </td>

      <td>
        1578503667
      </td>

      <td>
        [abc@gmail.com](mailto:abc@gmail.com)
      </td>

      <td>
        cookie\_123
      </td>

      <td>
        Awesome Ring - 13
      </td>

      <td>
        123
      </td>

      <td>
        $D\_1589274432
      </td>
    </tr>
  </tbody>
</Table>

> 📘 Data file timestamp
>
> The timestamp column(ts) of the data file must have an epoch format.\
> If there is any other column that has date-time values, mention these values in the $D\_epoch format.

## Profiles CSV file

The Profiles CSV file contains all the user profile details that you want to send to CleverTap. 

The Profiles file must have the following mandatory fields:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Column Header
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        ObjectId/Identity/guid
      </td>

      <td>
        Identity of the user
      </td>
    </tr>

    <tr>
      <td>
        ts
      </td>

      <td>
        Timestamp of the upload
      </td>
    </tr>
  </tbody>
</Table>

Following is an example for a Profiles CSV file:

<Table align={["left","left","left","left","left","left","left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        time_stamp
      </th>

      <th>
        identity
      </th>

      <th>
        cookie_id
      </th>

      <th>
        id_name
      </th>

      <th>
        id_email
      </th>

      <th>
        id_phone
      </th>

      <th>
        id_phone.operation
      </th>

      <th>
        is_married
      </th>

      <th>
        age
      </th>

      <th>
        Playlist
      </th>

      <th>
        Playlist.operation
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1574668407
      </td>

      <td>
        [johndoe@gmail.com](mailto:johndoe@gmail.com)
      </td>

      <td>
        dummy\_cookie-001
      </td>

      <td>
        JohnDoe-001
      </td>

      <td>
        [johndoe@abc.com](mailto:johndoe@abc.com)
      </td>

      <td>
        +918876543211
      </td>

      <td>

      </td>

      <td>
        Y
      </td>

      <td>
        21
      </td>

      <td>
        Rock
      </td>

      <td>
        $add
      </td>
    </tr>

    <tr>
      <td>
        1574668408
      </td>

      <td>
        [janedoe@gmail.com](mailto:janedoe@gmail.com)
      </td>

      <td>
        dummy\_cookie-002
      </td>

      <td>
        JaneDoe-002
      </td>

      <td>
        [janedoe@abc.com](mailto:janedoe@abc.com)
      </td>

      <td>
        +918876543212
      </td>

      <td>

      </td>

      <td>
        N
      </td>

      <td>
        22
      </td>

      <td>
        Pop
      </td>

      <td>
        $remove
      </td>
    </tr>

    <tr>
      <td>
        1574668409
      </td>

      <td>
        [johnroe@gmail.com](mailto:johnroe@gmail.com)
      </td>

      <td>
        dummy\_cookie-003
      </td>

      <td>
        JohnRoe-003
      </td>

      <td>
        [johnroe@abc.com](mailto:johnroe@abc.com)
      </td>

      <td>
        +918876543213
      </td>

      <td>
        $set
      </td>

      <td>
        Y
      </td>

      <td>
        25
      </td>

      <td>

      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        1574668410
      </td>

      <td>
        [janeroe@gmail.com](mailto:janeroe@gmail.com)
      </td>

      <td>
        dummy\_cookie-004
      </td>

      <td>
        JaneRoe-004
      </td>

      <td>
        [janeroe@abc.com](mailto:janeroe@abc.com)
      </td>

      <td>

      </td>

      <td>
        $delete
      </td>

      <td>
        N
      </td>

      <td>
        28
      </td>

      <td>

      </td>

      <td>

      </td>
    </tr>
  </tbody>
</Table>

### Operations for profile properties

You can use the following operations on profile properties: 

* $add - This operation will add value to the property array. For example,  you can add rock songs as  Playlist values.  
* $remove- This operation will remove the value from the property array. For example, you can remove Pop from the existing Playlist.
* $set - Use this operation to replace the current value. In our example, the phone number will be set as "+918876543213."
* $delete - This operation removes the property. For example, you can completely remove the property id\_phone for Jane Roe and all the values in it. 

> 📘 Note
>
> To add or remove multiple values in a property, create a separate row for each value.

# Step 4 - Upload Manifest File

The next step after uploading the CSV file is uploading the manifest file. The manifest file helps us to recognize your data. 

The manifest file is a JSON file that will map the custom attributes in your CSV files with CleverTap properties.  The format for the manifest file is  `anyfilename.manifest`. The following keys are mandatory in a manifest file:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Expected keys
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        type
      </td>

      <td>
        The event/profile/(any other type of data that we may support in the future).
      </td>
    </tr>

    <tr>
      <td>
        fileName
      </td>

      <td>
        The name of the data file.
      </td>
    </tr>

    <tr>
      <td>
        clientEmail
      </td>

      <td>
        This is a mandatory field.  We send the update mails to this email ID.
      </td>
    </tr>

    <tr>
      <td>
        columns
      </td>

      <td>
        This is an optional field. 

        * ctName - This is an optional field. The actual property name in CleverTap.
        * dataType - This is an optional field. The supported dataTypes are STRING, INTEGER, FLOAT, and LONG. If a datType is not defined, then it is considered as STRING.
      </td>
    </tr>
  </tbody>
</Table>

## Manifest file for Events

Create a manifest file to map your custom events to CleverTap Events.\
Following is a sample manifest file for Events:

```json
{
	"fileName": "events.csv",
	"type": "event",
	"columns": {
		"event_name": {
			"ctName": "evtName",
        		"dataType": "STRING",
     
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
		}
	},
	"clientEmail": "admin@clientdomain.com"
  
  
}
```

## Manifest file for Profiles

Create a manifest file to map the details of your user profiles to CleverTap Profiles.  

Following is a sample manifest file for Profiles:

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
       			"dataType": "STRING"
		},
		"age": {
			"ctName": "Age",
       			" dataType": "INTEGER"
		}
	},
	"clientEmail": "admin@clientdomain.com"
}
```

> 📘 Manifest note
>
> If a column is not mapped to a CleverTap property name, the data is processed with the column name as the property name.

# Step 5 -  Upload Data

You are now ready to upload data files to our servers. 

1. Upload the data file first through SFTP. 
2. Upload the manifest file to mark the end of the data upload and begin the processing of data.

Use any SFTP client and enter the following commands to upload your files:

1. `sftp -i ~/.ssh/id_rsa username@serverURL`
2. `put filename`
3. `put file.manifest`

We will send you a success email after the upload and processing is complete. 

You can now start using this data in our engagement channels. 

If the import fails for some reason, we will send you a failure email with the attached CSV file that contains the failed records. You can download this CSV file and check it for errors. 

Following is an example of an error file. The last column in this file displays all the errors.  

<Table align={["left","left","left","left","left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        time_stamp
      </th>

      <th>
        identity
      </th>

      <th>
        cookie_id
      </th>

      <th>
        id_name
      </th>

      <th>
        id_email
      </th>

      <th>
        id_phone
      </th>

      <th>
        is_married
      </th>

      <th>
        age
      </th>

      <th>
        Failure Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        1574668407
      </td>

      <td>
        [johndoe@gmail.com](mailto:johndoe@gmail.com)
      </td>

      <td>
        dummy\_cookie-001
      </td>

      <td>
        JohnDoe-001
      </td>

      <td>
        [johndoe@abc.com](mailto:johndoe@abc.com)
      </td>

      <td>
        +918876543211
      </td>

      <td>
        Y
      </td>

      <td>
        21
      </td>

      <td>
        Phone number is not valid
      </td>
    </tr>

    <tr>
      <td>
        1574668408
      </td>

      <td>
        [janedoe@gmail.com](mailto:janedoe@gmail.com)
      </td>

      <td>
        dummy\_cookie-002
      </td>

      <td>
        JaneDoe-003
      </td>

      <td>
        [janedoe@abc.com](mailto:janedoe@abc.com)
      </td>

      <td>
        +918876543212
      </td>

      <td>
        N
      </td>

      <td>
        22
      </td>

      <td>
        Phone number is not valid
      </td>
    </tr>
  </tbody>
</Table>

## Troubleshoot errors

There may be instances where you may see some errors during file upload. Following is an explanation of the errors and the steps to resolve them:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Error name
      </th>

      <th>
        Resolution
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        No data file found
      </td>

      <td>
        The data file is not uploaded. Upload the data file again and create a new manifest file for it.
      </td>
    </tr>

    <tr>
      <td>
        File couldn't be read
      </td>

      <td>
        The file is not in the expected format. Upload the file again with the correct format, that is, CSV for data files and manifest file for mapping.
      </td>
    </tr>

    <tr>
      <td>
        Internal infra error
      </td>

      <td>
        Upload the file again followed by a new manifest file.
      </td>
    </tr>

    <tr>
      <td>
        Incorrect details in manifest file
      </td>

      <td>
        This error file is emailed to you. Check the manifest file for incorrect details.
      </td>
    </tr>

    <tr>
      <td>
        Timestamp is mandatory and must be in epoch format
      </td>

      <td>
        Check the format for timestamp. So a date such as "Thursday, March 5, 2020 9:55:27 AM" should be represented as "1583402127".
      </td>
    </tr>

    <tr>
      <td>
        Atleast one of identity / fbId / gpId / objectId is mandatory under 1024 chars to identify the user
      </td>

      <td>
        Check that a minimum of one ID is present.
      </td>
    </tr>

    <tr>
      <td>
        [Internal error] Error while importing data
      </td>

      <td>
        This is an internal error. Try importing the data again.
      </td>
    </tr>

    <tr>
      <td>
        Event name is mandatory
      </td>

      <td>
        The event name is blank or the column name is not available.
      </td>
    </tr>

    <tr>
      <td>
        Event name belongs to CleverTap restricted system events
      </td>

      <td>
        Do not use the event names of the existing system events.
      </td>
    </tr>

    <tr>
      <td>
        Cannot have more than 100 event properties
      </td>

      <td>
        A maximum of 100 event properties is allowed. Check that the event properties do not exceed it.
      </td>
    </tr>

    <tr>
      <td>
        Profile operation must be one of $add, $set, $remove, $delete
      </td>

      <td>
        Add a valid profile operation.
      </td>
    </tr>
  </tbody>
</Table>

The status for each file is sent via email. 

# Receive imports notification

The data files will be picked up in a queue for ingestion as soon as they are uploaded. You can configure a webhook to receive notification upon completion of ingestion.  To configure a webhook, click on Receive Notifications and choose an already configured webhook from the list. 

# View imports

After a successful import, you can view your imported files on the CleverTap dashboard.\
Click Settings > Import > Imports tab to check for your uploaded files.  The Imports tab displays your uploaded files and their status. 

<Image title="AWS_Data_Import_Files_Status.png" alt={1436} className="border" border={true} src="https://files.readme.io/e6ac85d-AWS_Data_Import_Files_Status.png" />

# FAQs

Q. What is the maximum file size for upload?\
A. You can upload a maximum of 5 GB for each data file. 

Q. How much time will it take to complete the file processing?\
A. It will take up to 2 hours to complete the processing for a maximum upload of a 5 GB data file.

Q. Is segmentation possible on the uploaded data?\
A. Yes.

Q. Is engagement possible from the uploaded data?\
A. Yes. You can use our engagement channels to engage with your users. 

Q. Do you support live campaigns on the uploaded data?\
A. Yes. However, the support is available only for Push, Email, and SMS engagement channels.

Q. Can we do uploads at regular intervals?\
A. Yes.  You can write a script to connect to CleverTap SFTP and transfer data every day when it is ready.

Q. Can we upload any event or profile file without the manifest file?\
A. No. Every data file is recognized by a unique manifest file. Every manifest file should have a unique name.

Q. Can we upload any event or profile file without the column headers in the CSV file?\
A. Column headers are mandatory for a file. Without the column headers, the file will go into an error state.

Q. What are the mandatory columns in the event and profile data files?\
A. The column headers mandatory are ObjectId & ts.
