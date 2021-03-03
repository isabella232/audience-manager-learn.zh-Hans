---
title: 更新到Adobe Audience Manager DIL 8.0（或更高版本）
description: 本文将提供有关将Adobe Audience Manager(AAM)Data Integration Library(DIL)代码更新为版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而非Adobe Analytics数据的服务器端转发，它将涵盖没有Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。
feature: DIL实施
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: “开发人员、数据工程师”
level: 中间
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 1%

---


# 正在更新到Adobe Audience Manager的DIL版本8.0（或更高版本）{#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文将提供有关将Adobe Audience Manager(AAM)[!DNL Data Integration Library](DIL)代码更新为版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而非Adobe Analytics数据的服务器端转发，它将涵盖没有Adobe标签管理器解决方案的DTM、Launch by Adobe和实施。

## 概述 {#overview}

Audience Manager的[!DNL Data Integration Library](DIL)代码允许您在网站*上实施AAM。 在实施旧版DIL时，也不需要实施Adobe的Experience CloudID服务(ECID)（尽管这是一个很好的主意）。 从DIL版本8.0开始，对ECID版本3.3或更高版本存在硬依赖关系。 如果您在未使用ECID 3.3的情况下或使用早期版本实施DIL 8.0或更高版本，您将收到错误，并且它将无法正常工作。 由于您可以通过多种方式实施AAM，因此我们创建了此页面，以为您提供一些要执行的步骤以及一些建议。 在下面，您将发现这些步骤和建议按平台/实施方法分类。 [文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)中提供了有关DIL的更多信息。

* 如本页说明中所述，这将仅涵盖“客户端”DIL实施(由没有Adobe Analytics的AAM客户使用)。 如果您有Adobe Analytics，应使用实现AAM的服务器端转发方法。 此方法在[documentation](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)中有介绍。

## 重复和已弃用的元素和方法{#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的先前版本中，存在重复方法(在DIL和ECID中都具有相同功能的方法)，这导致对使用哪一种方法产生混淆。 通常，您需要同时使用它们并将其匹配起来，而且该信息没有与我们的客户很好地沟通。 从DIL 8.0开始，这些重复方法和元素在DIL中已弃用，建议您使用ECID版本。

例如：

* 使用[!DNL DIL.create]时，已弃用一些元素，应改用ECID元素。 这些元素在[[!DNL DIL.create] documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)中调用。
* [!DNL idSync]实例级方法也已弃用，如方法的[documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)中所调用。

## ID与客户ID {#id-syncing-with-a-customer-id}同步

在AAM中，您可以将计算机上的UUID（匿名唯一用户ID）与客户ID同步，以便上传有关该客户的脱机数据并将其与他们的联机行为关联，以便更好地了解您的客户。 过去，这是通过以下两种方式之一实现的：

* [!DNL idSync]实例级方法
* [!DNL DIL.create]中的[!DNL declaredId]元素

如果您使用其中任一较旧的方法与客户ID同步，强烈建议您更新为使用[!DNL setCustomerIDs]方法，该方法是ECID服务的一部分。 有关[!DNL setCustomerIDs]的详细信息，请参阅方法的[文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)。

**快速提** 示：之前使用上述任一方法时，您使用 [!UICONTROL Data Source] ID [!UICONTROL Data Source] 引用了AAM（AKA为“DPID”）。更新到[!DNL setCustomerIDs]时，您需要改用AAM [!UICONTROL Data Source]的“[!UICONTROL Integration Code]”。 它仍指向相同的[!UICONTROL Data Source]，但只是一个不同的标识符。 以下视频显示了此内容。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下各节列表了根据您的实现方法更新到DIL 8.0的步骤和建议：

## 更新到Adobe Experience Platform Launch中的DIL 8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL 8.0的基本步骤

1. 如果您使用的是8.0之前版本的DIL，请在升级之前，进入AAM扩展中的DIL配置，并记下您使用的任何高级选项（将在后续步骤中使用）
1. 将您的AAM扩展更新到8.0或更高版本
1. 验证您的Experience CloudID服务扩展是否为3.3.0版或更高版本
1. 对于您8.0之前的AAM扩展或用于DIL的自定义代码中的任何已弃用的方法/元素（如“[!DNL disableIDSyncs]”），请在ECID扩展中启用ECID方法。

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs
   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs
   1. (DIL)iframeAkamaiHTTPS ->(ECID)dSyncSSLUseAkamai
   1. (DIL)declaredId ->(ECID)setCustomerID

1. 发布更改

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在Adobe DTM {#updating-to-dil-in-adobe-dtm}中更新到DIL 8.0

1. 将您的AAM工具更新至8.0或更高版本。 此版本设置位于AAM工具的“常规”部分下。
1. 对于8.0之前AAM工具的自定义代码中用于DIL的任何已弃用的方法/元素（如“disableIDSyncs”），请注意这些方法/元素（以便将它们添加到ECID工具），然后从AAM工具的自定义[!DNL DIL code]中删除它们。
1. 将您的Experience CloudID服务扩展更新为3.3.0版或更高版本
1. 将高级选项添加到您从AAM工具的自定义代码中删除的ECID工具。
1. 发布更改

## 更新到DIL 8.0时没有Adobe标签管理解决方案{#additional-resources}

如果要直接在页面上更新代码，则您只需将旧项目替换为较新的项目，但需要将方法/元素从DIL移动到ECID时除外，如上所述。 在这种情况下，您只需将DIL位置中的旧方法/元素替换为ECID位置中的ECID方法/元素。

对于非Adobe标签管理器，情况也是如此。 无论您在该标签管理解决方案中具有旧版本，请按照以下步骤所述将其替换为新代码。

1. 将您的DIL库更新至最新版本（8.0或更高版本） — 您需要从Adobe咨询或Adobe客户关怀获得最新的DIL代码，因为它当前不在公共位置提供。 然后，只需将旧DIL库代码替换为新DIL库代码，然后继续执行下一步（不要立即停止，否则您会遇到问题，哈）。
1. 安装[!DNL ECID Service]或将现有版本更新为3.3.0或更高版本。 您可以从我们的GitHub页面](https://github.com/Adobe-Marketing-Cloud/id-service/releases)下载最新的Experience CloudID服务版本[。 如果您需要帮助，请参阅[文档](https://marketing.adobe.com/resources/help/zh_CN/mcvid/)或与Adobe顾问交谈。

1. 验证DIL自定义代码中的任何已弃用的方法或元素是否已移到ECID方法：

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)iframeAkamaiHTTPS ->(ECID)idSyncSSLUseAkamai

      [文档](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL)declaredId ->(ECID)setCustomerID

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)