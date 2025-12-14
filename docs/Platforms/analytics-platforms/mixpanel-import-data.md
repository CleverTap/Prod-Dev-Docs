---
title: Mixpanel - Import Data
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
You can import data such as events and profiles from Mixpanel to the CleverTap platform. Perform a data import in 3 easy steps.

## Step 1: Get the artifacts for data upload from the [CleverTap Repository](https://github.com/CleverTap/clevertap-data-upload). 

> 📘 Note
>
> The CleverTap script runs in `Go`. Check that you have installed `Go` on your system.  To download and install go, see [https://golang.org/dl/](https://golang.org/dl/).

## Step 2:Install the CleverTap upload tool. 

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

  -mixpanelSecret           Mixpanel API secret key

  -startDate                Start date for exporting events from Mixpanel <yyyy-mm--dd>

  -endDate                  End date for exporting events <yyyy-mm-dd>

  -startTs                  Start timestamp for events upload in epoch
```

## Step 3: Upload your Mixpanel data to CleverTap

You can import your data from Mixpanel to CleverTap. The script constantly checks your Mixpanel project for new data at an interval of 20 seconds. 

Example Events upload from Mixpanel:

```text
clevertap-data-upload -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX" -mixpanelSecret="<mp api secret>" -t="event" -startDate="<yyyy-mm-dd>" -endDate="<yyyy-mm-dd>"
```

Example Profiles upload from Mixpanel:

```text
clevertap-data-upload -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX" -mixpanelSecret="<mixpanel secret key>"
```
