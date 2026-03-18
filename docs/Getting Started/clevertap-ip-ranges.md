---
title: CleverTap IP Ranges
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: idc
      title: Configuration Guide for CleverTap Regions
---
# Overview

Secure communication is essential in transferring confidential data in an organization. Using authorized Internet Protocols (IPs) helps send trusted traffic through your network.

CleverTap's IP ranges provide high availability to our customers, and authorizing these IPs avoids service disruptions and provides a seamless user experience.

We recommend you revisit this page regularly to check the IP ranges and addresses that may be added or removed in the future.

# IP Ranges

IP ranges are the source addresses for inbound traffic from CleverTap to the customer's environment (for example, webhooks, API callbacks, exports), these IP ranges are not for making outbound requests from customer's environment to CleverTap.

Starting November 15, 2021, CleverTap uses IP ranges based on the data center regions to avoid service disruptions.

The following table lists the IP ranges for your data center regions to enable seamless and secure communication:

> 📘 IP Whitelisting
>
> Users must whitelist all the IP addresses listed against each region to ensure network connectivity.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Region
      </th>

      <th>
        IP Range
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Europe (`eu1`)
      </td>

      <td>
        `52.209.30.153`  
        `54.72.72.185`  
        `34.249.27.35`  
        `34.250.139.131`  
        `52.211.10.78`  
        `3.251.214.224/27` including `3.251.214.225` and `3.251.214.254 `
      </td>
    </tr>

    <tr>
      <td>
        India (`in1`)
      </td>

      <td>
        `3.109.101.117`  
        `3.109.195.164`  
        `13.126.204.11`  
        `35.154.242.39`  
        `43.205.28.141`  
        `3.109.204.224/27` including `3.109.204.225` and `3.109.204.254`
      </td>
    </tr>

    <tr>
      <td>
        Singapore (`sg1`)
      </td>

      <td>
        `18.136.230.61`  
        `18.138.76.105`  
        `3.1.111.122`  
        `52.221.166.167`  
        `13.214.14.47`  
        `13.214.15.96/27` including `13.214.15.97` and `13.214.15.126`
      </td>
    </tr>

    <tr>
      <td>
        United States (`us1`)
      </td>

      <td>
        `44.232.14.125`  
        `34.214.15.227`  
        `44.233.27.236`  
        `44.232.188.247`  
        `52.10.220.27`  
        `35.86.65.192/27` including `35.86.65.193` and `35.86.65.222`
      </td>
    </tr>

    <tr>
      <td>
        Indonesia (`aps3`)
      </td>

      <td>
        `108.137.145.234`  
        `108.137.16.120`  
        `43.218.28.119`  
        `43.218.2.244`  
        `43.218.27.66`  
        `43.218.56.0/27` including `43.218.56.1` and  `43.218.56.30`
      </td>
    </tr>

    <tr>
      <td>
        UAE (`mec1`)
      </td>

      <td>
        `3.28.244.243`  
        `3.28.170.94`  
        `3.28.217.95`  
        `3.28.209.115`  
        `3.28.22.190`  
        `3.28.255.96/27` including `3.28.255.97` and `3.28.255.126`
      </td>
    </tr>
  </tbody>
</Table>

For more information on data center regions, refer to [Configuration Guide for CleverTap Regions](doc:idc).

# Troubleshooting and FAQs

### Q. Is CleverTap adding new IPs to the existing list?

No. Our IPs range has remained unchanged since November 15, 2021.

### Q. I have already whitelisted all the IPs recommended by CleverTap. Do I need to check again?

CleverTap traffic originates from a combination of fixed IPs and specific IP ranges. While you may have already whitelisted the fixed IPs, the complete range might not be included. Therefore, we recommend rechecking that all required IPs—both fixed and range-based—are correctly whitelisted to maintain uninterrupted functionality and security.
