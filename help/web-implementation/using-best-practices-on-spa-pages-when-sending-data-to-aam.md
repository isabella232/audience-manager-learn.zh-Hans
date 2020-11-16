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


# 在向AAM发送数据时使用SPA页面上的最佳实践 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

在此文档中，我们将介绍从(SPA)发送数据到Adobe Audience Manager(AAM)时您应遵循并 [!UICONTROL Single Page Applications] 注意的几种最佳实践。 本文档重点介绍使 [!UICONTROL Experience Platform Launch]用，这是推荐的实现方法。

## 初始注释

* 以下项目将假定您正在使用在 [!DNL Platform Launch] 您的网站上实施。 如果您不使用，则仍会存 [!DNL Platform Launch]在考虑因素，但您需要将其调整为您的实现方法。
* 所有SPA都不同，因此您可能需要调整以下一些项目以最好地满足您的需求，但我们希望与您分享一些最佳实践；将数据从SPA页面发送到Audience Manager时需要考虑的事情。

## 在Experience Platform Launch中与SPA和AAM协作的简单示意图 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![aam in的spa [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如上所述，这是一个简化的图表，说明在Adobe Audience Manager实施(不包括Adobe Analytics)中如何使用SPA页面 [!DNL Platform Launch]。 正如您所看到的，这是相当直接的，最重要的决定是如何将视图变化（或行动）传达给 [!DNL Platform Launch]。

## 从SPA [!DNL Launch] 页面触发 {#triggering-launch-from-the-spa-page}

在中触发规则(并因此将数据发 [!DNL Platform Launch] 送到Audience Manager)的两种更常用方法是：

* 设置JavaScript自定义事件(请参 [阅此处](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) (带有Adobe Analytics)
* 使用 [!UICONTROL Direct Call Rule]

在此Audience Manager示例中，我们将使用 [!UICONTROL Direct Call rule] 中 [!DNL Launch] 触发进入Audience Manager的点击。 正如您将在下几节中看到的，通过将值设 [!UICONTROL Data Layer] 置为新值，这将变得非常有用，因此可以通过 [!UICONTROL Data Element] 中的 [!DNL Platform Launch]。

## 演示页 {#demo-page}

我们创建了一个小的演示页面，演示如何更改中的值 [!DNL data layer] 并将其发送到AAM，就像您在SPA页面上所做的那样。 此功能可建模，以进行更复杂的更改。 您可以在此处找到此演示 [页面](https://aam.enablementadobe.com/SPA-Launch.html)。

## 设置 [!DNL data layer]{#setting-the-data-layer}

如前所述，当新内容加载到页面上或某人在网站上执行操作时， [!DNL data layer] 需要在页面头中动态设置，然后 [!DNL Launch] 才会调用并运行 [!UICONTROL rules]，以 [!DNL Platform Launch] 便 [!DNL data layer] 从中选取新值并将其推入Audience Manager。

如果转到上面列出的演示站点并查看页面源，您将看到：

* 在 [!DNL data layer] 呼叫之前，位于页面的标题中 [!DNL Platform Launch]
* 模拟SPA链接中的JavaScript会更 [!UICONTROL Data Layer]改，然 [!DNL Platform Launch] 后调用(_satellite.track()调用)。 如果您使用的是JavaScript自定义事件而不是 [!UICONTROL Direct Call Rule]此，则课程是一样的。 先更改 [!DNL data layer]，然后拔打电话 [!DNL Launch]。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他资源 {#additional-resources}

* [SPA在Adobe论坛上的讨论](https://forums.adobe.com/thread/2451022)
* [参考体系结构站点，展示如何在Launch by Adobe中实现SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics跟踪SPA时使用最佳实践](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用于此文章的演示站点](https://aam.enablementadobe.com/SPA-Launch.html)
