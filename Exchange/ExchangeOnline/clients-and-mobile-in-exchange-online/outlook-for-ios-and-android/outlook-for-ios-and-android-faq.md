---
localization_priority: Normal
description: 'Summary: This article covers the most common questions asked by customers and administrators about using Outlook for iOS and Android with Exchange Online and Office 365.'
ms.topic: conceptual
author: msdmaguire
ms.author: dmaguire
ms.assetid: 747d4875-4b81-4b10-a206-fc2cbab83314
ms.date: 
title: Outlook Exchange Online FAQ, Outlook for iOS Exchange FAQ, Outlook for Android FAQ, Outlook for iOS security concerns, Outlook for Android security concerns, Outlook for iOS cloud architecture, Outlook for Android cloud architecture, Outlook app Office 365 architecture, Outlook app attachment faq, Outlook app contacts faq, Outlook app license faq
ms.collection: 
- exchange-online
- M365-email-calendar
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
manager: dansimp

---

# Outlook for iOS and Android in Exchange Online: FAQ

 **Summary**: This article covers the most common questions asked by customers and administrators about using Outlook for iOS and Android with Exchange Online and Office 365.

The Outlook for iOS and Android app is designed to enable users in your organization to do more from their mobile devices, by bringing together email, calendar, contacts, and other files. The following sections highlight the most common questions we receive, across three key areas:

- Outlook for iOS and Android architecture and security

- Managing and maintaining Outlook for iOS and Android in your Exchange organization after it has been deployed

- Common questions from end-users who access information in your Exchange organization with the Outlook for iOS and Android app on their mobile devices

## Architecture and security

The following questions are about the overall architecture of Outlook for iOS and Android in Exchange Online, as well as user authentication and other security concerns.

### Q: What cloud architecture is utilized by Outlook for iOS and Android for Office 365 accounts?

For more information on the architecture, see [Outlook for iOS and Android in Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/outlook-for-ios-and-android).

### Q: Can I add two different Office 365 accounts from different Office 365 regions to Outlook for iOS and Android?

Yes, provided both accounts do not have Intune App Protection Policies assigned. However, for Government Community Cloud customers, users may only add their own account and OneDrive for Business storage account to the app; adding personal or other commercial accounts is prevented to meet FedRAMP requirements. For more information on Government Community Cloud restrictions with Outlook for iOS and Android, please see [Using Outlook for iOS and Android in the Government Community Cloud](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/outlook-for-ios-and-android-in-the-government-cloud).

### Q: What authentication mechanism is used for Outlook for iOS are Android? Are credentials stored in Office 365?

Active Directory Authentication Library (ADAL)-based authentication is what Outlook for iOS and Android uses to access Exchange Online mailboxes in Office 365. ADAL authentication, used by Office apps on both desktop and mobile devices, involves users signing in directly to Azure Active Directory, which is Office 365's identity provider, instead of providing credentials to Outlook.

ADAL-based sign in enables OAuth for Office 365 accounts, and provides Outlook for iOS and Android a secure mechanism to access email without requiring access to user credentials. At sign in, the user authenticates directly with Office 365 and receives an access token in return. The token grants Outlook for iOS and Android access to the appropriate mailbox. OAuth provides Outlook with a secure mechanism to access Office 365 and the Outlook cloud service without needing or storing a user's credentials.

For more information, see the Office Blog post [New access and security controls for Outlook for iOS and Android](https://go.microsoft.com/fwlink/p/?LinkId=623595).

### Q: Do Outlook for iOS and Android and other Microsoft Office mobile apps support single sign-on?

All Microsoft apps that leverage the Azure Active Directory Authentication Library (ADAL) support single sign-on. In addition, single sign-on is also supported when the apps are used in conjunction with either the Microsoft Authenticator or Microsoft Company Portal apps.

Tokens can be shared and re-used by other Microsoft apps (such as Word mobile) under the following scenarios:

1. When the apps are signed by the same signing certificate and use the same service endpoint or audience URL (such as the Office 365 URL). In this case, the token is stored in app shared storage.

2. When the apps leverage or support single sign-on with a broker app. The tokens are stored within the broker app. Microsoft Authenticator is an example of a broker app. In the broker app scenario, after you attempt to sign in to Outlook for iOS and Android, ADAL will launch the Microsoft Authenticator app, which will make a connection to Azure Active Directory to obtain the token. It will then hold on to the token and re-use it for authentication requests from other apps, for as long as the configured token lifetime allows.

For more information, see [How to enable cross-app SSO on iOS using ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

### Q: What is the lifetime of the tokens generated and used by the Active Directory Authentication Library (ADAL) in Outlook for iOS and Android?

Two tokens are generated when a user authenticates through ADAL-enabled apps like Outlook for iOS and Android, the Authenticator app, or the Company Portal app: an access token and a refresh token. The access token is used to access the resource (Exchange message data), while a refresh token is used to obtain a new access or refresh token pair when the current access token expires.

By default, the access token lifetime is one hour and the refresh token lifetime is 90 days. These values can be adjusted; for more information see [Configurable token lifetimes in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes). Note that if you choose to reduce these lifetimes, you can also reduce the performance of Outlook for iOS and Android, because a smaller lifetime increases the number of times the application must acquire a fresh access token.

### Q: What happens to the access token when a user's password is changed?

A previously granted access token is valid until it expires. Upon expiration, the client will attempt to use the refresh token to obtain a new access token, but because the user's password has changed, the refresh token will be invalidated (assuming directory synchronization has occurred between on-premises and Azure Active Directory). The invalidated refresh token will force the user to re-authenticate in order to obtain a new access token and refresh token pair.

### Q: Does Outlook for iOS and Android support certificate-based authentication?

Yes, Outlook for iOS and Android supports certificate-based authentication for modern authentication-enabled accounts (Office 365 accounts or [on-premises accounts leveraging hybrid modern authentication](http://aka.ms/hmaom)). For more information, see:

- [Configuring Active Directory Federation Services (ADFS) with Office 365](https://go.microsoft.com/fwlink/p/?linkid=849806)

- [Certificate-based authentication on iOS](https://go.microsoft.com/fwlink/p/?linkid=849807)

- [Certificate-based authentication on Android](https://go.microsoft.com/fwlink/p/?linkid=849808)

### Q: What does background synchronization enable? I notice that when I launch the app with it enabled, I still have to wait for messages to download, even after I've received new mail notifications for them; and sometimes, I get reminders for appointments that had been cancelled.

Background synchronization enables new message notifications, calendar reminders, badge count updates, and background synchronization of mailbox and calendar information for Outlook for iOS and Android.

If background synchronization is disabled by the user in the mobile operating system's settings, then the user must launch the app and keep it in the foreground in order to synchronize messages and have an up-to-date calendar.

Background synchronization in Outlook for iOS and Android can also be temporarily disabled by the following actions:

- Force quitting Outlook for iOS.

- Restarting the iOS device.

- Outlook for iOS crashes and is not restarted by the user.

- Not opening the app for a given period of time. iOS will [automatically freeze third-party apps](https://developer.apple.com/library/archive/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html), like Outlook, based on usage patterns. Android [doze mode and app standby](https://developer.android.com/training/monitoring-device-state/doze-standby) features can also prevent background updates to the app while those features are active.

- On some Android devices, you can also restrict background processing or network access per-app. In these cases, Outlook for Android will not be able to process updates in the background. Android device manufacturers can modify the way you can interact with settings, therefore it is not possible to document every device scenario, but in general, these are the steps you can take to remove battery optimization:

 1. Open **Settings**.
 2. Tap **Battery**.
 3. Tap the ellipse and tap **Battery optimization**.
 4. Tap the down arrow and tap **All apps**.
 5. For the Microsoft Authenticator, Intune Company Portal and Outlook apps, tap **Not optimized** to turn off battery optimization.

If the mobile operating system prevents background synchronization, users will experience the following:

- New mail notifications will continue to be delivered, however, upon launching the app, the new messages will have to be downloaded.

- Calendar reminders will fire for appointments that have been cancelled because the app was unable to download and process the meeting cancellation.

> [!NOTE]
> Apple allows its native Mail and Calendar apps to do background refreshes without any restrictions. Therefore, users may notice a difference in the background synchronization experience between the apps. However, this also results in improved battery life and less data consumption with Outlook for iOS.

### Q: Does each user's instance of Outlook for iOS and Android have a unique device ID in the Office 365-based architecture? How is the device ID generated and is this same device ID used in Intune?

Upon initial account login, Outlook for iOS and Android establishes a connection to the Office 365-based architecture. A unique device ID is generated, and this device ID is what appears in Active Directory device records (which can be retrieved with cmdlets such as `Get-MobileDevice` in Exchange Online Powershell) and which appears in HTTP request headers.

Intune uses a different device ID. The basic workflow for how Intune assigns a device ID is described in [App-based conditional access with Intune](https://docs.microsoft.com/intune/deploy-use/restrict-access-to-email-and-o365-services-with-microsoft-intune). In Intune, the device ID is assigned when the device workplace joins for all device-conditional access scenarios. This is an AAD-generated unique ID for the device. Intune uses that unique ID when sending compliance information, and ADAL uses that unique ID when authenticating to services.

### Q: Does Outlook for iOS and Android support RMS?

Yes. Outlook for iOS and Android supports reading protected messages. Outlook for iOS and Android works differently than desktop versions of Outlook when it comes to RMS. For desktop versions of Outlook, once a protected message is received and access is attempted, and Outlook verifies that the user can read RM messages, Outlook connects to Exchange to request an encryption key. The Outlook desktop client uses that encryption key to decrypt the message in front of the user (client-side). Mobile clients operate differently. When Outlook for iOS and Android sets up its initial relationship with Exchange, it notifies Exchange that it supports RMS. Exchange decrypts any protected messages before passing them to the client. In other words, decryption is performed server-side. Outlook for iOS and Android doesn't perform any decryption itself.

In cases where Outlook for iOS and Android receives protected messages and prompts end-users to use an RM client to open the file, it means that Exchange hasn't decrypted the message, which is due to an issue on the Exchange side.

> [!NOTE]
> Outlook for iOS leverages iOS's native preview technology to quickly expose attachments to end users. iOS's preview technology does not support rights management and will report error "The operation couldn't be completed. (OfficeImportErrorDomain error 912)" when a user attempts to open a rights-protected attachment. Users will need to tap the respective Word, Excel, or PowerPoint app icon to open the rights-protected attachment in the native app.

### Q: Does Outlook for iOS and Android support Teams meetings?

Yes, Outlook for iOS and Android supports both Skype for Business and Teams meetings. The Teams coexistence mode at the Office 365 tenant level and the user level (the user setting takes precedence over the tenant setting) determines the meeting creation experience in Outlook for iOS and Android:

<table>
<thead>
<tr class="header">
<th><strong>Coexistence Mode</strong></th>
<th><strong>Outlook for iOS and Android experience</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Islands</td>
<td>Skype for Business</td>
</tr>
<tr class="even">
<td>Skype for Business Only</td>
<td>Skype for Business</td>
</tr>
<tr class="odd">
<td>Skype for Business with Teams Collaboration</td>
<td>Skype for Business</td>
</tr>
<tr class="even">
<td>Teams Only</td>
<td>Teams</td>
</tr>
<tr class="odd">
<td>Skype for Business with Teams Collaboration and Meetings</td>
<td>Teams</td>
</tr>
</tbody>
</table>

In addition, for users leveraging the native Microsoft sync technology, a Teams Join button is available in calendar events. This makes it easy to Join a Teams meeting and will be available for all coexistence modes. Users who are not leveraging the native Microsoft sync technology will be able to join Teams Meetings using the weblink in the meeting description.

For more information on the Teams coexistence modes, please see [Choose your upgrade journey from Skype from Business to Teams](https://docs.microsoft.com/microsoftteams/upgrade-and-coexistence-of-skypeforbusiness-and-teams).

### Q: What ports and end points does Outlook for iOS and Android use?

Outlook for iOS and Android communicates via TCP port 443. The app accesses various end points, depending on the activities of the user. Complete information is available in [Office 365 URLs and IP address ranges](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges).

### Q: Does Outlook for iOS and Android support proxy configurations?

Yes, Outlook for iOS and Android supports proxy configurations when the proxy infrastructure meets the following requirements:

- **Supports HTTP protocol without TLS decryption and inspection**.

- **Does not perform authentication**.

Outlook for iOS and Android will consume the proxy configuration as defined by the platform operating system. Typically, this configuration information is deployed via a PAC file. The PAC file must be configured to use hostnames instead of protocol; no additional custom settings are supported. For a list of hostnames that Outlook for iOS and Android accesses, please see [Office 365 URLs and IP address ranges](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges).

For tenants that have not been migrated to the native Microsoft sync technology, the following additional requirement applies:

- **Supports and has SOCKS proxy capability enabled**. The Outlook for iOS and Android client utilizes TCP connections to our Office 365-based architecture. The IP ranges for the SOCKS connections are not restricted to a subset of Azure IP ranges, which means that customers cannot define a whitelist range. The PAC must be configured to use hostnames instead of protocol and return the SOCKS proxy information given the host URL; no additional custom settings are supported.

## Native Microsoft sync technology migration

The following questions are about the migration from the REST API data sync protocol to the native Microsoft sync technology used by Outlook for iOS and Android for accessing mailbox data.

### Q: Is there a minimum version of Outlook for iOS and Android required to use the native Microsoft sync technology?

For Outlook for iOS, users should install 3.10.1 or later. For Outlook for Android, users should install 3.0.14 or later. As always, we recommend users keep the Outlook app up to date.

### Q: What will my users experience when our tenant is migrated to the native Microsoft sync technology?

Assuming the user is running a supported version of Outlook for iOS and Android, after your tenant is migrated, your users may see a brief notice indicating that we are updating their email and calendar data. Otherwise the user experience to migrate to the updated architecture will be seamless.

### Q: As a tenant administrator, can I control which of my users will be migrated to the native Microsoft sync technology?

No, the migration to the native Microsoft sync technology will be on a tenant-by-tenant basis and not a per-user basis. While the tenant selection order for migration is random, we are being deliberate about migrating Office 365 mailboxes first before we migrate on-premises mailbox accounts. If you are a customer operating in a hybrid configuration where a portion of your mailboxes remain on-premises, the on-premises users leveraging [hybrid modern authentication](https://docs.microsoft.com/Exchange/clients/outlook-for-ios-and-android/use-hybrid-modern-auth) will be migrated to the native Microsoft sync technology at a later date. This means that your Office 365 users will migrate to the native Microsoft sync technology, while the on-premises users continue to use the REST API to connect to Exchange Online.

Once your tenant is migrated, a user will not switch to the native Microsoft sync technology, until after they launch/resume Outlook for iOS and Android.

### Q: If my user doesn't upgrade to a supported build of Outlook for iOS and Android prior to my tenant's migration, does that mean the user will lose access to email and calendar data while mobile?

No, the user will continue to connect using the existing REST-based data sync protocol.

### Q: Will my Intune App Protection Policies or Azure AD Conditional Access policies be affected by this migration?

No, both Intune App Protection Policies and Azure AD Conditional Access policies will continue to be applied to the targeted identity, regardless of the data sync protocol leveraged by Outlook for iOS and Android.

### Q: Will I have to update my Exchange mobile device access policies (allow block quarantine (ABQ) rules)?

No, the user agent string that Outlook for iOS and Android uses does not change. For more information on what that user agent is, see [Securing Outlook for iOS and Android in Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/secure-outlook-for-ios-and-android).

### Q: As an Exchange administrator, is there a way for me to determine which data sync protocol Outlook for iOS and Android clients are utilizing in the Office 365-based architecture?

Yes, execute the following command from Exchange Online PowerShell:

```
Get-MobileDevice | where {$_.DeviceModel -eq "Outlook for iOS and Android"} | Format-List FriendlyName,DeviceID,DeviceOS,ClientType
```

The `ClientType` property indicates which data sync protocol is in use. If the value is REST, then the client is utilizing the REST API. If the value is Outlook, then the client is using the native Microsoft sync technology.

Alternatively, a user can login to Outlook on the web and, from within **Options**, select **Mobile Devices** to view the details of a mobile device. Like the cmdlet, the user can see the value for the `ClientType` property.

## Administrating and monitoring Outlook for iOS and Android in your organization

The following questions are about managing and monitoring the Outlook for iOS and Android app within your organization after the app has been deployed.

### Q: Is it necessary to file an in-app support ticket when I experience an issue with Outlook for iOS and Android?

Yes, if you want to troubleshoot and resolve the issue, or if you want to inform us of a product defect or limitation, you will need to file an in-app support ticket. Only through filing an in-app support ticket can the Outlook app's logs get collected and analyzed by our product engineers.

Customers with a Microsoft Premier agreement can open support cases with Customer Service & Support (CSS). Instead of having the user initiate an in-app support ticket, the user can leverage Collect Diagnostics to upload the logs and share the incident ID with CSS/Premier. Collect Diagnostics will capture data from Outlook for iOS and Android, Authenticator, and the Company Portal and upload all the relevant logs to Microsoft. Microsoft Support Escalation Engineers can use the incident ID to access the diagnostic logs and troubleshoot the user's issue.

To gather the logs:

1. Within Outlook for iOS and Android's settings, tap Help & Feedback.

2. Tap Collect Diagnostics.

3. Tap Get Started.

4. Tap Upload Outlook Logs (iOS) or Collect Logs (Android).

5. Share the incident ID with CSS.

### Q: As an Exchange administrator, I would like to deploy Outlook for iOS and Android, but in my testing I can't log in. What might be the issue?

Assuming authentication is not the issue, there are two areas you can check:

1. Check whether you have an EWS application policy that restricts which client applications can connect.

2. Check whether you have EWS enabled for the account.

For more information, see [Securing Outlook for iOS and Android in Exchange Online](secure-outlook-for-ios-and-android.md). If one of the above checks doesn't resolve the issue, please open an in-app support ticket.

### Q: Will Outlook for iOS and Android support third-party EMM or MDM solutions?

For more information, please see [Managing Outlook for iOS and Android in Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/outlook-for-ios-and-android/manage-outlook-for-ios-and-android).

### Q: Is a license required to use Outlook for iOS and Android?

Outlook for iOS and Android is free for consumer usage from the iOS App store and from Google Play. However, commercial users require an Office 365 subscription that includes the Office desktop applications: Business, Business Premium, Enterprise E3, E5, and ProPlus, or the corresponding versions of those plans for Government or Education. Commercial users with the following subscriptions are allowed to use the Outlook mobile app on devices with integrated screens 10.1" diagonally or less: Office 365 Enterprise E1, Office 365 F1, Office 365 Business Essentials, Office 365 A1, and if you only have an Exchange Online license (without Office). If you only have an Exchange on-premises (Exchange Server) license, you are not licensed to use the app.

## Common questions from end-users

The following questions concern end-users in your organization who are using Outlook for iOS and Android on their devices to access their Exchange mailboxes.

### Q: My users enabled the "Save Contacts" advanced settings option. However, they are complaining that not all contacts have synchronized on their iOS devices. Are there limitations with synchronization?

The initial export of contacts can only begin when Outlook is in the foreground. A user can switch between apps and the export will continue while Outlook is active in memory. There are iOS limitations when syncing with iCloud that may result in data inconsistency, but Outlook will automatically trigger a reconciliation to ensure that the contacts are always consistently exported (e.g., reconciliation will remove duplicates in the event that Outlook detects exported contacts from a previous export activity). In the event you are seeing an inconsistency and it has not been resolved after a short period of time, wait twenty-four hours and then restart the app to trigger the reconciliation process..

### Q: Why are the Office mobile apps required to be installed on Android in order to render attachments in Outlook, while iOS devices provide a preview of the attachments within Outlook?

This is due to the differences in the base operating systems. iOS provides native content rendering for known attachment types, which Outlook for iOS uses to provide basic attachment rendering. Android provides nothing similar. Android users have to install the Office apps and/or third-party apps in order to render attachment content.

### Q: A new message included an attachment, but while I was offline I couldn't open the attachment. Why is that?

Outlook (like other mobile clients) does not download attachments automatically. This is by design, in order to conserve device space. Attachments are only downloaded at the request of the user.

### Q: A week ago I accessed an attachment in a message, but now that I'm offline I can no longer access that attachment on my iOS device. However, I can access it on my Android device. Why is that?

Outlook for iOS stores attachments in our own database. As a result, every attachment we download to the client takes up a considerable amount of space in our database. To ensure the client is able to provide fast performance and take a small amount of space, we purge data rather aggressively based on usage (attachments will be cached up to seven days).

Unlike iOS, Android uses an accessible file system, so when Outlook for Android downloads an attachment, it doesn't go into the database, rather it is stored as a temporary file.

### Q: Why does data within Outlook for iOS disappear and then re-appear after I toggle the Focused Inbox or the Organize by Thread settings?

Whenever those options are changed, Outlook for iOS performs a soft reset. This wipes the existing data that has been downloaded to the app and requires a re-synchronization.

### Q: Can I view organization chart information in Outlook for iOS?

Yes. Outlook for iOS provides your company's organization information as part of a person's contact card details. Your company's reporting structure and a list of colleagues is also provided, to help employees connect with the people and teams they need to work with.

The list of people displayed as part of the Other Colleagues list under **Show Organization** is based on common email distribution lists, group memberships, and degrees of separation in the Organization structure defined in Azure Active Directory.

If you do not have organization chart data exposed in the app, consult with your directory administrator. There are two main scenarios to consider:

1. Your company has a hybrid topology where an on-premises directory is synchronized with Azure Active Directory. You will need to update Active Directory with the organization chart information, either directly in the directory or via your Human Resources system. Data will be synchronized into AAD automatically and will be accessible via the Global Address List in Exchange Online.

2. Your company only leverages Azure Active Directory for directory management. You will need to update Azure Active Directory with the organization chart information, either directly in the directory or via your Human Resources system. This data will be accessible via the Global Address List in Exchange Online.

### Q: How much of my mailbox data is synchronized with Outlook for iOS and Android?

Outlook for iOS and Android synchronizes 500 items per folder, with up to 1000 items per folder if the user taps **Load more conversations**. The app periodically trims the items per folder down to 500, in order to ensure optimal app performance.

### Q: Why are tasks and notes not available with Outlook for iOS and Android?

Microsoft's strategic direction for task management and note taking on mobile devices is the To-Do and OneNote apps, respectively. To-Do provides integration with the tasks stored in Exchange Online mailboxes.
