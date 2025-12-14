---
title: Amplitude - Import Data
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
You can import data such as events and profiles from Amplitude to the CleverTap platform. Perform a data import in three easy steps.

## Step 1: Get the artifacts for data upload from the [CleverTap Repository](https://github.com/CleverTap/clevertap-data-upload). 

> 📘 Note
>
> The CleverTap script runs in `Go`. Check that you have installed `Go` on your system.  To download and install go, see [https://golang.org/dl/](https://golang.org/dl/).

## Step 2: Install the CleverTap upload tool. 

```text
go get github.com/CleverTap/clevertap-data-upload 
go install github.com/CleverTap/clevertap-data-upload
```

You can use the following arguments:

```text
-csv string               Absolute path to the csv file
  
  -id string                CleverTap Account ID
  
  -p string                 CleverTap Account Passcode
  
  -t string                 The type of data, either profile or event, defaults to profile (default "profile")
  
  -evtName string           Event name. Required only when uploading events. Each CSV file can only have one type of event
  
  -r string                 The account region, either eu, in, sk, or sg, defaults to eu (default "eu")
  
  -dryrun                   Do a dry run, process records but do not upload

  -startDate                Start date for exporting events from Mixpanel <yyyy-mm--dd>

  -endDate                  End date for exporting events <yyyy-mm-dd>

  -startTs                  Start timestamp for events upload in epoch
```

## Step 3: Upload your Amplitude data to CleverTap

You can import data from Amplitude to CleverTap. You must first save the data from Amplitude in a CSV file. You can then upload this CSV file to CleverTap. 

Following is an example of columns in a CSV file for profiles:

<Image title="Amplitude_CSV_Events.png" alt={609} className="border" border={true} src="https://files.readme.io/608c915-Amplitude_CSV_Events.png" />

Example Profiles upload from CSV:

```text
clevertap-data-upload -csv="/Users/name/Documents/in.csv" -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX"
```

Following is an example of columns in a CSV file for events:

<Image title="Amplitude_CSV_Events.png" alt={625} className="border" border={true} src="https://files.readme.io/127c0c9-Amplitude_CSV_Events.png" />

Example Events upload from CSV:

```text
clevertap-data-upload -csv="/Users/name/Documents/in.csv" -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX" -t="event" -evtName="Product Viewed"
```

> 📘 Note
>
> For CSV uploads, you must include one of identity, objectId, FBID or GPID, in your data. Email addresses can serve as an identity value, but the key must be identity.
