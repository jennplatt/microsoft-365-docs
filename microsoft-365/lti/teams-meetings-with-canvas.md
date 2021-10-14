---
title: Use Microsoft Teams meetings with Canvas
ms.author: v-cichur
author: cichur
manager: serdars
ms.reviewer: sovaish
audience: admin
ms.topic: article
ms.service: o365-administration
f1.keywords:
- CSH
ms.collection: M365-modern-desktop
ms.localizationpriority: medium
ROBOTS: NOINDEX, NOFOLLOW
description: "Integrate Microsoft Teams meetings with Canvas"
---


# Use Microsoft Teams meetings with Canvas

Microsoft Teams meetings is a Learning Tools Interoperability (LTI) app that helps educators and students easily navigate between their Learning Management System (LMS) and Teams. Users can access their class teams associated with their course directly from within their LMS.

## Prerequisites before deployment

> [!NOTE]
> The current Teams Meetings LTI only supports syncing Canvas users with Microsoft Azure Active Directory (AAD) in a limited scope. 
> - Your tenant must have an Microsoft Education license.
> - Only a single Microsoft tenant can be used for mapping users between Canvas and Microsoft.
> - If you use SDS to create classes and groups, we recommend disabling the Team Creation Option in SDS and performing a Group Cleanup to avoid duplication of classes. SDS can still be used to sync organization and user data.
> - For recording:
>   -  Microsoft Stream needs to be enabled and have enouf storage for recordings.
>     - Both meeting organizer and person initiating the recording must have a  Office 365 E1, E3, E5, A1, A3, A5, Microsoft 365 Business Premium, Business Standard, or Business 
Basic license.
>   - To record meetings and group calls, **Allow cloud recording** policy needs to be enabled (*CsTeamsMeetingPolicy - AllowCloudRecording* parameter set to true).
>   - To record 1:1 calls, **Cloud recording is enabled for calling** policy to be enabled (*CsTeamsCallingPolicy - AllowCloudRecordingForCalls* parameter set to true).
>   - To enable transcription, **Allow transcription** policy needs to be enabled (*CsTeamsMeetingPolicy - AllowTranscription* parameter set to true).


## Enable the Microsoft Teams app in Canvas

To begin the integration, you need to enable the app in Canvas by enabling the developer keys, enabling the Microsoft Teams Sync, and approving the Microsoft-Teams-Sync-for-Canvas app. Note that approving the app can only be performed by a Microsoft tenant admin that can approve apps.

1. Sign in to Canvas as an administrator.

2. Select the **Admin** link in the global navigation, and then select your account.

3. In the admin navigation, select the **Developer Keys** link, and then select the **Inherited** tab.

4. Enable the LTI apps you are going to deploy by selecting the **ON** state for each of the appropriate apps.

5. In the admin navigation, select the **Settings** link, and then the **Integrations** tab.


![Canvas Teams Sync Updated png.](https://user-images.githubusercontent.com/87142492/128552407-78cb28e9-47cf-4026-954d-12dc3553af6f.png)

6. Fill out the following fields with the appropriate information. These fields will be used for matching users in Canvas with users in AAD. 
   * The **Tenant Name** is your Microsoft tenant name.
   * The **Login Attribute** is one of the following Canvas user attributes used for mapping:
      * **Email** is the Canvas user's default email address. If users change their default email address in Canvas, their enrollment in a course could be blocked from syncing to Teams.
      * **Unique User ID** is the user's Canvas login ID.
      * **SIS User ID** is the ID value that is populated from the Student Information System (SIS) and is viewable on the user's profile page.
      * **Integration ID** is only populated via SIS imports and is viewable on the user's profile page. Typically, this unique identifier is provided by the institution and used in account trusts or consortia situations to identify users across multiple accounts.

   * The **Suffix** field is optional and lets you specify a domain when there isn't an exact mapping between Canvas attributes and Microsoft AAD fields. For example, if your Canvas email is 'name@example.edu' while the UPN in Microsoft AAD is 'name', you can match users by entering '@example.edu' in the suffix field. The domain should be entered in this field with the preceding @.
   * The **Active Directory Lookup Attribute** is the field in AAD to which Canvas attributes are matched. Select UPN, primary email address, or the email alias.

7. Select **Update Settings**.

8. To approve access for Canvas’s **Microsoft-Teams-Sync-for-Canvas** Azure app, select the **Grant tenant access** link. You'll be redirected to the Microsoft Identity Platform Admin Consent Endpoint.

   ![permissions.](media/permissions.png)
> [!NOTE] 
> This step must be performed by a Microsoft tenant admin that can approve apps.
9. Select **Accept**. 

8. Enable the Microsoft Teams Sync by turning the toggle on.

> [!NOTE]
> Microsoft Teams Sync is managed by the Canvas administrator and allows educators to enable the sync at the individual course level using the Microsoft Graph API. When enabled, this sync allows classes to be created in Teams, and the membership is synced based on the enrollment of the course in Canvas.

   ![teams-sync.](media/teams-sync.png)

## Canvas Admin

Set up the Microsoft Teams LTI 1.3 Integration.

As a Canvas Admin, you'll need to add the Microsoft Teams meetings LTI app within your environment. Make a note of the LTI Client ID for the app.

 - Microsoft Teams meetings - 170000000000703

1. Access **Admin settings** > **Apps**.

2. Select **+ App** to add the Teams LTI apps.

   ![external-apps.](media/external-apps.png)

3. Select **By Client ID** for configuration type.

   ![add app.](media/add-app.png)

4. Enter the Client ID provided, and then select **Submit**.

   You'll notice the Microsoft Teams meetings LTI app name for the Client ID for confirmation.

5. Select **Install**.

   The Microsoft Teams meetings LTI app will be added to the list of external apps.

6. Enable the app by navigating to the developer keys in the Canvas admin account, selecting inherited, and turning the toggle "on" for Microsoft Teams Meetings.
   
## Enable for Canvas Courses

In order to use the LTI within a course, an instructor of the Canvas course must enable the integrations sync. Each course must be enabled by an instructor for a corresponding Teams to be created; there is no global mechanism for Teams creation. This is designed out of caution to prevent unwanted Teams being created.

Please refer your instructors to [educator documentation](https://support.microsoft.com/en-us/topic/use-microsoft-teams-classes-in-your-lms-preview-ac6a1e34-32f7-45e6-b83e-094185a1e78a#ID0EBD=Instructure_Canvas) for enabling the LTI for each course and finishing the integration setup.
