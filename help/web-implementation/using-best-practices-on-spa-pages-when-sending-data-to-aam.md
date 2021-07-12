---
title: 在将数据发送到SPA时，在AAM页面上使用最佳实践
description: 在本文档中，我们将介绍在将数据从单页应用程序(SPA)发送到Adobe Audience Manager(AAM)时，您应遵循并注意的一些最佳实践。 本文档将重点介绍如何使用Launch by Adobe，这是推荐的实施方法。
feature: 实施基础知识
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# 在将数据发送到SPA时，在AAM页面上使用最佳实践 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

在本文档中，我们将介绍在将数据从[!UICONTROL Single Page Applications](SPA)发送到Adobe Audience Manager(AAM)时，您应遵循并注意的一些最佳实践。 本文档重点介绍使用[!UICONTROL Experience Platform Launch]，这是推荐的实施方法。

## 初始说明

* 以下项目假定您使用[!DNL Platform Launch]在网站上实施。 如果您没有使用[!DNL Platform Launch]，但需要根据实施方法调整这些注意事项，则这些注意事项仍会存在。
* 所有SPA都不同，因此您可能需要调整以下一些项目以最好地满足您的需求，但我们希望与您分享一些最佳实践；将数据从SPA页面发送到Audience Manager时需要考虑的事项。

## 在Experience Platform Launch中使用SPA和AAM的简单图表 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![适用于aam的spa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如上所述，这是一个简化的图表，用于说明在使用[!DNL Platform Launch]的Adobe Audience Manager实施(不含Adobe Analytics)中如何处理SPA页面。 如您所见，这是相当直接的，大的决定是如何将视图更改（或操作）传达到[!DNL Platform Launch]。

## 从SPA页面触发[!DNL Launch] {#triggering-launch-from-the-spa-page}

在[!DNL Platform Launch]中触发规则(并因此将数据发送到Audience Manager)的两种更常见方法是：

* 设置JavaScript自定义事件(请参阅使用Adobe Analytics的示例[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html))
* 使用[!UICONTROL Direct Call Rule]

在此Audience Manager示例中，我们将使用[!DNL Launch]中的[!UICONTROL Direct Call rule]来触发进入Audience Manager的点击。 正如您在后面的章节中所看到的，将[!UICONTROL Data Layer]设置为新值后，该值将变得非常有用，以便[!DNL Platform Launch]中的[!UICONTROL Data Element]可以选取该值。

## 演示页面 {#demo-page}

我们创建了一个小的演示页面，用于演示如何更改[!DNL data layer]中的值并将其发送到AAM，就像您在SPA页面上所做的一样。 此功能可以建模，以便进行所需的更复杂更改。 您可以在[HERE](https://aam.enablementadobe.com/SPA-Launch.html)中找到此演示页面。

## 设置 [!DNL data layer] {#setting-the-data-layer}

如前所述，当页面上加载新内容或有人在网站上执行操作时，需要在页面标题中动态设置[!DNL data layer]，然后调用[!DNL Launch]并运行[!UICONTROL rules]，以便[!DNL Platform Launch]可以从[!DNL data layer]中提取新值并将其推入Audience Manager。

如果您转到上面列出的演示网站并查看页面源，您将看到：

* [!DNL data layer]位于页面标题中，调用[!DNL Platform Launch]之前
* 模拟的SPA链接中的JavaScript会更改[!UICONTROL Data Layer]，然后调用[!DNL Platform Launch](_satellite.track()调用)。 如果您使用的是JavaScript自定义事件，而不是此[!UICONTROL Direct Call Rule]，则课程将相同。 首先更改[!DNL data layer]，然后调用[!DNL Launch]。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他资源 {#additional-resources}

* [SPA在Adobe论坛上的讨论](https://forums.adobe.com/thread/2451022)
* [引用架构站点以显示如何在Launch by Adobe中实施SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics中跟踪SPA时使用最佳实践](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用于本文的演示网站](https://aam.enablementadobe.com/SPA-Launch.html)
