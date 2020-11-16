---
title: 更新至Adobe Audience Manager的DIL版本8.0（或更高版本）
description: 本文将提供有关将Adobe Audience Manager(AAM)Data Integration Library(DIL)代码更新到版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而非Adobe Analytics数据的服务器端转发，它将涵盖DTM、Launch by Adobe和没有Adobe标签管理器解决方案的实施。
feature: dil implementation
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 1%

---


# 更新至Adobe Audience Manager的DIL版本8.0（或更高版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文将提供有关将Adobe Audience Manager(AAM)( [!DNL Data Integration Library] DIL)代码更新到版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而非Adobe Analytics数据的服务器端转发，它将涵盖DTM、Launch by Adobe和没有Adobe标签管理器解决方案的实施。

## 概述 {#overview}

Audience Manager( [!DNL Data Integration Library] DIL)代码允许您在网站*上实施AAM。 在实施旧版DIL时，也无需实施Adobe的Experience CloudID服务(ECID)（尽管这是个很好的主意）。 从DIL版本8.0开始，对ECID版本3.3或更高版本存在硬依赖关系。 如果您在未使用ECID 3.3的情况下或在较早版本中实施DIL8.0或更高版本，您将收到错误，该错误将无法正常运行。 由于实施AAM有多种方法，因此我们创建了此页面，以便为您提供一些要执行的步骤以及一些建议。 下面您将看到这些步骤和建议按平台／实施方法分类。 有关DIL的更多信息，请参阅 [文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)。

* 如本页所述，这将仅涵盖“客户端”DIL实施(由没有Adobe Analytics的AAM客户使用)。 如果您有Adobe Analytics，应使用实施AAM的服务器端转发方法。 此方法在文档中有 [介绍](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)。

## 重复和已弃用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的先前版本中，存在重复方法(在DIL和ECID中都具有相同功能的方法)，这导致了对使用哪种方法的混淆。 通常，您需要同时使用它们并将其匹配起来，而我们的客户无法很好地传达这条信息。 从DIL8.0开始，这些重复方法和元素在DIL中已弃用，建议您使用ECID版本。

例如：

* 使用时 [!DNL DIL.create]，已弃用一些元素，您应改用ECID元素。 这些元素在文档中被 [[!DNL DIL.create] 调用](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)。
* 实 [!DNL idSync] 例级方法也已弃用，如方法文档中所调 [用](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)。

## ID与客户ID同步 {#id-syncing-with-a-customer-id}

在AAM中，您可以将计算机上的UUID（匿名唯一用户ID）与客户ID同步，以便上传有关该客户的脱机数据并将其与其在线行为关联，以更好地了解您的客户。 过去，这是通过以下两种方式实现的：

* 实 [!DNL idSync] 例级方法
* 中 [!DNL declaredId] 的元素 [!DNL DIL.create]

如果您使用其中任一旧方法与客户ID同步，强烈建议您更新为使用ECID服 [!DNL setCustomerIDs] 务中的方法。 有关详 [!DNL setCustomerIDs] 细信息，请参阅方法 [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)。

**快速提示：** 以前使用上述任一方法时，您使用 [!UICONTROL Data Source] ID引用 [!UICONTROL Data Source] 了AAM（AKA为“DPID”）。 更新到 [!DNL setCustomerIDs]时，您需要改用AAM [!UICONTROL Data Source]的“[!UICONTROL Integration Code]”。 它仍指向相同的标 [!UICONTROL Data Source] 识符，但只是不同的标识符。 这显示在以下视频中。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下各节列表了根据您的实现方法更新到DIL8.0的步骤和建议：

## 更新至Adobe Experience Platform LaunchDIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL8.0的基本步骤

1. 如果您使用的是8.0之前版本的DIL，请在升级之前进入AAM扩展中的DIL配置，并记下您正在使用的任何高级选项（将在后续步骤中使用）
1. 将AAM扩展更新到版本8.0或更高版本
1. 验证您的Experience CloudID服务扩展是否为3.3.0版或更高版本
1. 对于您8.0之前的AAM扩展或自定义代码中的任何已弃用的方法／元素(如[!DNL disableIDSyncs]“”)，请在ECID扩展中启用ECID方法。

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs
   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs
   1. (DIL)iframeAkamaiHTTPS ->(ECID)dSyncSSLUseAkamai
   1. (DIL)declaredId ->(ECID)setCustomerID

1. 发布更改

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在DILDTM中更新到Adobe8.0 {#updating-to-dil-in-adobe-dtm}

1. 将AAM工具更新至8.0版或更高版本。 此版本设置位于AAM工具的“常规”部分下。
1. 对于您8.0之前AAM工具的自定义代码中用于DIL的任何已弃用的方法／元素（如“disableIDSyncs”），请注意这些方法／元素（以便将它们添加到ECID工具），然后从AAM工具的自定义 [!DNL DIL code] 中删除它们。
1. 将Experience CloudID服务扩展更新为3.3.0版或更高版本
1. 将高级选项添加到您从AAM工具的自定义代码中删除的ECID工具。
1. 发布更改

## 更新到DIL8.0，无Adobe标签管理解决方案 {#additional-resources}

如果要直接在页面上更新代码，则只需将旧项目替换为较新的项目，但需要将方法／元素从DIL移到ECID时除外，如上所述。 在这种情况下，您只需将DIL位置中的旧方法／元素替换为ECID位置中的ECID方法／元素。

非Adobe标签管理器也是如此。 无论您在该标签管理解决方案中拥有旧版本，请按照以下步骤所述将其替换为新代码。

1. 将DIL库更新至最新版本（8.0或更高版本）-您需要从Adobe咨询或Adobe客户服务处获取最新的DIL代码，因为它当前不在公共位置。 然后，只需将旧DIL库代码替换为新DIL库代码，然后继续执行下一步（不要立即停止，否则您会遇到问题，哈）。
1. 安装 [!DNL ECID Service] 现有版本或将其更新至3.3.0或更高版本。 您可以从我们的GitHub页面下载最 [新的Experience CloudID服务版本](https://github.com/Adobe-Marketing-Cloud/id-service/releases)。 如果您需要有关此问题的帮助，请查 [看文档](https://marketing.adobe.com/resources/help/zh_CN/mcvid/) 或与Adobe顾问交谈。

1. 验证DIL自定义代码中的任何已弃用的方法或元素是否已移至ECID方法：

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)iframeAkamaiHTTPS ->(ECID)idSyncSSLUseAkamai

      [文档](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL)declaredId ->(ECID)setCustomerID

      [文档](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)