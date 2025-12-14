---
title: Imports via SFTP_WIP hackathon
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
[block:api-header]
{
  "title": "Overview"
}
[/block]
You can upload your user data to Clevertap and then use this data to analyze, engage, and retain users. You can then use the power of our analytics, segmentation, experiences to enhance engagement and retention. 

In addition, you can upload profile data such as name, email address, and event data such as product purchased and the purchase amount. Once you have established a connection with our servers, you can upload data automatically at regular intervals.

#Pre-requisites

You must have an SFTP client to be able to import data into CleverTap's SFTP server. 

# Step 1 - Start Import

Go to Settings> Partner Data> Import and click the SFTP button. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c6ca9a4-AWS_Data_Import_button.png",
        "AWS_Data_Import_button.png",
        1187,
        686,
        "#f8f8fa"
      ],
      "caption": "Import Data",
      "border": true
    }
  ]
}
[/block]

# Step 2 - Add Credentials

Our SFTP server needs to recognize your SFTP client to start receiving files. To establish this secured file transfer connection, an SSH key pair needs to be generated & the public key file should be shared with Clevertap. 

Once our SFTP server receives the SSH public key, a user is created for you. After authentication, you can seamlessly start transferring data-files. This will be a passwordless connection. The user-name & server URL is available in the Saved Keys section. 

Add new credentials by clicking the +Credentials button from the Credentials tab. The Add new credentials window appears. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cdd0fcc-AWS_Data_Import_credentials_add.png",
        "AWS_Data_Import_credentials_add.png",
        492,
        531,
        "#fafafc"
      ],
      "border": true
    }
  ]
}
[/block]
Enter your credentials in the following boxes and click Save:

* Username -   Your account ID is your username. It will be populated automatically.
* Key name - It is the name of your key. It can be any name to identify your key.
* SSH key - It is your machine's SSH public key. This is the key of the machine that uploads data to Clevertap.
 
Your credentials will be verified and saved under the Credentials tab. 
[block:callout]
{
  "type": "info",
  "title": "Limit",
  "body": "You can add up to 10 credentials."
}
[/block]
Refer to the server URL displayed in the notes of the Credentials tab for connecting to the SFTP URL. 

#Step 3 - Upload CSV files

You can upload Profiles or Events data to CleverTap with an SFTP client. Each upload must be followed with a manifest file to map your data upload with CleverTap properties. 
[block:callout]
{
  "type": "info",
  "title": "File uploads",
  "body": "All files (Events and Profiles) must be uploaded in the CSV format. The manifest file is a JSON file that provides metadata for the uploaded CSV data file. The processing of the data file will start only after the manifest file is uploaded. Every manifest file must have a unique name."
}
[/block]
##Events CSV file

The Events CSV file contains all the event details that you want to send to CleverTap. 

The Events file must have the following mandatory fields:
[block:parameters]
{
  "data": {
    "0-0": "ts",
    "0-1": "Timestamp of the event",
    "1-0": "ObjectId/Identity/guid/fbid",
    "1-1": "Identity of the user",
    "h-0": "Column Header",
    "h-1": "Description"
  },
  "cols": 2,
  "rows": 2
}
[/block]
Following is an example for an Events CSV file:
[block:parameters]
{
  "data": {
    "0-0": "Purchased",
    "h-0": "event_name",
    "h-1": "time_stamp",
    "h-2": "identity",
    "h-3": "cookie_id",
    "h-4": "item_name",
    "h-5": "item_price",
    "h-6": "item_delivery_date",
    "1-0": "Purchased",
    "2-0": "Purchased",
    "3-0": "Purchased",
    "0-1": "1578503667",
    "1-1": "1578503667",
    "2-1": "1578503667",
    "3-1": "1578503667",
    "0-2": "abc@gmail.com",
    "1-2": "abc@gmail.com",
    "2-2": "abc@gmail.com",
    "3-2": "abc@gmail.com",
    "0-3": "cookie_123",
    "1-3": "cookie_123",
    "2-3": "cookie_123",
    "3-3": "cookie_123",
    "0-4": "Awesome Ring - 10",
    "1-4": "Awesome Ring - 11",
    "2-4": "Awesome Ring - 12",
    "3-4": "Awesome Ring - 13",
    "0-5": "123",
    "1-5": "123",
    "2-5": "123",
    "3-5": "123",
    "0-6": "$D_1589274432",
    "1-6": "$D_1589274432",
    "2-6": "$D_1589274432",
    "3-6": "$D_1589274432"
  },
  "cols": 7,
  "rows": 4
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Data file timestamp",
  "body": "The timestamp column(ts) of the data file must have an epoch format. \nIf there is any other column that has date-time values, mention these values in the $D_epoch format."
}
[/block]

##Profiles CSV file

The Profiles CSV file contains all the user profile details that you want to send to CleverTap. 

The Profiles file must have the following mandatory fields:
[block:parameters]
{
  "data": {
    "0-0": "ObjectId/Identity/guid",
    "h-0": "Column Header",
    "h-1": "Description",
    "0-1": "Identity of the user",
    "1-0": "ts",
    "1-1": "Timestamp of the upload"
  },
  "cols": 2,
  "rows": 2
}
[/block]
Following is an example for a Profiles CSV file:
[block:parameters]
{
  "data": {
    "h-0": "time_stamp",
    "h-1": "identity",
    "h-2": "cookie_id",
    "h-3": "id_name",
    "h-4": "id_email",
    "h-5": "id_phone",
    "h-6": "id_phone.operation",
    "h-7": "is_married",
    "0-0": "1574668407",
    "1-0": "1574668408",
    "0-1": "johndoe@gmail.com",
    "1-1": "janedoe@gmail.com",
    "0-2": "dummy_cookie-001",
    "1-2": "dummy_cookie-002",
    "0-3": "JohnDoe-001",
    "1-3": "JaneDoe-002",
    "0-4": "johndoe@abc.com",
    "1-4": "janedoe@abc.com",
    "0-5": "+918876543211",
    "1-5": "+918876543212",
    "0-6": "",
    "1-6": "",
    "0-7": "Y",
    "1-7": "N",
    "h-8": "age",
    "0-8": "21",
    "1-8": "22",
    "h-9": "Playlist",
    "0-9": "Rock",
    "1-9": "Pop",
    "2-0": "1574668409",
    "3-0": "1574668410",
    "2-1": "johnroe@gmail.com",
    "3-1": "janeroe@gmail.com",
    "2-2": "dummy_cookie-003",
    "3-2": "dummy_cookie-004",
    "2-3": "JohnRoe-003",
    "3-3": "JaneRoe-004",
    "h-10": "Playlist.operation",
    "2-10": "",
    "3-10": "",
    "2-4": "johnroe@abc.com",
    "3-4": "janeroe@abc.com",
    "2-5": "+918876543213",
    "3-5": "",
    "2-6": "$set",
    "3-6": "$delete",
    "2-7": "Y",
    "3-7": "N",
    "h-11": "",
    "2-11": "",
    "3-11": "",
    "0-10": "$add",
    "1-10": "$remove",
    "2-8": "25",
    "3-8": "28"
  },
  "cols": 11,
  "rows": 4
}
[/block]

###Operations for profile properties
You can use the following operations on profile properties: 

* $add - This operation will add value to the property array. For example,  you can add rock songs as  Playlist values.  
* $remove- This operation will remove the value from the property array. For example, you can remove Pop from the existing Playlist.
* $set - Use this operation to replace the current value. In our example, the phone number will be set as "+918876543213."
* $delete - This operation removes the property. For example, you can completely remove the property id_phone for Jane Roe and all the values in it. 
[block:callout]
{
  "type": "info",
  "body": "To add or remove multiple values in a property, create a separate row for each value.",
  "title": "Note"
}
[/block]
# Step 4 - Upload Manifest File

The next step after uploading the CSV file is uploading the manifest file. The manifest file helps us to recognize your data. 

The manifest file is a JSON file that will map the custom attributes in your CSV files with CleverTap properties.  The format for the manifest file is  `anyfilename.manifest`. The following keys are mandatory in a manifest file:

[block:parameters]
{
  "data": {
    "0-0": "type",
    "0-1": "The event/profile/(any other type of data that we may support in the future).",
    "1-0": "fileName",
    "1-1": "The name of the data file.",
    "2-0": "clientEmail",
    "2-1": "This is a mandatory field.  We send the update mails to this email ID.",
    "h-0": "Expected keys",
    "h-1": "Description",
    "3-0": "columns",
    "3-1": "This is an optional field. \n\n* ctName - This is an optional field. The actual property name in CleverTap.\n* dataType - This is an optional field. The supported dataTypes are STRING, INTEGER, FLOAT, and LONG. If a datType is not defined, then it is considered as STRING."
  },
  "cols": 2,
  "rows": 4
}
[/block]

##Manifest file for Events

Create a manifest file to map your custom events to CleverTap Events. 
Following is a sample manifest file for Events:
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"fileName\": \"events.csv\",\n\t\"type\": \"event\",\n\t\"columns\": {\n\t\t\"event_name\": {\n\t\t\t\"ctName\": \"evtName\",\n        \t\t\"dataType\": \"STRING\",\n     \n\t\t},\n\t\t\"time_stamp\": {\n\t\t\t\"ctName\": \"ts\",\n      \t\t\t\"dataType\": \"INTEGER\"\n\t\t},\n\t\t\"identity\": {\n\t\t\t\"ctName\": \"identity\",\n      \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"cookie_id\": {\n\t\t\t\"ctName\": \"objectId\",\n      \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"item_name\": {\n\t\t\t\"ctName\": \"Name\",\n      \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"item_price\": {\n\t\t\t\"ctName\": \"Price\",\n      \t\t\t\"dataType\": \"FLOAT\"\n\t\t},\n\t\t\"item_delivery_date\": {\n\t\t\t\"ctName\": \"DeliveryDate\",\n      \t\t\t\"dataType\": \"STRING\"\n\t\t}\n\t},\n\t\"clientEmail\": \"admin@clientdomain.com\"\n  \n  \n}",
      "language": "json"
    }
  ]
}
[/block]
##Manifest file for Profiles

Create a manifest file to map the details of your user profiles to CleverTap Profiles.  

Following is a sample manifest file for Profiles:

[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"fileName\": \"profiles.csv\",\n\t\"type\": \"profile\",\n\t\"columns\": {\n\t\t\"time_stamp\": {\n\t\t\t\"ctName\": \"ts\",\n      \t\t\t\"dataType\": \"INTEGER\"   \n\t\t},\n\t\t\"identity\": {\n\t\t\t\"ctName\": \"identity\",\n       \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"cookie_id\": {\n\t\t\t\"ctName\": \"objectId\",\n       \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"id_name\": {\n\t\t\t\"ctName\": \"Name\",\n       \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"id_email\": {\n\t\t\t\"ctName\": \"Email\",\n       \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"id_phone\" : {\n\t\t\t\"ctName\": \"Phone\",\n       \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"is_married\": {\n\t\t\t\"ctName\": \"Married\",\n       \t\t\t\"dataType\": \"STRING\"\n\t\t},\n\t\t\"age\": {\n\t\t\t\"ctName\": \"Age\",\n       \t\t\t\" dataType\": \"INTEGER\"\n\t\t}\n\t},\n\t\"clientEmail\": \"admin@clientdomain.com\"\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Manifest note",
  "body": "If a column is not mapped to a CleverTap property name, the data is processed with the column name as the property name."
}
[/block]

#Step 5 -  Upload Data

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
[block:parameters]
{
  "data": {
    "h-0": "time_stamp",
    "h-1": "identity",
    "h-2": "cookie_id",
    "h-3": "id_name",
    "h-4": "id_email",
    "h-5": "id_phone",
    "h-6": "is_married",
    "h-7": "age",
    "0-0": "1574668407",
    "1-0": "1574668408",
    "0-1": "johndoe@gmail.com",
    "1-1": "janedoe@gmail.com",
    "0-2": "dummy_cookie-001",
    "1-2": "dummy_cookie-002",
    "0-3": "JohnDoe-001",
    "1-3": "JaneDoe-003",
    "0-4": "johndoe@abc.com",
    "1-4": "janedoe@abc.com",
    "0-5": "+918876543211",
    "1-5": "+918876543212",
    "0-6": "Y",
    "1-6": "N",
    "0-7": "21",
    "1-7": "22",
    "h-8": "Failure Description",
    "0-8": "Phone number is not valid",
    "1-8": "Phone number is not valid"
  },
  "cols": 9,
  "rows": 2
}
[/block]
##Troubleshoot errors

There may be instances where you may see some errors during file upload. Following is an explanation of the errors and the steps to resolve them:
[block:parameters]
{
  "data": {
    "h-0": "Error name",
    "h-1": "Resolution",
    "0-0": "No data file found",
    "1-0": "File couldn't be read",
    "2-0": "Internal infra error",
    "3-0": "Incorrect details in manifest file",
    "0-1": "The data file is not uploaded. Upload the data file again and create a new manifest file for it.",
    "1-1": "The file is not in the expected format. Upload the file again with the correct format, that is, CSV for data files and manifest file for mapping.",
    "2-1": "Upload the file again followed by a new manifest file.",
    "3-1": "This error file is emailed to you. Check the manifest file for incorrect details.",
    "4-0": "Timestamp is mandatory and must be in epoch format",
    "5-0": "Atleast one of identity / fbId / gpId / objectId is mandatory under 1024 chars to identify the user",
    "6-0": "[Internal error] Error while importing data",
    "7-0": "Event name is mandatory",
    "8-0": "Event name belongs to CleverTap restricted system events",
    "9-0": "Cannot have more than 100 event properties",
    "10-0": "Profile operation must be one of $add, $set, $remove, $delete",
    "4-1": "Check the format for timestamp. So a date such as \"Thursday, March 5, 2020 9:55:27 AM\" should be represented as \"1583402127\".",
    "5-1": "Check that a minimum of one ID is present.",
    "6-1": "This is an internal error. Try importing the data again.",
    "7-1": "The event name is blank or the column name is not available.",
    "8-1": "Do not use the event names of the existing system events.",
    "9-1": "A maximum of 100 event properties is allowed. Check that the event properties do not exceed it.",
    "10-1": "Add a valid profile operation."
  },
  "cols": 2,
  "rows": 11
}
[/block]
The status for each file is sent via email. 

# Receive imports notification
The data files will be picked up in a queue for ingestion as soon as they are uploaded. You can configure a webhook to receive notification upon completion of ingestion.  To configure a webhook, click on Receive Notifications and choose an already configured webhook from the list. 

# View imports
After a successful import, you can view your imported files on the CleverTap dashboard. 
Click Settings > Import > Imports tab to check for your uploaded files.  The Imports tab displays your uploaded files and their status. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e6ac85d-AWS_Data_Import_Files_Status.png",
        "AWS_Data_Import_Files_Status.png",
        1436,
        764,
        "#f6f6f4"
      ],
      "border": true
    }
  ]
}
[/block]

#FAQs

Q. What is the maximum file size for upload?
A. You can upload a maximum of 5 GB for each data file. 
 
Q. How much time will it take to complete the file processing?
A. It will take up to 2 hours to complete the processing for a maximum upload of a 5 GB data file.

Q. Is segmentation possible on the uploaded data? 
A. Yes.

Q. Is engagement possible from the uploaded data? 
A. Yes. You can use our engagement channels to engage with your users. 

Q. Do you support live campaigns on the uploaded data?
A. Yes. However, the support is available only for Push, Email, and SMS engagement channels.

Q. Can we do uploads at regular intervals?
A. Yes.  You can write a script to connect to CleverTap SFTP and transfer data every day when it is ready.

Q. Can we upload any event or profile file without the manifest file?
A. No. Every data file is recognized by a unique manifest file. Every manifest file should have a unique name.

Q. Can we upload any event or profile file without the column headers in the CSV file?
A. Column headers are mandatory for a file. Without the column headers, the file will go into an error state.

Q. What are the mandatory columns in the event and profile data files?
A. The column headers mandatory are ObjectId & ts.

REVIEW EXISTING LIVE PAGE AND ASSESS WHAT INFO CAN BE INCORPORATED FROM THE HACKATHON COPY BELOW

FAQ

Q: If we do not have information about a certain property/ header, can we keep the cell blank for that identity?
A:  No. If any cell is left blank, we skip the row and read the remaining data. 

Q: Do we support uploading Charged event via SFTP?
A: Charged event can be uploaded via SFTP. However, the items array can NOT be uploaded via SFTP. APIs or SDK methods can be used to upload the Charged - Items array.

Q: How can we upload integer values in SFTP? 
A: Any value uploaded without quotes will be treated like a Number. SFTP data type handling is similar to data type handling of the API upload.

Q: Can we upload mobile tokens via SFTP?
A: No, as of now we can not upload mobile tokens via SFTP. APIs can be used to upload mobile tokens