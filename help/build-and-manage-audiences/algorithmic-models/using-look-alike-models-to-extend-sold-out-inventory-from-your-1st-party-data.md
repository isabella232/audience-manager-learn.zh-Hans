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


# 使用相似 [!UICONTROL Models] 扩展数据中的已售库存 [!UICONTROL First Party] 量 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教程中，我们将介绍设置和使用相似受众时应采取的步骤， [!UICONTROL Models]以便您能够创建新的相似，并将它们作为转化的扩展进行销售 [!UICONTROL segment]。

## 用例详细信息 {#use-case-details}

您是内容发布者。 如果您已经为网站上的转换器销售了全部库存，您可能认为您的机会到此为止。 进入AAM的相似 [!UICONTROL Models]。 通过使用此功能，您可以进一步扩展已售出的库存，还可以销售受众尚未转化但外观／行为与已转化人员相似的人员。 此受众 [!UICONTROL segment] 通常比实际转化器的受众量低，但是，如果您为希望在您网站上投放广告的广告商提供额外的选项，则可以增加您的利润。 此用例的额外好处是，在第一方数据上运行此模型不会花费任何成本。

本教程中的步骤如下：

1. 识别／创建理想用户（转换） [!UICONTROL trait] 或 [!UICONTROL segment]
1. 使用 [!UICONTROL model] 此转换 [!UICONTROL trait]/[!UICONTROL segment] 作为基项
1. 在 [!UICONTROL First party] 中选择数据源并 [!UICONTROL model] 运行 [!UICONTROL model]
1. 根据结果 [!UICONTROL Trait] 创建 [!UICONTROL model] 算法，并将 [!UICONTROL trait] 该算法 [!UICONTROL segment]
1. 优惠 [!UICONTROL segment] 给感兴趣的广告商以扩大转化销 [!UICONTROL segment] 售

## 识别／创建理想用户（转换） [!UICONTROL trait] 或 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您试图在您的站点上让他人执行哪些操作？ 您的转化事件是什么？ 当然，根据您的网站类型／垂直和组织目标，此问题有许多不同的答案。 无论如何，AAM中为符合这些标准的 [!UICONTROL trait] 访客创建一个标准是很常见的。

在此用例中，已假定这是因为您已经为转换器人员售出了库存。 但是，在本教程中，最好将其作为其余用例的参考进行讨论。

此外，在使用事件进 [!UICONTROL traits]行创建时，您需要牢记一个重大错误，这样您不会收集到比您应收集到的用户数量更多的用户 [!UICONTROL trait]。 请观看以下视频，了解大展示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在以上视频中，我展示的示例假定您拥有Adobe Analytics。 显然，情况可能并非如此。 如果您有Google Analytics(GA)，我们有一个模块，您可以用它将数据发送到AAM(请参阅 [文档](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html))，如果您站点上的转换活动是通过GA发送到AAM的，则您可以从中创建转换特征。 如果您有其他分析解决方案（或没有分析解决方案），您仍可以通过我们的DIL代码和函数将数据 `submit` 发送到AAM，等。 (请参阅 [文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html))。 然后，同样，根据在网站上执行转换活动时发送的数据创建转换特征。

## 从数据创建类 [!UICONTROL Model] 似 [!UICONTROL First Party] 内容 {#creating-a-look-alike-model-from-first-party-data}

在此步骤中，我们将创建 [!UICONTROL First Party] 相似 [!UICONTROL Model]。 这意味着，我们不仅将使用 [!UICONTROL first party] 转 [!UICONTROL trait]换/[!UICONTROL segment] (对于我们的基础 [!UICONTROL trait]/[!UICONTROL segment][!UICONTROL models][!UICONTROL first party] （无论如何，这对大多数人来说都是正常的），而且我们还将只针对更多看起来像转换器的人来研究数据池。 我们不会查看或 [!UICONTROL second party] 数 [!UICONTROL third party] 据。

在这种使用情况下，这很重要，因为我们试图在我们的网站上创建 [!UICONTROL segment] 一些用户，这些用户看起来像转换器，但还没有转换，这样我们就可以将这种相似产品销售给感兴趣的 [!UICONTROL segment] 广告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 创建算法 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一 [!UICONTROL Trait]个Algorithmic，以便使用 [!UICONTROL model] 结果。 没有创造 [!UICONTROL trait]，就 [!UICONTROL model] 毫无用处。 因此，运行 [!UICONTROL model] 后，请务必进入对话框并 [!UICONTROL trait] 创建一个Algorithmic [!UICONTROL Trait]。 以下视频将逐步介绍这一过程，并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 向广告商提 [!UICONTROL Segment] 供算法 {#offering-the-algorithmic-segment-to-advertisers}

一旦您创建了Algorithmic [!UICONTROL Trait]（算法），您就可以创建新的 [!UICONTROL segment] Algorithmic（放入），这样您就可以激活数据(不能激活一个数据，而是使用Algorithmic [!UICONTROL trait]（算法）创建一个新的Algorithm(只[!UICONTROL trait] 使用)，这样您就可以激活（使用）该数据 [!UICONTROL segment][!UICONTROL Trait][!UICONTROL segment]。

一旦您创建了一 [!UICONTROL segment] 些 [!UICONTROL first party][!UICONTROL model][!UICONTROL segment] 访客，他们在相似产品中得分较高（即，他们看起来像转换器，但还没发生过转换），您就可以将此优惠给您网站上的广告商，即使您已经销售了网站上所有实际转换器的库存。 这是扩展此受众并在Audience Manager中使用相似产品继续获得额外收入的极好 [!UICONTROL Models] 方式。
