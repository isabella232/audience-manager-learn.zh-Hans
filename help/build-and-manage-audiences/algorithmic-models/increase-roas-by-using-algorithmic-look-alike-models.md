---
title: 在Audience Manager中使用算法（相似人群拓展）模型来增加ROAS
description: Audience Manager相似人群拓展建模的真正强大功能是，当您寻求针对来自第二方和第三方数据源的一组全新优质用户，扩展基线受众。 在本教程中，了解根据此数据创建模型的步骤。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 0%

---

# 在Audience Manager中使用算法（相似人群拓展）[!UICONTROL Models]来增加ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

当您寻求根据[!UICONTROL second party]和[!UICONTROL third party] [!UICONTROL data sources]中全新的优质用户集来扩展基线受众时，Audience Manager相似人群拓展[!UICONTROL Modeling]的真正威力就来了。 在本教程中，了解从此数据创建[!UICONTROL model]所需的步骤。

## 从Audience Marketplace启用[!UICONTROL Second Party]或[!UICONTROL Third Party]数据流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

要在相似人群[!UICONTROL model]中使用[!UICONTROL second party]和[!UICONTROL third party]数据，我们首先必须在您的Audience Manager界面中启用此数据。 Adobe有大量[!UICONTROL second party]和[!UICONTROL third party]数据提供程序，您可以从中进行选择。 这些配置文件可在AAM的自助式界面中通过Audience Marketplace提供。 导航到Audience Marketplace并浏览各种可能性。 以下视频将向您展示如何执行此操作，包括如何启用免费的“购买前尝试”流，以便您能够锁定对贵组织最有用的数据，然后再承诺使用数据提供商的定价。

此外，为了帮助您研究和确定要使用哪个数据提供程序，[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)是一个很好的资源。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 识别/创建理想用户（转化）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的网站上让人们执行哪些操作？ 什么是转化事件？ 当然，此问题有许多不同的答案，具体取决于您的网站类型/垂直和组织目标。 无论如何，AAM中通常为满足这些条件的访客创建[!UICONTROL trait]。

在以下视频中，我将向您展示如何创建转化[!UICONTROL trait]，在您完成本教程并创建类似型[!UICONTROL model]时，您希望将该转化实施。

此外，在使用Adobe Analytics事件创建[!UICONTROL traits]时，您还需要牢记一个主要问题，以便在[!UICONTROL trait]中收集的用户不会多于应收集的用户数。 请观看以下视频，了解大展示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在以上视频中，我显示的示例假定您具有Adobe Analytics。显然，情况可能并非如此。 如果您拥有Google Analytics(GA)，我们有一个模块，您可以使用该模块将数据发送到AAM（请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)）；如果您网站上的转化活动通过GA发送到AAM，则可以从中创建转化[!UICONTROL trait]。 如果您有其他分析解决方案（或者没有分析解决方案），您仍可以通过我们的DIL代码和`submit`函数等将数据发送到AAM。 （请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)）。 然后，根据在网站上执行转化活动时发送的数据创建转化[!UICONTROL trait]。

## 从[!UICONTROL Second Party]或[!UICONTROL Third Party]数据创建相似人群[!UICONTROL Model] {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步骤后，我们现在可以创建算法（相似人群）[!UICONTROL Model]。 在设置[!UICONTROL model]时，我们将使用转化[!UICONTROL trait]作为基础[!UICONTROL trait]（我们要复制的关键访客），并且我们将使用已启用的[!UICONTROL third party]数据流作为要从中提取的人员池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳实践 {#an-important-best-practice}

在Audience Manager中创建算法[!UICONTROL model]时，显然我们希望[!UICONTROL model]尽可能有效。 由于[!UICONTROL model]正在考虑基[!UICONTROL trait]/[!UICONTROL segment]成员所包含的所有[!UICONTROL traits]，因此如果所有人员都位于[!UICONTROL trait]/[!UICONTROL segment]中，则对[!UICONTROL model]没有帮助。 因此，如果您有任何超级通用[!UICONTROL traits]（与访问您网站的每个人或从您收到任何广告的每个人一样），请确保它们所属的[!UICONTROL data source]未包含在[!UICONTROL model]的[!UICONTROL data sources]中。 在本文的用例中，您不太可能这样做，因为我们将重点关注[!UICONTROL third party]数据以获取我们的新外观，但无论如何都值得一提，并且适用于您的算法[!UICONTROL models]的所有。

## 创建算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一个算法[!UICONTROL Trait]，以便能够使用[!UICONTROL model]的结果。 如果不创建[!UICONTROL trait]，则模型无用。 因此，运行[!UICONTROL model]后，请务必进入[!UICONTROL trait]对话框并创建算法[!UICONTROL Trait]。 以下视频将演示该视频并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 从[!UICONTROL Model]数据创建[!UICONTROL Segment]并将其发送到DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

创建算法[!UICONTROL Trait]后，可以创建一个新的[!UICONTROL segment]以将其放入，以便激活数据(您不能激活[!UICONTROL trait]，而是使用其中的算法[!UICONTROL Trait]创建一个新的[!UICONTROL trait] [!UICONTROL segment]，以便激活（使用）[!UICONTROL segment])。

从此算法[!UICONTROL trait]创建[!UICONTROL segment]后，您将拥有一个潜在客户的受众，这些客户看起来就像您网站上已转换的用户。 现在，您可以将此[!UICONTROL segment]映射到Audience Manager中的任意DSP [!UICONTROL destinations]。 您将能够将营销目标定位到那些外观相似的访客，这些访客在您的网站上转化的可能性比普通公众更大，从而提高广告支出回报。 祝你好运！
