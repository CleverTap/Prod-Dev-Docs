---
title: How to Create an Apple Push Notification Service Certificate
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
Apple Push Notification service (APNs) is a platform notification service that enables third-party application developers to send notification data to applications installed on Apple devices. iOS applications that use APNs need to have a certificate. This section explains how to generate an APNs certificate for your application.

#Create a Certificate Signing Request (CSR) from Keychain

1. On your MAC open *Keychain Access* and navigate to *Certificate Assistant*.
2. Select *Request a Certificate From a Certificate Authority*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/217563c-cer1.png",
        "cer1.png",
        768,
        480,
        "#e0e0e2"
      ]
    }
  ]
}
[/block]
Selecting this option directs you to the *Certificate Assistant*.

3. Select *Request is* > *Saved to Disk* and leave the email address blank as shown below.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/00bef1c-cer2.png",
        "cer2.png",
        606,
        425,
        "#eae8e8"
      ]
    }
  ]
}
[/block]
4. Click **Continue** to save the file. 

## Download an APNs Certificate from Your Account

1. Log in to [developer.apple.com](https://developer.apple.com/), and navigate to the *Member Center* and select* Certificates, Identifiers & Profiles*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a68a27e-cer3.png",
        "cer3.png",
        768,
        442,
        "#f4f3f4"
      ]
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b2d368f-cer4.png",
        "cer4.png",
        899,
        646,
        "#eaebec"
      ]
    }
  ]
}
[/block]
2. Select *Certificates* under *iOS Apps*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3ac9f6b-cer5.png",
        "cer5.png",
        768,
        514,
        "#ecebeb"
      ]
    }
  ]
}
[/block]
3. Select *Development/Production* under certificates.
4. Click **+** to add a certificate.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/eb674e5-cer6.png",
        "cer6.png",
        768,
        439,
        "#f4f2f1"
      ]
    }
  ]
}
[/block]
5. Select *Apple Push Notification service SSL (Sandbox)*.
6. Click **Continue** and select the *Application ID* for which you want to create the certificate.
7.  Upload the CSR file created on the first step and download the certificate you generated.

## Convert the .cer File to a .p12 Certificate

1. Open the .cer file you downloaded; it will open *Keychain Access*.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cb1478e-cer7.png",
        "cer7.png",
        773,
        487,
        "#edeced"
      ]
    }
  ]
}
[/block]
2. Select your certificate, right-click, and export your certificate in a .p12 format.