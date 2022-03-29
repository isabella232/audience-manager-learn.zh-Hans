---
title: 向AAM发送数据时，请在SPA页面上使用最佳实践
description: 了解将数据从单页应用程序(SPA)发送到Adobe Audience Manager(AAM)的最佳实践。 本文重点介绍如何使用Experience Platform标记，推荐的实施方法。
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d4874d9f6d7a36bb81ac183eb8b853d893822ae0
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# 向AAM发送数据时，请在SPA页面上使用最佳实践 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

本文档介绍了将数据从单页应用程序(SPA)发送到Adobe Audience Manager(AAM)的几个最佳实践。 本文重点介绍使用 [!UICONTROL Experience Platform tags]，推荐的实施方法。

## 初始说明

* 以下项目假定您使用Platform标记在网站上实施。 如果您未使用Platform标记，但需要根据实施方法进行调整，因此考虑事项仍然存在。
* 所有SPA都不同，因此您可能需要调整以下一些项目以最好地满足您的要求，但是Adobe希望共享一些最佳实践，当您将数据从SPA页面发送到Audience Manager时，您需要考虑这些实践。

## 在Experience Platform标记（以前称为Launch）中使用SPA和AAM的简单图表{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![标记中的aam的spa](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如上所述，这是一个简化的图表，用于说明在Adobe Audience Manager实施(不包括Adobe Analytics)中如何使用Platform标记来处理SPA页面。 如您所见，这是相当直接的，最重要的决定是如何将视图更改（或操作）传达到Platform标记。

## 从SPA页面触发标记 {#triggering-launch-from-the-spa-page}

在Platform标记中触发规则(从而将数据发送到Audience Manager)的两种更常见方法是：

* 设置JavaScript自定义事件（请参阅示例） [此处](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) 与Adobe Analytics)
* 使用 [!UICONTROL Direct Call Rule]

在本Audience Manager示例中，您使用 [!UICONTROL Direct Call rule] ，以触发进入Audience Manager的点击。 正如您在后面的章节中所看到的，通过设置 [!UICONTROL Data Layer] 值，以便该值可由 [!UICONTROL Data Element] 中。

## 演示页面 {#demo-page}

以下是一个小页面，用于演示如何更改数据层中的值并将其发送到Audience Manager，就像您在SPA页面上所做的一样。 此功能可以建模，以便进行所需的更复杂更改。 您可以找到此演示页面 [此处](https://aam.enablementadobe.com/SPA-Launch.html).

## 设置数据层 {#setting-the-data-layer}

如上所述，当页面上加载了新内容或有人在网站上执行操作时，需要在调用平台标记之前在页面标题中动态设置数据层并运行 [!UICONTROL rules]，以便Platform标签可以从数据层中提取新值并将其推送到Audience Manager。

如果您转到上面列出的演示网站并查看页面源，您将看到：

* 在调用Platform标记之前，数据层位于页面标题中
* 模拟的SPA链接中的JavaScript会更改 [!UICONTROL Data Layer]，然后调用Platform标记( `_satellite.track()` 调用)。 如果您使用的是JavaScript自定义事件，而不是 [!UICONTROL Direct Call Rule]，则课程内容相同。 首先更改 [!DNL data layer]，然后调用Platform标记。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他资源 {#additional-resources}

* [SPA在Adobe论坛上的讨论](https://forums.adobe.com/thread/2451022)
* [引用架构网站，以显示如何在Platform标记中实施SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics中跟踪SPA时使用最佳实践](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用于本文的演示网站](https://aam.enablementadobe.com/SPA-Launch.html)
