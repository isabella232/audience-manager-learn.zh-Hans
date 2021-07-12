---
title: 正在更新到Adobe Audience ManagerDIL版本8.0（或更高版本）
description: 本文将为您提供有关将Adobe Audience Manager(AAM)Data Integration Library(DIL)代码更新到版本8.0或更高版本的步骤和建议。 这是指“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，它将涵盖没有Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。
feature: DIL实施
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 正在更新到Adobe Audience Manager的DIL版本8.0（或更高版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文将为您提供有关将Adobe Audience Manager(AAM)[!DNL Data Integration Library](DIL)代码更新到版本8.0或更高版本的步骤和建议。 这是指“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，它将涵盖没有Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。

## 概述 {#overview}

Audience Manager的[!DNL Data Integration Library](DIL)代码允许您在网站上实施AAM*。 在实施DIL的早期版本时，不需要同时实施Adobe的Experience CloudID服务(ECID)（尽管这是一个非常好的主意）。 从DIL版本8.0开始，ECID版本3.3或更高版本存在严重依赖关系。 如果您在未使用ECID 3.3的情况下实施DIL8.0或更高版本，或者您实施的是早期版本，则会收到错误，该错误将无法正常运行。 由于您可以通过多种方式来实施AAM，因此我们创建此页面是为了为您提供一些要完成的步骤以及一些建议。 在下面，您将找到按平台/实施方法划分的这些步骤和建议。 有关DIL的更多信息，请参阅[文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)。

* 如本页描述中所述，这将仅涵盖“客户端”DIL实施，这些实施由没有Adobe Analytics的AAM客户使用。 如果您拥有Adobe Analytics，则应使用实施AAM的服务器端转发方法。 [文档](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)中介绍了此方法。

## 重复和已弃用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的先前版本中，存在重复的方法(在DIL和ECID中具有相同功能的方法)，这导致对使用哪种方法产生混淆。 通常，您需要同时使用这两个参数并进行匹配，并且该消息无法很好地传达给我们的客户。 从DIL8.0开始，DIL中已弃用这些重复的方法和元素，建议您使用ECID版本。

例如：

* 使用[!DNL DIL.create]时，已弃用一些元素，您应该改用ECID元素。 这些元素在[[!DNL DIL.create] 文档](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)中标注。
* [!DNL idSync]实例级别方法也已弃用，如方法的[documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)中所述。

## ID与客户ID同步 {#id-syncing-with-a-customer-id}

在AAM中，您可以将计算机上的UUID（匿名独特用户ID）与客户ID同步，以便您可以上传有关该客户的离线数据，并将其与他们的在线行为绑定，以便更好地了解您的客户。 过去，这是通过以下两种方式之一实现的：

* [!DNL idSync]实例级别方法
* [!DNL DIL.create]中的[!DNL declaredId]元素

如果您使用了以下任一旧方法来与客户ID同步，强烈建议您使用[!DNL setCustomerIDs]方法（ECID服务的一部分）更新为。 有关[!DNL setCustomerIDs]的更多信息，请参阅方法的[文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)。

**快速提示：** 以前使用上述任一方法时，您引用的AAM [!UICONTROL Data Source] 中 [!UICONTROL Data Source] 包含ID（即“DPID”）。更新到[!DNL setCustomerIDs]时，您需要改用AAM [!UICONTROL Data Source]的“[!UICONTROL Integration Code]”。 它仍指向相同的[!UICONTROL Data Source]，但只是不同的标识符。 下面的视频中显示了这一点。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下部分列出了根据您的实施方法更新到DIL8.0的步骤和建议：

## 在Adobe Experience Platform Launch中更新到DIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL8.0的基本步骤

1. 如果您使用的是8.0之前版本的DIL，请在升级之前，转到AAM扩展中的DIL配置，并记下您使用的任何高级选项（将在后续步骤中使用）
1. 将AAM扩展更新到版本8.0或更高版本
1. 确认您的Experience CloudID服务扩展的版本为3.3.0或更高版本
1. 对于8.0之前AAM扩展或用于DIL的自定义代码中任何已弃用的方法/元素（如“[!DNL disableIDSyncs]”），请在ECID扩展中启用ECID方法。

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs
   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs
   1. (DIL)iframeAkamaiHTTPS ->(ECID)dSyncSSLUseAkamai
   1. (DIL)declaredId ->(ECID)setCustomerIDs

1. 发布更改

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在DILDTM中更新到Adobe8.0 {#updating-to-dil-in-adobe-dtm}

1. 将AAM工具更新到版本8.0或更高版本。 此版本设置位于AAM工具的“常规”部分下。
1. 对于8.0之前AAM工具的自定义代码中用于DIL的任何已弃用的方法/元素（如“disableIDSyncs”），请注意这些方法/元素（以便您可以将其添加到ECID工具中），然后从AAM工具的自定义[!DNL DIL code]中删除它们。
1. 将您的Experience CloudID服务扩展更新到版本3.3.0或更高版本
1. 将高级选项添加到您从AAM工具的自定义代码中删除的ECID工具中。
1. 发布更改

## 正在更新到DIL8.0，但没有Adobe标签管理解决方案 {#additional-resources}

如果您直接在页面上更新代码，则只需将旧项目替换为较新的项目即可，除非您需要将方法/元素从DIL移动到ECID，如上所述。 在这种情况下，您只需将DIL位置中的旧方法/元素替换为ECID位置中的ECID方法/元素即可。

对于非Adobe标签管理器，情况也是如此。 无论您在该标签管理解决方案中具有哪些旧版本，请按照以下步骤所述，将其替换为新代码。

1. 将您的DIL库更新到最新版本（8.0或更高版本） — 您将需要从Adobe咨询或Adobe客户关怀团队获取最新的DIL代码，因为该代码当前在公共位置不可用。 然后，只需将旧DIL库代码替换为新DIL库代码，然后继续执行下一步（不要立即停止，否则您会遇到问题，哈）。
1. 安装[!DNL ECID Service]或将现有版本更新至3.3.0或更高版本。 您可以从我们的GitHub页面](https://github.com/Adobe-Marketing-Cloud/id-service/releases)下载最新的Experience CloudID服务版本[。 如果您需要此方面的帮助，请参阅[文档](https://marketing.adobe.com/resources/help/zh_CN/mcvid/)或与Adobe顾问联系。

1. 验证任何已弃用的方法或元素是否已移至ECID方法以进行DIL，请执行以下操作：

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)iframeAkamaiHTTPS ->(ECID)idSyncSSLUseAkamai

      [文档](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL)declaredId ->(ECID)setCustomerIDs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)
