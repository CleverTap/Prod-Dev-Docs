---
title: Security Overview
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

Protecting customer data is always a priority at CleverTap. Our success as a business relies on the security of customer data stored with us.  As a company, we use the CleverTap platform ourselves for user analytics and engagement. This document mentions steps we take to ensure security, privacy, and confidentiality of customer data.

# Inflight Incoming Data Security

All data collection endpoints support TLS 1.2 encryption protocols with SHA256 with RSA signature algorithm. This allows devices with varied support to access our collection/incoming endpoints while providing the TLS 1.2 to devices that support it.

We maintain a small list of known URLs where we respond to HTTPS requests. All requests to unknown URLs or requests that do not match our expected data format are logged and silently dropped.

The operation team reviews all dropped requests periodically.

# Dashboard Data Security

The dashboard and API endpoints support TLS 1.2 encryption protocols with SHA256 with RSA signature algorithm. Outdated SSL protocols are not supported. We allow dashboard access through modern secure browsers. Developers are required to use updated libraries to access our API endpoints.

# Incoming Request Logging

All incoming requests are logged and stored on persistent storage for analysis and audit. Logs are purged periodically based on industry retention best practices.

# Customer Data Security

Customer data is stored in an encoded format optimized for performance rather than stored in a traditional file system or a database. Data is dispersed across a number of physical and logical volumes for redundancy and expedient access, thereby obfuscating it from tampering.

Customers own all rights to their data and can choose to download it via an API or delete it from our systems. CleverTap provides role-based app­ level access control to customers to manage access to their own data.

CleverTap does not share or sell customer data.

# Running Systems

CleverTap uses battle-tested, open-source software to power some parts of its application stack. We subscribe to CVE vulnerability data and patch critical vulnerabilities within 24 hours of publication. In addition, we destroy and rebuild nodes that power public-facing endpoints every few days. This ensures we don’t have configuration drift in production.

# Data Center Security

Amazon Web Services is our hosting provider. They maintain data­ centers that are fully compliant with a range of certifications that allow finance, healthcare, and government data to be stored in their data­centers. A full list of compliance and more information along with certification is available at [https://aws.amazon.com/compliance/](https://aws.amazon.com/compliance/).

Shared responsibility with Amazon means we focus on application and data security while physical security is managed by them.

# Team

At CleverTap, our engineering and IT teams are experienced and have a know-how of industry best practices on security. Before CleverTap, we have built and run many heavy traffic sites both on physical as well as virtual infrastructure. We bring many years of operational experience running secure and scalable services.

The security and confidentiality of your data is core to our success as a business and we will continue to be proactive, vigilant, and diligent in ensuring its safety.

# FAQ

## What is TLS version 1.1 and why is it being deprecated? What are the potential implications for my users?

TLS 1.0 and TLS 1.1 are out-of-date protocols that do not support modern cryptographic algorithms and can cause security vulnerabilities that may be exploited by attackers. From August 16, 2021, CleverTap will only support TLS version 1.2 and above as it provides enhanced security and improves handshake performance.

In general, most of the devices and web/application servers have already upgraded to the latest TLS 1.2, and do not support TLS 1.1 and below. As a CleverTap customer, you should have almost no visible impact. Based on our analysis, less than 1% of your users might still be on TLS 1.1 and their incoming data will not be accepted by our servers due to the upgrade.

## What do I need to do at my end to be compliant with this change?

You need to upgrade to TLS version 1.2 or higher if you have not already done it. It takes anywhere from a few hours to a couple of days to implement these changes.

## Whom can I contact if I have any questions?

If you notice something unusual in your account, please raise a ticket from the CleverTap dashboard.
