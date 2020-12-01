---
title: 在向AAM发送数据时使用SPA页面上的最佳实践
description: 在此文档中，我们将介绍从单页应用程序(SPA)发送数据到Adobe Audience Manager(AAM)时您应遵循并注意的几种最佳实践。 本文档将重点介绍使用Launch by Adobe，这是推荐的实现方法。
feature: implementation basics
topics: spa
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---


# 在向AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}发送数据时，使用SPA页面上的最佳实践

在此文档中，我们将介绍从[!UICONTROL Single Page Applications](SPA)发送数据至Adobe Audience Manager(AAM)时您应遵循并注意的几种最佳实践。 本文档重点介绍使用[!UICONTROL Experience Platform Launch]，这是推荐的实现方法。

## 初始注释

* 以下项目将假定您正在使用[!DNL Platform Launch]在您的站点上实施。 如果您不使用[!DNL Platform Launch]，则考虑事项仍然存在，但您需要将其调整为您的实现方法。
* 所有SPA都不同，因此您可能需要调整以下一些项目以最好地满足您的需求，但我们希望与您分享一些最佳实践；将数据从SPA页面发送到Audience Manager时需要考虑的事情。

## 在Experience Platform Launch{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}中使用SPA和AAM的简单示意图

![aam in的spa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如上所述，这是一个简化的示意图，说明在Adobe Audience Manager实施(没有Adobe Analytics)中使用[!DNL Platform Launch]处理SPA页面的方式。 正如您所看到的，这是相当直接的，最重要的决定是如何将视图变化（或操作）传达给[!DNL Platform Launch]。

## 从SPA页面{#triggering-launch-from-the-spa-page}触发[!DNL Launch]

在[!DNL Platform Launch]中触发规则(并因此将数据发送到Audience Manager)的两种更常用方法是：

* 设置JavaScript自定义事件(请参见示例[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)(使用Adobe Analytics)
* 使用[!UICONTROL Direct Call Rule]

在此Audience Manager示例中，我们将使用[!DNL Launch]中的[!UICONTROL Direct Call rule]触发进入Audience Manager的点击。 正如您在后面几节中所看到的，通过将[!UICONTROL Data Layer]设置为新值，这样，[!DNL Platform Launch]中的[!UICONTROL Data Element]可以拾取该值，这真的变得很有用。

## 演示页{#demo-page}

我们创建了一个小的演示页，其中演示了如何更改[!DNL data layer]中的值并将它发送到AAM，就像您在SPA页上所做的那样。 此功能可建模，以进行更复杂的更改。 您可以找到此演示页[HERE](https://aam.enablementadobe.com/SPA-Launch.html)。

## 设置 [!DNL data layer]{#setting-the-data-layer}

如前所述，当新内容加载到页面上或有人在网站上执行操作时，[!DNL data layer]需要在页面的标题中动态设置， BEFORE [!DNL Launch]将被调用并运行[!UICONTROL rules]，这样[!DNL Platform Launch]可以从[!DNL data layer]中选取新值并将其推入Audience Manager。

如果转到上面列出的演示站点并查看页面源，您将看到：

* 在调用[!DNL Platform Launch]之前，[!DNL data layer]位于页面头中
* 模拟SPA链接中的JavaScript更改[!UICONTROL Data Layer]，然后调用[!DNL Platform Launch](_satellite.track()调用)。 如果您使用的是JavaScript自定义事件而不是此[!UICONTROL Direct Call Rule]，则本课是相同的。 首先更改[!DNL data layer]，然后调用[!DNL Launch]。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他资源 {#additional-resources}

* [SPA在Adobe论坛上的讨论](https://forums.adobe.com/thread/2451022)
* [参考体系结构站点，展示如何在Launch by Adobe中实现SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics跟踪SPA时使用最佳实践](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用于此文章的演示站点](https://aam.enablementadobe.com/SPA-Launch.html)
