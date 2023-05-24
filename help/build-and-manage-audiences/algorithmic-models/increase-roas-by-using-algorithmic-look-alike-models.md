---
title: 使用算法（相似）模型提高ROAS
description: 当您寻求根据来自第二方和第三方数据源的高质量、全新用户集扩展基准受众时，Audience Manager相似人群建模的真正力量就来了。 在本教程中，了解根据此数据创建模型的步骤。
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

# 在Audience Manager中使用算法（相似）模型提高ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager相似人群的真正力量 [!UICONTROL Modeling] 当您寻求根据来自第二方和第三方数据源的崭新高质量用户集扩展基准受众时，会遇到这种情况。 在本教程中，了解根据此数据创建模型所需的步骤。

## 启用来自Audience Marketplace的第二方或第三方数据流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

要在相似人群拓展模型中使用第二方和第三方数据，我们首先必须将此数据启用到您的Audience Manager界面中。 Adobe有大量第二方和第三方数据提供商可供您选择。 这些通过Audience Marketplace在AAM中的自助式界面中提供。 导航到Audience Marketplace并浏览各种可能性。 以下视频介绍如何执行此操作，包括如何启用免费的“在购买之前尝试”流，以便在您承诺使用数据提供商的定价之前，锁定对您的组织最有用的数据。

此外，为了帮助您研究和决定要使用哪个数据提供商，一个很棒的资源是 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 识别或创建理想的用户（转化）特征或区段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想让用户在您的网站上执行什么操作？ 您的转化事件是什么？ 当然，根据您的网站类型/垂直以及组织目标，此问题有多种不同的答案。 无论如何，在AAM中通常都会为符合这些条件的访客创建一个特征。

在下面的视频中，我将展示如何创建转化特征，当您继续完成本教程并创建相似人群拓展模型时，需要将这些特征放置到适当位置。

此外，在使用Adobe Analytics事件创建特征时，需要牢记以下主要问题，以便您不会将过多的用户收集到特征中。 观看以下视频，了解重大展品。 ：)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上面的视频中，我展示的示例假定您拥有Adobe Analytics。 显然，情况可能并非如此。 如果您有Google Analytics(GA)，则我们有可用于将数据发送到AAM的模块(请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))，如果您网站上的转化活动是在GA之前发送到AAM，则您可以从中创建转化特征。 AAM如果您有其他Analytics解决方案（或没有Analytics解决方案），则仍可以通过我们的DIL代码和 `submit` 函数等。 (请参阅 [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))。 然后，根据在网站上执行转化活动时发送的数据创建转化特征。

## 从第二方或第三方数据创建相似人群拓展模型 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步骤后，我们现在可以创建算法（相似）模型。 在设置模型时，我们将使用转化特征作为基本特征（我们要复制的关键访客），并将已启用的第三方数据流用作可供提取的人员池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳实践 {#an-important-best-practice}

在Audience Manager中创建算法模型时，我们显然希望模型尽可能有效。 由于模型会考虑基本特征/区段成员属于的所有特征，因此如果所有人员都位于某个特征/区段中，则对模型没有帮助。 因此，如果您有任何超通用特征（例如，访问您网站的所有人或收到您任何广告的所有人等），请确保其所属的数据源不会包含在模型的数据源中。 在本篇文章的用例中，您不太可能这样做，因为我们侧重于查看第三方数据以了解我们的新外观，但无论如何这都值得一提，并且适用于您的所有算法模型。

## 创建 [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一个  [!UICONTROL Algorithmic Trait]，以便模型的结果能够被使用。 如果不创建特征，模型就毫无用处。 因此，在模型运行后，请务必进入特征对话框并创建 [!UICONTROL Algorithmic Trait]. 以下视频介绍了它并显示了几个提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 从模型数据创建一个区段并将其发送到DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

创建 [!UICONTROL Algorithmic Trait]，您可以创建一个新区段来放置它，以便激活数据（您无法激活特征，而只能使用创建一个新的单特征区段） [!UICONTROL Algorithmic Trait] ，以便您可以激活（使用）区段。

一旦您根据此算法特征创建了区段，您就拥有了一批潜在客户，他们看起来就像在您的网站上已转化的人。 现在，您可以将此区段映射到Audience Manager中的任何DSP目标。 您将能够将营销目标定位到相似人群中，这些相似人群比普通大众更有可能在您的网站上转化，从而提高广告支出回报率。
