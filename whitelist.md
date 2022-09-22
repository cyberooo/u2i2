---
layout: page
title: Whitelists
---

## Preface

We found 3 whitelisting issues from our tested devices. All the caught whitelisting issues lack a comprehensive caller fingerprint validation mechanism, so that any app with a package name appeared in the whitelist can bypass the permission mechanism and access sensitive user information.

<ol>
<li> In the devices of Huawei (A10), 253 apps are whitelisted
for chip and cellular UUIs (UUIs #1-4). They are from
various categories such as online shopping, online travel
agents, food delivery, and productivity.</li>
<li> In the devices of Oppo (D10,11), 272 apps are whitelisted
for background access to GPS location. They include
leading apps for transportation, food delivery and ridehailing,
and so on. They are all system apps or apps
developed by the manufacturer, such as app store, theme
app, launcher app and mobile wallet. All their package
names are hardcoded in the OS image.</li>
<li> In the devices of Oppo (D10,11) and Vivo (G10), 3 map
apps are whitelisted for WiFi connection details (SSID
and BSSID). Their package names are also hardcoded in
the OS image.</li>
</ol>

## A Case Study

Among the 3 whitelisting issues, the one found in Huawei's device involves the most number of permissions and allows the highest number of apps, too. Therefore, we disclose more details of that issue here.

#### Whitelisting Documents

We search relevant documents on the Internet to support our finding, and we successfully find one webpage hosted in Huawei's developers community website, saying that the whitelisting mechanism is intentionally introduced for bring convenience to device users to avoid too many bothering user-clicking actions to allow certain runtime permissions.  
We reported this issue to Huawei and then received the acknowledgment of the issue. The webpage has been later taken down by Huawei. We keep a copy of the [**webpage snapshot**](/docs/public/huawei-whitelist-documentation-snapshot-20-Jul-2021.pdf){:target="_blank"} that describes the whitelisting mechanism (in Chinese).  

<img src="/docs/public/huawei-whitelist-documentation.jpg" alt="Webpage snapshot in Chinese" width="600"/>

#### Whitelisting Implementation

We dump the image of our invloved tested devices and analyze how the whitelisting mechanism is implemented.  
We find the source of whitelisting mechanism is in a class named *TelephonyPermissions*, in which a vendor modification over the original AOSP is made. As the result, all the permission checking against caller's identity is compromised as long as the caller's package name appears in the pre-defined whitelist.  
We present the technical details in [**slides**](/docs/public/technical-demo.pdf){:target="_blank"} attached on this site. 

<img src="/docs/public/technical-demo.jpg" alt="Demo slides of how the whitelisting mechanism is implemented" width="600"/>

#### Whitelisting Apps

From the dumped image we also perform a text search and eventually find the list of *whitelisted apps* from a XML resource file.  
Within the XML file, there are lists of apps' package names defined in a *string-array* data structure. We find there are *162*  and *253* apps are defined as *"system apps"* and *"whitelisted app"*, respectively.  
We attach the partial content of the XML file on this site, you can click [**here**](/docs/public/whitelisted-app.xml){:target="_blank"} to download it.

<img src="/docs/public/whitelisted-app.jpg" alt="The XML file contains the whitelisted apps' package names" width="600"/>

<p class="message">
<b>Claim of Responsible Disclose</b>
<br>
All the three whitelisting issues, including the one shown in the case study above, have been acknowledged and fixed by the OEMs. At the moment of this was disclosed, all the devices of involved OEMs that have installed the latest OS and security patches are free from the whitelisting risk.  
</p>