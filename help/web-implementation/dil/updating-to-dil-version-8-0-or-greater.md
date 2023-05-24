---
title: 正在更新到Adobe Audience ManagerDIL版本8.0（或更高版本）
description: 本文将为您提供有关将Adobe Audience Manager (AAM)Data Integration Library(DIL)代码更新到版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，将涵盖不使用Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。
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

本文将提供有关更新Adobe Audience Manager (AAM)的步骤和建议 [!DNL Data Integration Library] (DIL)版本8.0或更高版本的代码。 这指的是“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，将涵盖不使用Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。

## 概述 {#overview}

Audience Manager [!DNL Data Integration Library] (DIL)代码允许您在网站上实施AAM*。 在实施以前版本的DIL时，不需要同时实施Adobe的Experience CloudID服务(ECID)（尽管这是个非常好的主意）。 从DIL版本8.0开始，硬依赖于ECID版本3.3或更高版本。 如果您在不使用ECID 3.3的情况下实施DIL8.0或更高版本，或者实施了早期版本，则会收到错误，并且无法正常运行。 由于有多种方法可以实施AAM，因此我们创建了此页面，为您提供了一些要完成的步骤以及一些建议。 在下面，您将看到按平台/实施方法划分的这些步骤和建议。 有关DIL的更多信息，请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* 如本页描述中所述，这仅涵盖“客户端”DIL实施，由不具有Adobe Analytics的AAM客户使用。 如果您有Adobe Analytics，则应该使用实施AAM的服务器端转发方法。 有关该方法的描述，请参见 [文档](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## 重复和已弃用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的早期版本中，存在重复方法(在DIL和ECID中执行相同功能的方法)，这会导致在使用哪种方法时产生混淆。 通常情况下，您需要同时使用这两种工具并将它们匹配起来，而该消息没有很好地传达给我们的客户。 从DIL8.0开始，DIL中已弃用这些重复的方法和元素，建议您使用ECID版本。

例如：

* 使用时 [!DNL DIL.create]，一些元素已被弃用，您应该改用ECID元素。 这些元素在 [[!DNL DIL.create] 文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* 此 [!DNL idSync] 实例级别的方法也已弃用，如该方法的 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## 与客户ID同步的ID {#id-syncing-with-a-customer-id}

在AAM中，您可以将计算机上的UUID（匿名唯一用户ID）与客户ID同步，以便您可以上传有关该客户的离线数据，并将其与其在线行为相关联，从而更好地了解您的客户。 在过去，这是以两种方式之一完成的：

* 此 [!DNL idSync] 实例级方法
* 此 [!DNL declaredId] 中的元素 [!DNL DIL.create]

如果您一直使用其中一种旧方法同步客户ID，强烈建议您使用 [!DNL setCustomerIDs] 方法，它是ECID服务的一部分。 有关以下内容的更多信息 [!DNL setCustomerIDs] 在方法的中可用 [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html).

**快速提示：** 之前使用上述任一方法时，您参考了AAM [!UICONTROL Data Source] 使用 [!UICONTROL Data Source] ID（又称“DPID”）。 当更新到 [!DNL setCustomerIDs]，您需要使用AAM [!UICONTROL Data Source]的”[!UICONTROL Integration Code]”而非。 它仍然指向相同的 [!UICONTROL Data Source] 但只是个不同的标识。 这如下面的视频中所示。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下部分列出了根据您的实施方法更新到DIL8.0的步骤和建议：

## 更新至Adobe Experience Platform标记中的DIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL8.0的基本步骤

1. 如果您使用的是8.0之前的DIL，则在升级之前，请转到AAM扩展中的DIL配置，并记下您所使用的任何高级选项（将在后续步骤中使用）。
1. 请将AAM扩展更新到8.0或更高版本。
1. 验证您的Experience CloudID服务扩展的版本是否为3.3.0或更高版本。
1. 用于任何已弃用的方法/元素(例如 `disableIDSyncs`AAM )，或者在DIL的自定义代码中，启用ECID扩展中的ECID方法。

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. 发布更改。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在AdobeDTM中更新到DIL8.0 {#updating-to-dil-in-adobe-dtm}

1. 将AAM工具更新至8.0或更高版本。 此版本设置位于AAM工具的“常规”部分下。
1. 用于任何已弃用的方法/元素(例如 `disableIDSyncs`AAM )，请记下它们（以便将其添加到ECID工具中），然后从自定义DIL中移除它们 [!DNL DIL code] 在AAM工具中。
1. 将您的Experience CloudID服务扩展更新至版本3.3.0或更高版本
1. 将高级选项添加到您从AAM工具的自定义代码中删除的ECID工具中。
1. 发布更改

## 正在更新到DIL8.0，其中没有Adobe的Tag Management解决方案 {#additional-resources}

如果您直接在页面上更新代码，则只需使用较新的项目替换旧的项目即可，除非您需要将方法/元素从DIL移动到ECID，如上所述。 在这种情况下，您只需将DIL位置中的旧方法/元素替换为ECID位置中的ECID方法/元素即可。

非Adobe标签管理器的情况也是如此。 无论该标签管理解决方案中位于何处，只要您有旧版本，请按照以下步骤中的说明，将其替换为新的代码。

1. 将您的DIL库更新到最新版本（8.0或更高版本） — 您需要从“Adobe咨询”或“Adobe客户关怀”获取最新的DIL代码，因为它当前在公共场所不可用。 然后，只需将旧的DIL库代码替换为新的DIL库代码，并继续执行下一步即可（不要立即停止，否则您将遇到问题，ha）。
1. 安装 [!DNL ECID Service] 或将现有版本更新为3.3.0或更高版本。 您可以下载最新的Experience CloudID服务版本 [从我们的GitHub页面](https://github.com/Adobe-Marketing-Cloud/id-service/releases). 如果您需要这方面的帮助，请参见 [文档](https://experienceleague.adobe.com/docs/id-service/using/home.html) 或咨询Adobe顾问。

1. 验证自定义代码中用于DIL的任何已弃用方法或元素是否已移至ECID方法：

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
