---
title: 使用相似模型扩展第一方数据中的已售库存
description: 在本教程中，我们将介绍设置和使用相似模型时应采取的步骤，以便您创建新的相似受众，并将它们作为转化细分的扩展进行销售。
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1688
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 0%

---


# 使用相似[!UICONTROL Models]扩展[!UICONTROL First Party]数据{#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}中的已售库存

在本教程中，我们将介绍设置和使用相似[!UICONTROL Models]时应采取的步骤，以便您创建新的相似受众，并作为转换[!UICONTROL segment]的扩展销售它们。

## 用例详细信息{#use-case-details}

您是内容发布者。 如果您已经为网站上的转换器销售了全部库存，您可能认为您的机会到此为止。 输入AAM的相似性[!UICONTROL Models]。 通过使用此功能，您可以进一步扩展已售出的库存，还可以销售受众尚未转化但外观／行为与已转化人员相似的人员。 此受众[!UICONTROL segment]通常比实际转换器的价格低，但通过为希望在您的网站上投放广告的广告商提供额外的受众选项，您仍可以增加营收。 此用例的额外好处是，在第一方数据上运行此模型不会花费任何成本。

本教程中的步骤如下：

1. 识别／创建理想用户（转换）[!UICONTROL trait]或[!UICONTROL segment]
1. 使用此转换[!UICONTROL trait]/[!UICONTROL segment]作为基项创建[!UICONTROL model]
1. 在[!UICONTROL model]中选择[!UICONTROL First party]数据源并运行[!UICONTROL model]
1. 根据[!UICONTROL model]结果创建算法[!UICONTROL Trait]，并将[!UICONTROL trait]添加到[!UICONTROL segment]
1. 将[!UICONTROL segment]优惠给感兴趣的广告商，以扩展转化[!UICONTROL segment]销售

## 识别／创建理想用户（转换）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您试图在您的站点上让他人执行哪些操作？ 您的转化事件是什么？ 当然，根据您的网站类型／垂直和组织目标，此问题有许多不同的答案。 无论如何，AAM中通常为满足这些条件的访客创建[!UICONTROL trait]。

在此用例中，已假定这是因为您已经为转换器人员售出了库存。 但是，在本教程中，最好将其作为其余用例的参考进行讨论。

此外，在使用事件创建[!UICONTROL traits]时，您需要记住一个重要错误，这样您收集的用户数不会超过您在[!UICONTROL trait]中应收集的用户数。 请观看以下视频，了解大展示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上面的视频中，我展示的示例假定您拥有Adobe Analytics。显然，情况可能并非如此。 如果您有Google Analytics(GA)，我们有一个模块，您可以用它将数据发送到AAM（请参阅[文档](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)）；如果您站点上的转换活动是通过GA发送到AAM的，则您可以从中创建转换特征。 如果您有其他分析解决方案（或没有分析解决方案），您仍可以通过我们的DIL代码和`submit`函数等将数据发送到AAM。 （请参阅[文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)）。 然后，同样，根据在网站上执行转换活动时发送的数据创建转换特征。

## 从[!UICONTROL First Party]数据{#creating-a-look-alike-model-from-first-party-data}创建类似[!UICONTROL Model]

在此步骤中，我们将创建[!UICONTROL First Party]类似[!UICONTROL Model]。 这意味着，我们不仅要对基本[!UICONTROL trait]/[!UICONTROL segment]使用[!UICONTROL first party]转换[!UICONTROL trait]/[!UICONTROL segment]（这对大多数[!UICONTROL models]来说都是正常的），而且我们只会为更多像转换器一样的人查看[!UICONTROL first party]数据池。 我们不会查看[!UICONTROL second party]或[!UICONTROL third party]数据。

在此用例中，这很重要，因为我们正在尝试在我们的网站上创建一个[!UICONTROL segment]用户，这些用户看起来像转换器，但尚未转换，因此我们可以将这个相似的[!UICONTROL segment]销售给感兴趣的广告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 创建算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一个算法[!UICONTROL Trait]，以便使用[!UICONTROL model]的结果。 如果不创建[!UICONTROL trait]，则[!UICONTROL model]是无用的。 因此，运行[!UICONTROL model]后，请务必进入[!UICONTROL trait]对话框并创建一个算法[!UICONTROL Trait]。 以下视频将逐步介绍这一过程，并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 向广告商{#offering-the-algorithmic-segment-to-advertisers}提供算法[!UICONTROL Segment]

创建算法[!UICONTROL Trait]后，您可以创建新[!UICONTROL segment]来放入它，以便激活数据(不能激活[!UICONTROL trait]，而是使用[!UICONTROL Trait]中的算法创建新的[!UICONTROL trait] [!UICONTROL segment]，以便激活（使用）[!UICONTROL segment]。

一旦您创建了[!UICONTROL first party]的[!UICONTROL segment]访客，在类似[!UICONTROL model]中得分较高（即看起来像转换器但尚未转换），即使您已经销售了网站上所有实际转换器库存，您也可以将此[!UICONTROL segment]优惠给您网站上的广告商。 这是扩展此受众并继续使用Audience Manager中的相似[!UICONTROL Models]获得额外收入的绝佳方式。
