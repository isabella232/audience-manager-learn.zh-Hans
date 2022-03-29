---
title: 使用相似人群拓展模型来扩展来自第一方数据的已售库存
description: 在本教程中，我们将逐步介绍设置和使用相似人群拓展模型时应该采取的步骤，以便您能够创建新的相似人群拓展受众，并将其作为转化区段的扩展进行销售。
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
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# 使用相似人群拓展模型来扩展来自第一方数据的已售库存 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教程中，我们将介绍设置和使用相似人群拓展所需执行的步骤 [!UICONTROL Models]，以便您能够创建新的相似人群拓展受众，并将其作为转化区段的扩展进行销售。

## 用例详细信息 {#use-case-details}

您是内容发布者。 如果您已经售罄网站转换器的库存，则您可能认为您的机会将止于此。 输入AAM相似人群拓展 [!UICONTROL Models]. 通过使用此功能，您可以进一步扩展已售出的库存，还可以销售那些可能尚未转化，但外观/行为与已转化的人员的受众。 此受众区段的销售价格通常低于实际的转化率，但是，如果您的广告商希望在您的网站上投放广告，则可以为他们提供额外的受众选项，从而增加收益。 此用例的额外好处是，在第一方数据上运行此模型不会造成任何费用。

本教程中的步骤如下所示：

1. 识别/创建理想的用户（转化）特征或区段
1. 使用此转化特征/区段作为基项创建模型
1. 选择 [!UICONTROL First party] 数据源并运行模型
1. 创建 [!UICONTROL Algorithmic Trait] 将特征添加到区段
1. 将区段提供给感兴趣的广告商以扩展转化区段的销售

## 识别或创建理想的用户（转化）特征或区段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的网站上让人们执行哪些操作？ 什么是转化事件？ 当然，此问题有许多不同的答案，具体取决于您的网站类型/垂直和组织目标。 无论如何，在AAM中，通常为满足这些条件的访客创建特征。

在此用例中，已假定存在这种情况，因为您已经为转换器用户售出了库存。 但是，在本教程中，最好将其作为其余用例的参考。

此外，在使用事件创建特征时，还需要牢记一个重大问题，以便收集的用户数量不会超出该特征中应收集的用户数量。 请观看以下视频，了解大展示。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在以上视频中，我显示的示例假定您具有Adobe Analytics。 显然，情况可能并非如此。 如果您拥有Google Analytics(GA)，我们有一个模块，您可以使用该模块将数据发送到AAM(请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))，并且如果您网站上的转化活动通过GA发送到AAM，则可以从中创建转化特征。 如果您有其他分析解决方案（或者没有分析解决方案），您仍可以通过我们的DIL代码和 `submit` 函数等 (请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))。 然后，再次根据在网站上执行转化活动时发送的数据创建转化特征。

## 从第一方数据创建相似人群拓展模型 {#creating-a-look-alike-model-from-first-party-data}

在此步骤中，我们将创建 [!UICONTROL First Party] 相似模型。 这意味着我们不仅要将第一方转化特征/区段用于基本特征/区段（无论如何，这对于大多数模型都是正常的），而且我们还将只针对更多看起来像转化器的人查看第一方数据池。 我们不会查看第二方或第三方数据。

在此用例中，这很重要，因为我们正在尝试在我们的网站上创建一个类似于转换器但尚未转换的用户区段，以便我们可以将此类似区段出售给感兴趣的广告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 创建算法特征 {#creating-an-algorithmic-trait}

接下来，我们需要创建 [!UICONTROL Algorithmic Trait]，以便使用模型的结果。 如果不创建特征，模型将毫无用处。 因此，在模型运行后，请务必进入特征对话框并创建 [!UICONTROL Algorithmic Trait]. 以下视频将演示该视频，并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 提供 [!UICONTROL Algorithmic Segment] 到广告商 {#offering-the-algorithmic-segment-to-advertisers}

创建 [!UICONTROL Algorithmic Trait]，则可以创建一个新区段以将其放入，以便激活数据(您无法激活某个特征，而是使用 [!UICONTROL Algorithmic Trait] ，以便激活（使用）区段。

在您创建了一个在相似人群拓展模型中得分较高的第一方访客区段（即，看起来像转化器，但尚未发生转化）后，即便您已售罄网站上实际转化器库存，仍可以向网站上的广告商提供此区段。 这是扩展此受众并继续使用相似人群拓展来继续查看额外收入的有效途径 [!UICONTROL Models] Audience Manager。
