---
title: How to enable Audit Active Directory objects in Windows Server 2003
description: Describes how to use Windows Server 2003 auditing to track user activities and system-wide events in Active Directory.
ms.date: 09/08/2020
author: Deland-Han
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika
ms.prod-support-area-path: Permissions, access control, and auditing
ms.technology: WindowsSecurity
---
# How to: Audit Active Directory objects in Windows Server 2003

This step-by-step article describes how to use Windows Server 2003 auditing to track user activities and system-wide events in Active Directory.

For a Microsoft Windows 2000 version of this article, see the following Knowledge Base article: [314955](https://support.microsoft.com/help/314955) HOW TO: Audit Active Directory Objects in Windows 2000  

_Original product version:_ &nbsp;Windows Server 2003  
_Original KB number:_ &nbsp;814595

## Summary

When you use Windows Server 2003 auditing, you can track both user activities and Windows Server 2003 activities that are named events, on a computer. When you use auditing, you can specify which events are written to the Security log. For example, the Security log can maintain a record of both valid and invalid logon attempts and events that relate to creating, opening, or deleting files or other objects. An audit entry in the Security log contains the following information:

- The action that was performed.
- The user who performed the action.
- The success or failure of the event and the time that the event occurred.

An audit policy setting defines the categories of events that Windows Server 2003 logs in the Security log on each computer. The Security log makes it possible for you to track the events that you specify.

When you audit Active Directory events, Windows Server 2003 writes an event to the Security log on the domain controller. For example, if a user tries to log on to the domain by using a domain user account and the logon attempt is unsuccessful, the event is recorded on the domain controller and not on the computer where the logon attempt was made. This behavior occurs because it's the domain controller that tried to authenticate the logon attempt but couldn't do so.

Use Event Viewer to view events that Windows Server 2003 logs in the Security log. You can also archive log files to track trends over time. For example, if you want to determine the use of either printers or files, or if you want to verify the use of unauthorized resources.

To enable auditing of Active Directory objects:

- Configure an audit policy setting for a domain controller. When you configure an audit policy setting, you can audit objects but you can't specify the object you want to audit.
- Configure auditing for specific Active Directory objects. After you specify the events to audit for files, folders, printers, and Active Directory objects, Windows Server 2003 tracks and logs these events.

## Configure an Audit Policy Setting for a Domain Controller

By default, auditing is turned off. For domain controllers, an audit policy setting is configured for all domain controllers in the domain. To audit events that occur on domain controllers, configure an audit policy setting that applies to all domain controllers in a non-local Group Policy object (GPO) for the domain. You can access this policy setting through the Domain Controllers organizational unit. To audit user access to Active Directory objects, configure the Audit Directory Service Access event category in the audit policy setting.

### NOTES

- You must grant the Manage Auditing And Security Log user right to the computer where you want to either configure an audit policy setting or review an audit log. By default, Windows Server 2003 grants these rights to the Administrators group.
- The files and folders that you want to audit must be on Microsoft Windows NT file system (NTFS) volumes. To configure an audit policy setting for a domain controller:

1. Click **Start**, point to **Programs**, point to **Administrative Tools**, and then click **Active Directory Users and Computers**.
2. On the **View** menu, click **Advanced Features**.
3. Right-click **Domain Controllers**, and then click **Properties**.
4. Click the **Group Policy** tab, click **Default Domain Controller Policy**, and then click **Edit**.
5. Click **Computer Configuration**, double-click **Windows Settings**, double-click **Security Settings**, double-click **Local Policies**, and then double-click **Audit Policy**.
6. In the right pane, right-click **Audit Directory Services Access**, and then click **Properties**.
7. Click **Define These Policy Settings**, and then click to select one or both of the following check boxes:

    - **Success**: Click to select this check box to audit successful attempts for the event category.
    - **Failure**: Click to select this check box to audit failed attempts for the event category.
8. Right-click any other event category that you want to audit, and then click **Properties**.
9. Click **OK**.
10. Because the changes that you make to your computer's audit policy setting take effect only when the policy setting is propagated or applied to your computer, complete either of the following steps to initiate policy propagation:

    - Type gpupdate /Target:computer at the command prompt, and then press ENTER.
    - Wait for automatic policy propagation that occurs at regular intervals that you can configure. By default, policy propagation occurs every five minutes.
11. Open the Security log to view logged events.

    > [!NOTE]
    > If you are either a domain or an enterprise administrator, you can enable security auditing for workstations, member servers, and domain controllers remotely.

## Configure Auditing for Specific Active Directory Objects

After you configure an audit policy setting, you can configure auditing for specific objects, such as users, computers, organizational units, or groups, by specifying both the types of access and the users whose access that you want to audit. To configure auditing for specific Active Directory objects:

1. Click **Start**, point to **Programs**, point to **Administrative Tools**, and then click **Active Directory Users and Computers**.
2. Make sure that **Advanced Features** is selected on the **View** menu by making sure that the command has a check mark next to it.
3. Right-click the Active Directory object that you want to audit, and then click **Properties**.
4. Click the **Security** tab, and then click **Advanced**.
5. Click the **Auditing** tab, and then click **Add**.
6. Complete one of the following:

    - Type the name of either the user or the group whose access you want to audit in the **Enter the object name to select** box, and then click **OK**.
    - In the list of names, double-click either the user or the group whose access you want to audit.
7. Click to select either the **Successful** check box or the **Failed** check box for the actions that you want to audit, and then click **OK**.
8. Click **OK**, and then click **OK**.

## Troubleshoot

The size of the Security log is limited. Because of this, Microsoft recommends that you carefully select the files and the folders that you want audit. Also consider the amount of disk space that you want to devote to the Security log. The maximum size is defined in Event Viewer.