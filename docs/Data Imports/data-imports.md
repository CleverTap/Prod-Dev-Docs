---
title: Data Imports
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

The CleverTap enables you to import data such as events and profiles from third parties to the CleverTap platform. 

## Data Import

To perform a data import, complete the following steps:

1. Get the artifacts for data upload from the [CleverTap Repository](https://github.com/CleverTap/clevertap-data-upload). 

> 📘 Note
>
> The CleverTap script runs in `Go`. Check that you have installed `Go` on your system.  To download and install go, see [https://golang.org/dl/](https://golang.org/dl/).

2. Install the CleverTap upload tool. 

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

3. Upload your data to CleverTap.

## From Mixpanel

You can import your data from Mixpanel directly to CleverTap. The script constantly checks your Mixpanel project for new data at an interval of 20 seconds. 

Example Events upload from Mixpanel:

```text
clevertap-data-upload -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX" -mixpanelSecret="<mp api secret>" -t="event" -startDate="<yyyy-mm-dd>" -endDate="<yyyy-mm-dd>"
```

Example Profiles upload from Mixpanel:

```text
clevertap-data-upload -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX" -mixpanelSecret="<mixpanel secret key>"
```

## From other platforms such as Amplitude or any analytics tool

You can import data from any provider to CleverTap. You must first save the data from your platform in a CSV. You can then upload this CSV to CleverTap.\
Example Events upload from CSV:

```text
clevertap-data-upload -csv="/Users/name/Documents/in.csv" -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX" -t="event" -evtName="Product Viewed"
```

Example Profiles upload from CSV:

```text
clevertap-data-upload -csv="/Users/name/Documents/in.csv" -id="XXX-XXX-XXXX" -p="XXX-XXX-XXXX"
```

> 📘 Note
>
> For CSV uploads, you must include one of identity, objectId, FBID or GPID, in your data. Email addresses can serve as an identity value, but the key must be identity.
