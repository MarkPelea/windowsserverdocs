---
title: Create a Rule for Packaged Apps
description: "Windows Server Security"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-applocker
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ccde65f-9153-4c29-aa24-deaa6b33776e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
---
# Create a Rule for Packaged Apps

>Applies To: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

This topic shows how to create an AppLocker rule for packaged apps with a publisher condition in Windows Server 2012 and Windows 8.

Packaged apps (also known as Windows 8 apps) are new to Windows Server 2012 and Windows 8. They are based on the new app model that ensures that all the files within an app package share the same identity. Therefore, it is possible to control the entire application using a single AppLocker rule as opposed to the non-packaged apps where each file within the app could have a unique identity. Windows does not support unsigned packaged apps which implies all packaged apps must be signed. AppLocker supports only publisher rules for Packaged apps. A publisher rule for a Packaged app is based on the following information:

-   Publisher of the package

-   Package name

-   Package version

All the files within a package as well as the package installer share these attributes. Therefore, an AppLocker rule for a Packaged app controls both the installation as well as the running of the app. Otherwise, the publisher rules for Packaged apps are no different than the rest of the rule collections; they support exceptions, can be increased or decreased in scope, and can be assigned to users and groups.

For information about the publisher condition, see [Understanding the Publisher Rule Condition in AppLocker](../../get-started/how-applocker-works/understanding-the-publisher-rule-condition-in-applocker.md).

You can perform this task by using the Group Policy Management Console for an AppLocker policy in a Group Policy Object (GPO) or by using the Local Security Policy snap-in for an AppLocker policy on a local computer or in a security template. For information how to use these MMC snap-ins to administer AppLocker, see [Using the MMC snap-ins to administer AppLocker](../administer-applocker.md#BKMK_Using_Snapins).

### <a name="BKMK_CreatePubRuleGPO"></a>To create a Packaged app rule

1.  In the console tree of the snap-in, double-click **Application Control Policies**, double-click **AppLocker**, and then click **Packaged app Rules**.

2.  On the **Action** menu, or by right-clicking on **Packaged app Rules**, click **Create New Rule**.

3.  On the **Before You Begin** page, click **Next**.

4.  On the **Permissions** page, select the action (allow or deny) and the user or group that the rule should apply to, and then click **Next**.

5.  On the **Publisher** page, you can select a specific reference for the Packaged app rule and set the scope for the rule. The following table describes the reference options.

    |Selection|Description|Example|
    |-------|--------|------|
    |**Use an installed packaged app as a reference**|If selected, AppLocker requires you to choose an app that is already installed on which to base your new rule. AppLocker uses the publisher, package name and package version to define the rule.|You want the Sales group only to use the app named Microsoft.BingMaps for its outside sales calls. The Microsoft.BingMaps app is already installed on the computer where you are creating the rule, so you choose this option, and select the app from the list of apps installed on the computer and create the rule using this app as a reference.|
    |**Use a packaged app installer as a reference**|If selected, AppLocker requires you to choose an app installer on which to base your new rule. A packaged app installer has the .appx extension. AppLocker uses the publisher, package name and package version of the installer to define the rule.|Your company has developed a number of internal LoB packaged apps. The app installers are stored on a common file share. Employees can install the required apps from that file share. You want to allow all your employees to install the Payroll app from this share. So you choose this option from the wizard, browse to the file share and choose the installer for the Payroll app as a reference to create your rule.|

    The following table describes setting the scope for the Package app rule.

    |Selection|Description|Example|
    |-------|--------|------|
    |Applies to **Any publisher**|This is the least restrictive scope condition for an **Allow** rule. It permits every packaged app to run or install.<br /><br />Conversely, if this is a **Deny** rule, then this option is the most restrictive because it denies all apps from installing or running.|You want the Sales group to use any packaged app from any signed publisher. You set the Permissions to allow Salesgroup to be able to run any app.|
    |Applies to a specific **Publisher**|This scopes the rule to all apps published by a particular publisher.|You want to allow all your users to install apps published by the publisher of Microsoft.BingMaps. You could select Microsoft.BingMaps as a reference and choose this rule scope.|
    |Applies to a **Package name**|This scopes the rule to all packages that share the Publisher name and Package name as the reference file.|You want to allow your Sales group to install any version of the Microsoft.BingMaps app. You could select the Microsoft.BingMaps app as a reference and choose this rule scope.|
    |Applies to a **Package version**|This scopes the rule to a particular version of the Package.|You want to be very selective in what you allow. You do not want to implicitly trust all future updates of the Microsoft.BingMaps app. You can limit the scope of your rule to the version of the app currently installed on your reference computer.|
    |Applying custom values to the rule|Selecting Use custom values allows you to adjust the scope fields for your particular circumstance.|You want to allow users to install all Microsoft.Bing\* applications which include Microsoft.BingMaps, Microsoft.BingWeather, Microsoft.BingMoney. You can choose the Microsoft.BingMaps as a reference, choose the **Use custom values** scope option and manually edit the Package name field by adding ???Microsoft.Bing*??? as the Package name.|

6.  Click **Next**.

7.  (Optional) On the **Exceptions** page, specify conditions by which to exclude files from being affected by the rule. This allows you to add exceptions based on the same rule reference and rule scope as you set before. Click **Next**.

8.  On the **Name and Description** page, either accept the automatically generated rule name or type a new rule name, and then click **Create**.

