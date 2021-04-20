---
title: 向AAM发送数据时在SPA页面上使用最佳实践
description: 在此文档中，我们将介绍从单页应用程序(SPA)向Adobe Audience Manager(AAM)发送数据时您应遵循并注意的几种最佳实践。 本文档将重点介绍使用Launch by Adobe，这是推荐的实现方法。
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: "Developer, Data Engineer"
level: Experienced
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# 向AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}发送数据时，使用SPA页面上的最佳实践

在此文档中，我们将介绍从[!UICONTROL Single Page Applications](SPA)向Adobe Audience Manager(AAM)发送数据时您应遵循并注意的几种最佳实践。 本文档重点介绍使用[!UICONTROL Experience Platform Launch]，这是建议的实现方法。

## 初始注释

* 以下项目将假设您使用[!DNL Platform Launch]在您的站点上实施。 如果您不使用[!DNL Platform Launch]，则考虑事项仍然存在，但您需要将它们调整为您的实现方法。
* 所有SPA都不同，因此您可能需要调整以下一些项目以最好地满足您的需求，但我们希望与您分享一些最佳实践；当您将数据从SPA页面发送到Audience Manager时，您需要考虑的事情。

## 在Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}中使用SPA和AAM的简单示意图

![aam in  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如上所述，这是一个简化的示意图，说明在使用[!DNL Platform Launch]的Adobe Audience Manager实施(无Adobe Analytics)中如何处理SPA页面。 正如您所看到的，这是相当直接的，最重要的决定是如何将视图更改（或动作）传达到[!DNL Platform Launch]。

## 从SPA页面{#triggering-launch-from-the-spa-page}触发[!DNL Launch]

在[!DNL Platform Launch]中触发规则(因此将数据发送到Audience Manager)的两种更常见方法是：

* 设置JavaScript自定义事件(请参阅示例[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)(使用Adobe Analytics)
* 使用[!UICONTROL Direct Call Rule]

在此Audience Manager示例中，我们将使用[!DNL Launch]中的[!UICONTROL Direct Call rule]触发进入Audience Manager的点击。 正如您在下几节中将看到的，将[!UICONTROL Data Layer]设置为新值会变得非常有用，这样，[!DNL Platform Launch]中的[!UICONTROL Data Element]就可以拾取它。

## 演示页{#demo-page}

我们创建了一个小的演示页面，演示如何更改[!DNL data layer]中的值并将其发送到AAM，就像您在SPA页面上所做的那样。 此功能可以建模，以便进行更复杂的更改。 您可以在此演示页[此处](https://aam.enablementadobe.com/SPA-Launch.html)找到。

## 设置 [!DNL data layer]{#setting-the-data-layer}

如前所述，当页面上加载新内容或有人在网站上执行操作时，需要在页面的头中动态设置[!DNL data layer]，然后调用[!DNL Launch]并运行[!UICONTROL rules]，这样[!DNL Platform Launch]可以从[!DNL data layer]中提取新值并将其推入Audience Manager。

如果转到上面列出的演示站点并查看页面源，您将看到：

* 在调用[!DNL Platform Launch]之前，[!DNL data layer]位于页面的头部
* 模拟SPA链接中的JavaScript更改[!UICONTROL Data Layer]，然后THEN调用[!DNL Platform Launch](_satellite.track()调用)。 如果您使用的是JavaScript自定义事件而不是此[!UICONTROL Direct Call Rule]，则本课是相同的。 首先更改[!DNL data layer]，然后调用[!DNL Launch]。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他资源 {#additional-resources}

* [SPA在Adobe论坛上的讨论](https://forums.adobe.com/thread/2451022)
* [参考体系结构站点，以说明如何在Launch by Adobe中实现SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics中跟踪SPA时使用最佳实践](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用于此文章的演示站点](https://aam.enablementadobe.com/SPA-Launch.html)
