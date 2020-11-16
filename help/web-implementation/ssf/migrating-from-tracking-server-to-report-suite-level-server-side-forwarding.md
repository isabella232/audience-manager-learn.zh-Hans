---
title: 从跟踪服务器迁移到报告包级别的服务器端转发
description: 本文和视频将向您展示如何在报告套件级别而不是在跟踪服务器级别启用Audience Manager分析数据的服务器端转发。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# 从级 [!UICONTROL Tracking Server] 别迁 [!UICONTROL Report Suite]移到级别 [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和视频将向您展示如何在 [!UICONTROL server-side forwarding] 某个 [!DNL Analytics] 级别而非某个级 [!UICONTROL report suite] 别启用Audience Manager [!UICONTROL tracking server] 数据。

## 简介 {#introduction}

如果您拥有Adobe Audience Manager和Adobe Analytics，则可[!UICONTROL Server-side Forwarding]以向Audience Manager [!DNL Analytics] 实施“ ”数据。 这意味着，与您的页面发送2次点击(一 [!DNL Analytics] 次到Audience Manager，一次到Audience Manager)不同，它只需向其发送点击 [!DNL Analytics]，并将 [!DNL Analytics] 该数据转发到。 如果您已经启动并运行了此服务，并且在2017年10月之前启用／实施了此服务，则您的服务可能 [!UICONTROL server-side forwarding] 基于“[!UICONTROL Tracking Server]”，而“”必须由Adobe客户服务或Adobe咨询来启用。 自2017年10月起，您现在可 [!UICONTROL server-side forwarding] 以自行配置，并在某个级别( [!UICONTROL Report Suite] 转发PER [!UICONTROL Report Suite])进行。 这有很大的好处，下面将讨论。

## [!UICONTROL Tracking Server] 转发 {#tracking-server-forwarding}

您 [!UICONTROL tracking server] 是您发送数据的 [!DNL Analytics] 位置，也是写入图像请求和Cookie的域。 它应在DTM或文 [!DNL Experience Platform Launch]件中设 [!DNL AppMeasurement.js] 置，并且通常如此，您的站点或业务名称将替换为“mysite”:

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果 [!UICONTROL server-side forwarding] 设置为在级别转发， [!UICONTROL tracking server] 则发送到此的任何点击 [!UICONTROL tracking server] (如果Experience CloudID服务也已启用)都将转发给Audience Manager。 这必须由Adobe客户关怀或Adobe咨询来实现。 在您切换到转发后，也可以禁用它，如 [!UICONTROL report suite] 下所述。

如果您不确定是否 [!DNL tracking server forwarding] 为您启用了该选项，请与Adobe客户服务或Adobe咨询联系，他们应该能够告知您。

## [!UICONTROL Report Suite]-级别 [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

从转发转发转发的最大 [!UICONTROL report suite] 好处 [!UICONTROL tracking server] 之一是，您现在将能够使用“Audience Analytics”，即将Audience Manager转发回 [!UICONTROL segments] Adobe Analytics进行详细 [!UICONTROL segment] 分析。 如果您仍在转发而未转发，则不支 [!UICONTROL tracking server] 持此出色 [!UICONTROL report suite] 功能。 请参阅文档中有关Audience Analytics的更 [多信息](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上面的视频所述，一旦您将所有要转发给Audience Manager的设 [!UICONTROL report suites] 置都设置为转发，您应联系Adobe客户服务或Adobe咨询部门并让他们停用转 [!UICONTROL tracking server] 发。 这样做并非紧急情况，因为同时进行转发和 [!UICONTROL tracking server] 转发 [!UICONTROL report suite] 不会导致重复点击。 但是，最好只开发转 [!UICONTROL report suite] 发功能。 如果您 [!UICONTROL tracking server] 保留转发，它不仅会转发您不希望转发的数据，而且在您(和您公司的所有人)将来忘记转发已启用后，您可能认为数据没有转发特定的数据 [!UICONTROL report suites] （因为在报告包级别未打开它），但数据仍因为转发而转发 [!UICONTROL tracking server][!UICONTROL report suite][!UICONTROL tracking server]。 然后，您将浪费时间和金钱去了解它转发的原因，并为您意想不到的AAM服务器呼叫付费。 因此，只要您已准备好所有 [!UICONTROL tracking server] 符合您业务需求的转发，就 [!UICONTROL report suites] 应立即禁用转发。
