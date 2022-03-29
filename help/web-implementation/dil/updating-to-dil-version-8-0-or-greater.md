---
title: 正在更新到Adobe Audience ManagerDIL版本8.0（或更高版本）
description: 本文将为您提供有关将Adobe Audience Manager(AAM)Data Integration Library(DIL)代码更新到版本8.0或更高版本的步骤和建议。 这是指“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，它将涵盖没有Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 1%

---

# 正在更新到Adobe Audience Manager的DIL版本8.0（或更高版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文将为您介绍有关更新Adobe Audience Manager(AAM)的步骤和建议 [!DNL Data Integration Library] (DIL)代码到版本8.0或更高版本。 这是指“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，它将涵盖没有Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。

## 概述 {#overview}

Audience Manager [!DNL Data Integration Library] (DIL)代码允许您在网站上实施AAM*。 在实施DIL的早期版本时，不需要同时实施Adobe的Experience CloudID服务(ECID)（尽管这是一个非常好的主意）。 从DIL版本8.0开始，ECID版本3.3或更高版本存在严重依赖关系。 如果您在未使用ECID 3.3的情况下实施DIL8.0或更高版本，或者您实施的是早期版本，则会收到错误，该错误将无法正常运行。 由于您可以通过多种方式来实施AAM，因此我们创建此页面是为了为您提供一些要完成的步骤以及一些建议。 在下面，您将找到按平台/实施方法划分的这些步骤和建议。 有关DIL的更多信息，请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* 如本页描述中所述，这将仅涵盖“客户端”DIL实施，这些实施由没有Adobe Analytics的AAM客户使用。 如果您拥有Adobe Analytics，则应使用实施AAM的服务器端转发方法。 此方法在 [文档](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## 重复和已弃用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的先前版本中，存在重复的方法(在DIL和ECID中具有相同功能的方法)，这导致对使用哪种方法产生混淆。 通常，您需要同时使用这两个参数并进行匹配，并且该消息无法很好地传达给我们的客户。 从DIL8.0开始，DIL中已弃用这些重复的方法和元素，建议您使用ECID版本。

例如：

* 使用 [!DNL DIL.create]，一些元素已被弃用，您应该改用ECID元素。 这些元素在 [[!DNL DIL.create] 文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* 的 [!DNL idSync] 实例级别方法也已弃用，如方法中所述 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## ID与客户ID同步 {#id-syncing-with-a-customer-id}

在AAM中，您可以将计算机上的UUID（匿名独特用户ID）与客户ID同步，以便您可以上传有关该客户的离线数据，并将其与他们的在线行为绑定，以便更好地了解您的客户。 过去，这是通过以下两种方式之一实现的：

* 的 [!DNL idSync] 实例级方法
* 的 [!DNL declaredId] 元素 [!DNL DIL.create]

如果您之前使用过以下任一旧方法来与客户ID同步，则强烈建议您使用 [!DNL setCustomerIDs] 方法，它是ECID服务的一部分。 有关 [!DNL setCustomerIDs] 在方法的中可用 [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html).

**快速提示：** 以前使用上述任一方法时，您会引用AAM [!UICONTROL Data Source] 和 [!UICONTROL Data Source] ID（即“DPID”）。 更新到 [!DNL setCustomerIDs]，则需要使用AAM [!UICONTROL Data Source]&#39;s &quot;[!UICONTROL Integration Code]”。 这仍然指向了同样的 [!UICONTROL Data Source] 但只是一个不同的标识符。 下面的视频中显示了这一点。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下部分列出了根据您的实施方法更新到DIL8.0的步骤和建议：

## 在Adobe Experience Platform标记中更新到DIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL8.0的基本步骤

1. 如果您使用的是8.0之前的DIL，请在升级之前，转到AAM扩展中的DIL配置，并记下您使用的任何高级选项（将在后续步骤中使用）。
1. 将AAM扩展更新到版本8.0或更高版本。
1. 确认您的Experience CloudID服务扩展的版本为3.3.0或更高版本。
1. 对于任何已弃用的方法/元素(例如 `disableIDSyncs`)或自定义代码中(用于DIL)的ECID方法。

   1. (DIL) `disableDestinationPublishingIframe` ->(ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` ->(ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` ->(ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` ->(ECID) `setCustomerIDs`

1. 发布更改。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在DILDTM中更新到Adobe8.0 {#updating-to-dil-in-adobe-dtm}

1. 将AAM工具更新到版本8.0或更高版本。 此版本设置位于AAM工具的“常规”部分下。
1. 对于任何已弃用的方法/元素(例如 `disableIDSyncs`)中的自定义代码，请记下这些代码（以便您可以将它们添加到ECID工具中），然后从自定义代码中删除它们 [!DNL DIL code] 在AAM工具中。
1. 将您的Experience CloudID服务扩展更新到版本3.3.0或更高版本
1. 将高级选项添加到您从AAM工具的自定义代码中删除的ECID工具中。
1. 发布更改

## 更新到DIL8.0，但无AdobeTag Management解决方案 {#additional-resources}

如果您直接在页面上更新代码，则只需将旧项目替换为较新的项目即可，除非您需要将方法/元素从DIL移动到ECID，如上所述。 在这种情况下，您只需将DIL位置中的旧方法/元素替换为ECID位置中的ECID方法/元素即可。

对于非Adobe标签管理器，情况也是如此。 无论您在该标签管理解决方案中具有哪些旧版本，请按照以下步骤所述，将其替换为新代码。

1. 将您的DIL库更新到最新版本（8.0或更高版本） — 您将需要从Adobe咨询或Adobe客户关怀团队获取最新的DIL代码，因为该代码当前在公共位置不可用。 然后，只需将旧DIL库代码替换为新DIL库代码，然后继续执行下一步（不要立即停止，否则您会遇到问题，哈）。
1. 安装 [!DNL ECID Service] 或将现有版本更新至3.3.0或更高版本。 您可以下载最新的Experience CloudID服务版本 [从我们的GitHub页面](https://github.com/Adobe-Marketing-Cloud/id-service/releases). 如果您需要有关此方面的帮助，请参阅 [文档](https://experienceleague.adobe.com/docs/id-service/using/home.html) 或咨询Adobe顾问。

1. 验证任何已弃用的方法或元素是否已移至ECID方法以进行DIL，请执行以下操作：

   1. (DIL) `disableDestinationPublishingIframe` ->(ECID) `disableIdSyncs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` ->(ECID) `disableIdSyncs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` ->(ECID) `idSyncSSLUseAkamai`

      [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` ->(ECID) `setCustomerIDs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
