---
title: How to Create an iOS APNs Certificate
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
Every iOS application that wants to use Apple Push Notification Services needs to have an APNS certificate. In this article we will show you how to generate an APNS certificate for your application.

# Step 1: Creating a CSR from Your Keychain

On your MAC open Keychain Access, navigate to Certificate Assistant and select Request a Certificate From a Certificate Authority.
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
On selecting this option you will be directed to the Certificate Assistant, here you have to select Request is → Saved to Disk and let the CA email Address remain blank as shown below.
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
Click on continue and save the file. Your CSR is now ready!

# Step 2: Downloading an APNS Certificate from Your Account

Log in to [developer.apple.com](https://developer.apple.com/), and navigate to the Member Center and select Certificates, Identifiers & Profiles.
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
Select Certificates under your iOS Apps.

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
Choose Development/Production under certificates, depending on which one you want to generate and then click on “+” button to add a certificate.
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
Select Apple Push Notification service SSL (Sandbox), click on continue and select the Application ID for which you want to create the Certificate.

Then you have to upload the CSR file we created on the first step and finally we can download the certificate that has been generated.

# Step 3: Converting the .cer File to a .p12 Certificate

Open the .cer certificate file that you just downloaded, it will open Keychain Access.
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
Select your certificate, right click and choose to export your certificate in a .p12 format. Once you have this file, you’re good to go!