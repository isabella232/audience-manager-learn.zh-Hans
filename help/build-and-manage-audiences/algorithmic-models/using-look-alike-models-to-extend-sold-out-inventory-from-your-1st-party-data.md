---
title: 使用相似人群拓展模型从第一方数据延长已售库存
description: 在本教程中，我们将介绍设置和使用相似人群拓展模型时应该采取的步骤，以便您能够创建新的相似人群拓展受众，并将其作为转化区段的扩展进行销售。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 0%

---

# 使用相似人群拓展数据[!UICONTROL First Party]中的已售库存[!UICONTROL Models] {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教程中，我们将介绍设置和使用相似人群拓展[!UICONTROL Models]时应采取的步骤，以便您能够创建新的相似人群拓展受众，并将其作为扩展销售，以便进行转化[!UICONTROL segment]。

## 用例详细信息 {#use-case-details}

您是内容发布者。 如果您已经售罄网站转换器的库存，则您可能认为您的机会将止于此。 输入AAM相似人群[!UICONTROL Models]。 通过使用此功能，您可以进一步扩展已售出的库存，还可以销售那些可能尚未转化，但外观/行为与已转化的人员的受众。 此受众[!UICONTROL segment]的售价通常低于实际的转化率，但是允许您通过为希望在您的网站上投放广告的广告商提供额外的受众选项，从而增加收益。 此用例的额外好处是，在第一方数据上运行此模型不会造成任何费用。

本教程中的步骤如下所示：

1. 识别/创建理想用户（转化）[!UICONTROL trait]或[!UICONTROL segment]
1. 使用此转化[!UICONTROL trait]/[!UICONTROL segment]作为基项创建[!UICONTROL model]
1. 在[!UICONTROL model]中选择[!UICONTROL First party]数据源，然后运行[!UICONTROL model]
1. 根据[!UICONTROL model]结果创建算法[!UICONTROL Trait]，并将[!UICONTROL trait]添加到[!UICONTROL segment]
1. 向感兴趣的广告商提供[!UICONTROL segment]以扩展转化[!UICONTROL segment]销售

## 识别/创建理想用户（转化）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的网站上让人们执行哪些操作？ 什么是转化事件？ 当然，此问题有许多不同的答案，具体取决于您的网站类型/垂直和组织目标。 无论如何，AAM中通常为满足这些条件的访客创建[!UICONTROL trait]。

在此用例中，已假定存在这种情况，因为您已经为转换器用户售出了库存。 但是，在本教程中，最好将其作为其余用例的参考。

此外，在使用事件创建[!UICONTROL traits]时，您还需要牢记一个主要问题，以便在[!UICONTROL trait]中收集的用户不会多于应收集的用户数。 请观看以下视频，了解大展示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在以上视频中，我显示的示例假定您具有Adobe Analytics。显然，情况可能并非如此。 如果您拥有Google Analytics(GA)，我们有一个模块，您可以使用该模块将数据发送到AAM（请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)）；如果您网站上的转化活动是通过GA发送到AAM，则可以从中创建转化特征。 如果您有其他分析解决方案（或者没有分析解决方案），您仍可以通过我们的DIL代码和`submit`函数等将数据发送到AAM。 （请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)）。 然后，再次根据在网站上执行转化活动时发送的数据创建转化特征。

## 从[!UICONTROL First Party]数据创建相似人群拓展图[!UICONTROL Model] {#creating-a-look-alike-model-from-first-party-data}

在此步骤中，我们将创建[!UICONTROL First Party]相似人群拓展[!UICONTROL Model]。 这意味着，我们不仅要将[!UICONTROL first party]转换[!UICONTROL trait]/[!UICONTROL segment]用于基础[!UICONTROL trait]/[!UICONTROL segment]（这对大多数[!UICONTROL models]而言都是正常的），而且我们还将只查看[!UICONTROL first party]数据池，以了解更多与转换器相似的人群。 我们不会查看[!UICONTROL second party]或[!UICONTROL third party]数据。

在此用例中，这很重要，因为我们正在尝试在网站上创建一个[!UICONTROL segment]用户，这些用户看起来像转换器，但尚未进行转换，因此我们可以将此类型的[!UICONTROL segment]出售给感兴趣的广告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 创建算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一个算法[!UICONTROL Trait]，以便能够使用[!UICONTROL model]的结果。 如果不创建[!UICONTROL trait]，则[!UICONTROL model]将毫无用处。 因此，运行[!UICONTROL model]后，请务必进入[!UICONTROL trait]对话框并创建算法[!UICONTROL Trait]。 以下视频将演示该视频，并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 向广告商提供算法[!UICONTROL Segment] {#offering-the-algorithmic-segment-to-advertisers}

创建算法[!UICONTROL Trait]后，可以创建一个新的[!UICONTROL segment]以将其放入，以便激活数据(您不能激活[!UICONTROL trait]，而是使用其中的算法[!UICONTROL Trait]创建一个新的[!UICONTROL trait] [!UICONTROL segment]，以便激活（使用）[!UICONTROL segment]。

在创建了[!UICONTROL segment]的[!UICONTROL first party]访客（这些访客在类似的[!UICONTROL model]中得分较高，即看起来像转换器，但尚未发生转换）后，即使您已售罄网站上所有实际转换器库存，您仍可以将此[!UICONTROL segment]提供给您网站上的广告商。 这是在Audience Manager中使用相似人群拓展[!UICONTROL Models]来扩展此受众并继续查看额外收入的绝佳方式。
