---
title: 在Audience Manager中使用算法（相似）模型来增加ROAS
description: Audience Manager相似建模的真正强大之处在于您寻求根据来自第二方和第三方数据源的全新优质用户群扩展基准受众。 在本教程中，学习根据此数据创建模型的步骤。
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# 在Audience Manager中使用算法（相似）来增加 [!UICONTROL Models] ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager相似性的真正强 [!UICONTROL Modeling] 大之处在于您寻求扩展基线受众，使之与来自和的优质、全新的用户群 [!UICONTROL second party] 相抗衡 [!UICONTROL third party][!UICONTROL data sources]。 在本教程中，学习根据此数据创建 [!UICONTROL model] 应用程序所需的步骤。

## 从Audience Marketplace [!UICONTROL Second Party] 启 [!UICONTROL Third Party] 用或启用数据流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

为了在类 [!UICONTROL second party] 似 [!UICONTROL third party] 中使用和使用数据，我 [!UICONTROL model]们首先必须将这些数据启用到您的Audience Manager界面中。 Adobe拥有大量可供 [!UICONTROL second party] 您选 [!UICONTROL third party] 择的数据提供者。 通过Audience Marketplace，您可以在AAM的自助服务界面中使用这些功能。 导航到Audience Marketplace并浏览各种可能性。 以下视频将向您展示如何实现这一点，包括如何启用免费的“购买前试用”流，以便您在承诺数据提供商定价之前锁定对组织最有用的数据。

此外，为了帮助您研究和确定要使用哪个数据提供者，一个极好的资源就是 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 识别／创建理想用户（转换） [!UICONTROL trait] 或 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您试图在您的站点上让他人执行哪些操作？ 您的转化事件是什么？ 当然，根据您的网站类型／垂直和组织目标，此问题有许多不同的答案。 无论如何，AAM中为符合这些标准的 [!UICONTROL trait] 访客创建一个标准是很常见的。

在以下视频中，我将向您展示如何创建转 [!UICONTROL trait]化，当您继续阅读本教程并创建相似内容时，您将希望实现该转换 [!UICONTROL model]。

此外，在使用Adobe Analytics事件进行创 [!UICONTROL traits]作时，您需要记住一个重要错误，这样您就不会收集到比您应该收集到的用户更多的用户 [!UICONTROL trait]。 请观看以下视频，了解大展示。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在以上视频中，我展示的示例假定您拥有Adobe Analytics。 显然，情况可能并非如此。 如果您有Google Analytics(GA)，我们有一个模块，您可以用它将数据发送到AAM(请参 [阅文档](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html))，如果您站点上的转换活动是通过GA发送到AAM的，则您可以从中创建转 [!UICONTROL trait] 换。 如果您有其他分析解决方案（或没有分析解决方案），您仍可以通过我们的DIL代码和函数将数据 `submit` 发送到AAM，等。 (请参阅 [文档](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html))。 然后，根据在 [!UICONTROL trait] 网站上执行转换活动时发送的数据创建转换。

## 从或数据创 [!UICONTROL Model] 建 [!UICONTROL Second Party] 类 [!UICONTROL Third Party] 型 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步骤后，我们现在可以创建算法（相似） [!UICONTROL Model]。 在设置时，我 [!UICONTROL model]们将使用转化作 [!UICONTROL trait] 为基础( [!UICONTROL trait] 我们希望重复的关键访客)，并将启用的数据流用作我们从中拉出的 [!UICONTROL third party] 人员池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳实践 {#an-important-best-practice}

当在Audience Manager中 [!UICONTROL model] 创建算法时，显然我们 [!UICONTROL model] 希望算法尽可能有效。 由于 [!UICONTROL model] 考虑的是基 [!UICONTROL traits] 地／成员所属的所有内容，因此，如果所有人员都在a/中，则这 [!UICONTROL trait]样做对您无益[!UICONTROL segment] 。 [!UICONTROL model][!UICONTROL trait][!UICONTROL segment]因此，如果您拥有任何超级通用 [!UICONTROL traits] 工具（如访问您网站的所有人或收到您任何广告的所有人等），请确保它们所属的 [!UICONTROL data source] 内容不包含在 [!UICONTROL data sources] 您的中 [!UICONTROL model]。 在本文的用例中，您不太可能这样做，因为我们侧重于查看新的外观 [!UICONTROL third party] 类型的数据，但无论如何都值得一提，并且适用于您的所有算法 [!UICONTROL models]。

## 创建算法 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下来，我们需要创建一个 [!UICONTROL Trait]算法，以便能够使用 [!UICONTROL model] 结果。 如果不创 [!UICONTROL trait]建模型，模型就毫无用处。 因此，运行 [!UICONTROL model] 后，一定要进入对话框并 [!UICONTROL trait] 创建一个Algorithmic [!UICONTROL Trait]。 以下视频将逐步介绍这些方法并显示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 从数 [!UICONTROL Segment] 据创 [!UICONTROL Model] 建并将其发送到DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

一旦您创建了Algorithmic( [!UICONTROL Trait]算法)，就可以创建新的 [!UICONTROL segment] Algorithmic（放入），这样您就可以激活数据(不能激活数据，而是使用其中的Algorithmic（算法）创建新的One- [!UICONTROL trait]-Algorithmic(只使[!UICONTROL trait] 用)，这样您就可以激活（使用） [!UICONTROL segment][!UICONTROL Trait][!UICONTROL segment]数据。

一旦您从这个 [!UICONTROL segment] Algorithmic创建了 [!UICONTROL trait]一个，您就会有一受众潜在客户，这些客户看上去就像您网站上已经转换过的用户。 现在，您可以将此 [!UICONTROL segment] 映射到任何DSP的 [!UICONTROL destinations] Audience Manager。 您将能够将营销目标给那些看似者，他们在您的网站上转化的可能性高于普通公众，从而提高广告支出回报。 祝你好运！
