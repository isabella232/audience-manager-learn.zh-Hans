---
title: 使用算法（相似人群拓展）模型来增加ROAS
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
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# 在Audience Manager中使用算法（相似人群拓展）模型来增加ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager的“相似人物”的真正力量 [!UICONTROL Modeling] 当您寻求根据来自第二方和第三方数据源的全新优质用户集来扩展基准受众时，即会出现这种情况。 在本教程中，了解从此数据创建模型所需的步骤。

## 从Audience Marketplace启用第二方或第三方数据流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

要在相似人群拓展模型中使用第二方和第三方数据，我们首先必须在您的Audience Manager界面中启用此数据。 Adobe拥有大量可供您选择的第二方和第三方数据提供商。 这些配置文件可在AAM的自助式界面中通过Audience Marketplace提供。 导航到Audience Marketplace并浏览各种可能性。 以下视频将向您展示如何执行此操作，包括如何启用免费的“购买前尝试”流，以便您能够锁定对贵组织最有用的数据，然后再承诺使用数据提供商的定价。

此外，为了帮助您研究和确定要使用哪个数据提供商，一个非常棒的资源是 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 识别或创建理想的用户（转化）特征或区段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的网站上让人们执行哪些操作？ 什么是转化事件？ 当然，此问题有许多不同的答案，具体取决于您的网站类型/垂直和组织目标。 无论如何，在AAM中，通常为满足这些条件的访客创建特征。

在以下视频中，我将向您展示如何创建转化特征，在您完成本教程并创建相似人群拓展模型时，您希望该转化特征就绪。

此外，在使用Adobe Analytics事件创建特征时，还需要牢记一个重大问题，以便收集到的用户不会多于该特征中应收集的用户数。 请观看以下视频，了解大展示。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在以上视频中，我显示的示例假定您具有Adobe Analytics。 显然，情况可能并非如此。 如果您拥有Google Analytics(GA)，我们有一个模块，您可以使用该模块将数据发送到AAM(请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))，并且如果您网站上的转化活动通过GA发送到AAM，则可以从中创建转化特征。 如果您有其他分析解决方案（或者没有分析解决方案），您仍可以通过我们的DIL代码和 `submit` 函数等 (请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))。 然后，根据在网站上执行转化活动时发送的数据创建转化特征。

## 从第二方或第三方数据创建相似人群拓展模型 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步骤后，我们现在可以创建算法（相似人群拓展）模型。 在建立模型时，我们将使用转化特征作为基本特征（要复制的关键访客），并且我们将使用已启用的第三方数据流作为要从中提取的人员池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳实践 {#an-important-best-practice}

在Audience Manager中创建算法模型时，显然我们希望模型尽可能有效。 由于模型考虑的是基本特征/区段成员所包含的所有特征，因此如果所有人员都位于特征/区段中，则对模型无益。 因此，如果您具有任何超级通用特征（例如，访问您网站的每个人，或从您收到任何广告的每个人，等等），请确保它们所属的数据源未包含在模型的数据源中。 在本文的用例中，您不太可能会这样做，因为我们将重点放在查看第三方数据以获取我们的新外观，但无论如何都值得一提，并且适用于您的所有算法模型。

## 创建 [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建  [!UICONTROL Algorithmic Trait]，以便使用模型的结果。 如果不创建特征，模型将毫无用处。 因此，在模型运行后，请务必进入特征对话框并创建 [!UICONTROL Algorithmic Trait]. 以下视频将演示该视频并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 根据模型数据创建区段，并将其发送到DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

创建 [!UICONTROL Algorithmic Trait]，则可以创建一个新区段以将其放入，以便激活数据(您无法激活某个特征，而是使用 [!UICONTROL Algorithmic Trait] ，以便激活（使用）区段。

根据此算法特征创建区段后，您将拥有一群潜在客户，这些客户看起来就像您网站上已转换的用户。 现在，您可以在Audience Manager中将此区段映射到任何DSP目标。 您将能够将营销目标定位到那些外观相似的访客，这些访客在您的网站上转化的可能性比普通公众更大，从而提高广告支出回报。
