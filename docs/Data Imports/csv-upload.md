---
title: CSV Upload
excerpt: >-
  Learn how to upload new or update existing user profiles on the CleverTap
  dashboard using CSV Upload feature.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

With the Profile Uploads feature, you can do the following:

- Upload new [user profiles](doc:concepts-user-profiles) on the CleverTap dashboard in bulk by uploading a CSV file with a list of user profiles.
- Add or update information for existing user profiles.

> 📘 Private Beta Release
> 
> This feature is currently released in private Beta.

# Upload Profiles

To upload profiles to the CleverTap dashboard: 

1. Log in to your CleverTap account and navigate to _Settings_ > _CSV Uploads_.
2. Click **+ Upload**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/936328c-Click__Upload.gif",
        null,
        "Click + Upload to Upload CSV File"
      ],
      "align": "center",
      "border": true,
      "caption": "Click + Upload to Upload CSV File"
    }
  ]
}
[/block]

3. Drag and drop the .csv file or click **Browse** to upload the required file.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a83f055-Drag_and_Drop_or_Browse.png",
        "Drag and Drop or Browse.png",
        1800
      ],
      "align": "center",
      "border": true,
      "caption": "Drag and drop the CSV File or Browse CSV File from your System "
    }
  ]
}
[/block]

4. Enter the upload _Name_ and click **Save & Upload** to upload the file. We recommend keeping the upload _Name_ unique, as it will help you identify your upload easily.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b7bb660-Click_Save__Upload.png",
        "Click Save & Upload.png",
        1384
      ],
      "align": "center",
      "border": true,
      "caption": "Preview of CSV File being Uploaded"
    }
  ]
}
[/block]

   The file upload _Status_ changes to _Start Mapping_ at this point. 

5. Click **Map**. You have the following two options to map your uploaded user profiles to data types and user properties:
   - **Manually map file**: Select this option to manually define the _Data Type_ and _User Property_ for all the columns in your uploaded CSV file. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cde593a-Map_File.gif",
        null,
        "Mapping CSV File Manually"
      ],
      "align": "center",
      "border": true,
      "caption": "Mapping CSV File Manually"
    }
  ]
}
[/block]

> 📘 Missing User Property
> 
> If you cannot find the User Property for a particular column of your file, you can leave it undefined. CleverTap creates that user property on your dashboard at the time of file processing.

- **Upload manifest file**: Select this option if you have a manifest file that maps your user profiles to data types and user profiles on the CleverTap dashboard. You can drag and drop the file directly or browse the file location on your local system.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0396107-Upload_Manifest_File.png",
        "Upload Manifest File.png",
        2448
      ],
      "align": "center",
      "border": true,
      "caption": "Mapping CSV File by Uploading Manifest File"
    }
  ]
}
[/block]

6. Click **Process File**. You are navigated back to the Profile Upload page, and the status of the file upload changes to _Processing_. It may take up to five minutes for the file to upload. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4dd9d53-File_Processing.png",
        null,
        "File Processing"
      ],
      "align": "center",
      "border": true,
      "caption": "File Processing"
    }
  ]
}
[/block]

| <p>Field</p>       | <p>Description</p>                                                                                                                                                                                                                  |
| :----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Name</p>        | <p>Displays the upload name entered at the time of uploading the file. We recommend keeping this name unique, as it helps you identify your upload quickly.</p>                                                                     |
| <p>Uploaded on</p> | <p>Displays the date on which the file is uploaded.</p>                                                                                                                                                                             |
| <p>Uploaded by</p> | <p>Displays the email ID of the person uploading the file.</p>                                                                                                                                                                      |
| <p>Status</p>      | <ul><li>Displays the exact status of the file to be uploaded to the CleverTap dashboard.</li><li>Following are the possible values:<ul><li>Start Mapping</li><li>Processing</li><li>Completed </li><li>Rejected</li></ul></li></ul> |
| Errors             | Displays the number of records in error and not uploaded to the CleverTap dashboard.                                                                                                                                                |
| Processed          | Displays the number of records that are uploaded to the CleverTap dashboard successfully.                                                                                                                                           |

# File Format

This section provides information about the sample .csv and manifest file formats.

## CSV File Format

This file contains all the user profiles you may want to add or update on the CleverTap dashboard. You can only upload a CSV file with a size of up to 1 GB. Download the [sample CSV file](https://s3.eu-west-1.amazonaws.com/static.wizrocket.com/csv/sample-upload-csv.csv). The following is the sample CSV file:

![](https://files.readme.io/b6c4a5a-Sample_CSV_File.png "Sample CSV File.png")

**`identity`**  
`[identity](doc:concepts-user-profiles)` is a mandatory field. However, if you want to make profile updates using the CleverTap ID, you can use the `objectId` in place of identity. The following is the sample format for the CSV file using `objectId`:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bcf5586-CSV_File_with_ObjectID.png",
        "CSV File with ObjectID.png",
        1587
      ],
      "align": "center",
      "border": true
    }
  ]
}
[/block]



## Manifest File Format

This file maps the details of your user profiles to CleverTap profiles. The file name format for the manifest file is `anyfilename.manifest`. Every manifest file must have a unique name. The following keys are mandatory in a manifest file:

| <p>Expected Key</p> | <p>Description</p>                                                                                                                                                                                                                                                   |
| :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>columns</p>      | <ul><li>ctName - Represents the actual property name in CleverTap.</li><li>dataType - Represents supported dataTypes like STRING, INTEGER, FLOAT, BOOLEAN, and LONG for a particular property. If a dataType is not defined, then it is considered STRING.</li></ul> |

The following is a sample manifest file for profiles:

```json Sample Manifest File
{
	"columns": {
		"identity": {
			"ctName": "identity",
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
		"id_phone": {
			"ctName": "Phone",
			"dataType": "STRING"
		},
		"id_gender": {
			"ctName": "Gender",
			"dataType": "STRING"
		},
		"Employment Status": {
			"ctName": "empStatus",
			"dataType": "STRING"
		},
		"Education status": {
			"ctName": "education_status",
			"dataType": "STRING"
		},
		"Wallet Balance": {
			"ctName": "Wallet bBalance",
			"dataType": "INTEGER"
		},
		"DOB": {
			"ctName": "DOB",
			"dataType": "STRING"
		},
		"MSG-SMS": {
			"ctName": "MSG-sms",
			"dataType": "BOOLEAN"
		}
	}
}
```



> 📘 Field Mapping in Manifest File
> 
> If a column is not mapped to a CleverTap property name, the data is processed with the column name as the property name.

# FAQs

### Q. What is the maximum file size for upload?

A. You can upload a maximum of 1 GB for each data file.

### Q. What are the mandatory columns in the Profile Files?

A. `identity` or `Object ID` is the mandatory field for adding or updating any user profile on the CleverTap dashboard.

### Q. Can multiple CSV files be uploaded simultaneously?

 A. You can upload multiple CSV files simultaneously, even when existing files are currently processing.

### Q. Does CleverTap support uploading data with columns of data type Date?

A. Columns of data type Date are uploaded to CleverTap as String. CleverTap is working on this enhancement and will soon make it available.

### Why is there a mismatch in the number of records included in the file and uploaded to CleverTap?

A. This mismatch occurs when a few rows may not have been processed due to some error. In such cases, the total number of rows in a file = Number of rows in Error + Number of rows Processed.

### What if the user skips the mapping step at the time of file upload?

A. The user can skip mapping the columns at the time of file upload. They can go back to the _Profile Upload_ page and map the columns later.

### What are the different file upload statuses?

A. The following are different file upload statuses:

| <p>Status</p>        | <p>Description</p>                                                                                                                                                                                                                                     |
| :------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Start Mapping</p> | <p>This indicates that the user can now map the uploaded user profiles to data types and the corresponding user properties on the CleverTap dashboard.</p>                                                                                             |
| <p>Processing</p>    | <p>This indicates the file is now being processed and uploaded.</p>                                                                                                                                                                                    |
| <p>Completed</p>     | <p>This indicates one of the following:</p><ul><li>All the records have been processed successfully</li><li>A few records have been processed, and the remaining have not been processed due to some error.</li></ul>                                  |
| <p>Rejected</p>      | <p>This indicates the file was not at all uploaded due to one of the following reasons:</p><ul><li>Network issue at the user’s end at the time of file upload.</li><li>User did not complete the mapping step within 15 days of initiating the upload. |