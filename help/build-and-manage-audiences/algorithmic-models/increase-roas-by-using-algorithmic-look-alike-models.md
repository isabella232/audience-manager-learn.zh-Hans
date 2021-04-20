---
title: 在Audience Manager中使用算法（相似）模型来增加ROAS
description: Audience Manager相似建模的真正强大之处在于您寻求针对来自第2方和第3方数据源的全新高质量用户群扩展基准受众。 在本教程中，了解从此数据创建模型的步骤。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: "Business Practitioner, Developer, Data Engineer, Architect, Data Architect, Administrator, Leader"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 0%

---


# 在Audience Manager{#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}中使用算法（相似）[!UICONTROL Models]提高ROAS

当您寻求针对[!UICONTROL second party]和[!UICONTROL third party] [!UICONTROL data sources]中的优质、全新用户集扩展基准受众时，Audience Manager的相似性[!UICONTROL Modeling]的真正强大功能就来了。 在本教程中，了解根据此数据创建[!UICONTROL model]所需的步骤。

## 从Audience Marketplace{#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}启用[!UICONTROL Second Party]或[!UICONTROL Third Party]数据流

为了在类似[!UICONTROL model]中使用[!UICONTROL second party]和[!UICONTROL third party]数据，我们首先必须在您的Audience Manager接口中启用此数据。 Adobe有大量[!UICONTROL second party]和[!UICONTROL third party]数据提供者，您可以从中进行选择。 这些组件可通过Audience Marketplace在AAM的自助服务界面中提供。 导航到Audience Marketplace并浏览各种可能性。 以下视频将向您介绍如何实现此操作，包括如何启用免费的“购买前试用”流，以便在您承诺提供数据提供商的定价之前锁定对您的组织最有用的数据。

此外，为了帮助您研究和确定要使用哪个数据提供程序，[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)是一个不错的资源。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 识别/创建理想用户（转换）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您试图在您的网站上让他人执行哪些操作？ 您的转化事件是什么？ 当然，根据您的网站类型/垂直和组织目标，有许多不同的问题答案。 无论如何，AAM中通常为满足这些条件的访客创建[!UICONTROL trait]。

在以下视频中，我将向您介绍如何创建转换[!UICONTROL trait]，在您继续阅读本教程并创建相似的[!UICONTROL model]时，您将希望该转换正确。

此外，在使用Adobe Analytics事件创建[!UICONTROL traits]时，您需要牢记一个重大错误，这样您不会在[!UICONTROL trait]中收集比您应收集的用户多的用户。 观看以下视频，了解重大展示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上面的视频中，我显示的示例假定您拥有Adobe Analytics。显然，情况可能并非如此。 如果您有Google Analytics(GA)，则我们有一个模块，您可以用它将数据发送到AAM（请参阅[documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)）；如果您站点上的转换活动是通过GA发送到AAM的，则您可以从中创建转换[!UICONTROL trait]。 如果您有其他分析解决方案（或没有分析解决方案），您仍可以通过我们的DIL代码和`submit`函数等将数据发送到AAM。 （请参阅[文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)）。 然后，根据在站点上执行转换活动时发送的数据创建转换[!UICONTROL trait]。

## 从[!UICONTROL Second Party]或[!UICONTROL Third Party]数据{#create-a-look-alike-model-from-2nd-or-3rd-party-data}创建类似[!UICONTROL Model]

完成上述步骤后，我们现在可以创建Algorithmic(Look-alike)[!UICONTROL Model]。 在设置[!UICONTROL model]时，我们将使用转换[!UICONTROL trait]作为基本[!UICONTROL trait](要重复的关键访客)，并将启用的[!UICONTROL third party]数据流用作要抽取的人员池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳实践{#an-important-best-practice}

当在Audience Manager中创建算法[!UICONTROL model]时，显然我们希望[!UICONTROL model]尽可能有效。 由于[!UICONTROL model]正在考虑基[!UICONTROL trait]/[!UICONTROL segment]的所有成员都属于的[!UICONTROL traits]，因此，如果所有人员都在[!UICONTROL trait]/[!UICONTROL segment]中，则对[!UICONTROL model]没有帮助。 因此，如果您有任何超级通用[!UICONTROL traits]（与访问您网站的所有人或收到您任何广告的所有人一样），请确保它们所属的[!UICONTROL data source]不包含在[!UICONTROL model]的[!UICONTROL data sources]中。 在本文的用例中，您不太可能这样做，因为我们侧重于查看[!UICONTROL third party]数据以找到新的外观，但无论如何，这值得一提，并且适用于您的算法[!UICONTROL models]。

## 创建算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一个算法[!UICONTROL Trait]，以便使用[!UICONTROL model]的结果。 如果不创建[!UICONTROL trait]，则模型无用。 因此，运行[!UICONTROL model]后，请务必进入[!UICONTROL trait]对话框并创建Algorithmic [!UICONTROL Trait]。 以下视频将逐个介绍并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 从[!UICONTROL Model]数据创建[!UICONTROL Segment]并将其发送到DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

创建算法[!UICONTROL Trait]后，您可以创建新[!UICONTROL segment]来放入它，以便激活数据(不能激活[!UICONTROL trait]，而是使用[!UICONTROL Trait]中的算法创建新[!UICONTROL trait] [!UICONTROL segment]，以便激活（使用）[!UICONTROL segment])。

从此算法[!UICONTROL trait]创建[!UICONTROL segment]后，您将拥有一个潜在客户的受众，这些客户看起来就像您网站上已转换过的人。 现在，您可以将此[!UICONTROL segment]映射到Audience Manager中的任何DSP [!UICONTROL destinations]。 您将能够将营销目标给那些在您的网站上转化的相似人群，而不仅仅是普通公众，从而提高广告支出回报。 祝你好运！
