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


# 从[!UICONTROL Tracking Server]迁移到[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和视频将向您展示如何启用[!DNL Analytics]数据的[!UICONTROL server-side forwarding]以[!UICONTROL report suite]级别而不是[!UICONTROL tracking server]级别Audience Manager。

## 简介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，则可以将[!DNL Analytics]数据的“[!UICONTROL Server-side Forwarding]”实施到Audience Manager。 这意味着，与发送2次点击(1次到[!DNL Analytics],1次到Audience Manager)的页面不同，它只需向[!DNL Analytics]发送点击，而[!DNL Analytics]会将该数据转发到Audience Manager。 如果您已经启动并运行了此服务，并且您在2017年10月之前启用／实施了此服务，则您的[!UICONTROL server-side forwarding]可能基于您的“[!UICONTROL Tracking Server]”，而“&lt;a1/>”必须由Adobe客户服务或Adobe咨询来启用。 自2017年10月起，您现在可以自行配置[!UICONTROL server-side forwarding]，并在[!UICONTROL Report Suite]级别（转发PER [!UICONTROL Report Suite]）进行配置。 这有很大的好处，下面将讨论。

## [!UICONTROL Tracking Server] 转发  {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是您发送[!DNL Analytics]数据的位置，也是写入图像请求和cookie的域。 它应在DTM或[!DNL Experience Platform Launch]中，或在[!DNL AppMeasurement.js]文件中设置，并且通常如此，您的站点或业务名称将替换为“mysite”:

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果[!UICONTROL server-side forwarding]设置为在[!UICONTROL tracking server]级别转发，则发送到此[!UICONTROL tracking server]的任何点击(如果Experience CloudID服务也已启用)都将转发给Audience Manager。 这必须由Adobe客户关怀或Adobe咨询来实现。 在您切换到[!UICONTROL report suite]转发后，也可以禁用它，如下所述。

如果您不确定是否为您启用了[!DNL tracking server forwarding]，请与Adobe客户服务或Adobe咨询联系，他们应该能够告知您。

## [!UICONTROL Report Suite]-级别  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

从[!UICONTROL tracking server]转发转到[!UICONTROL report suite]转发的最大好处之一是您现在可以使用“Audience Analytics”，即将Audience Manager[!UICONTROL segments]转回Adobe Analytics以进行详细的[!UICONTROL segment]分析。 如果您仍处于[!UICONTROL tracking server]转发状态，而不是处于[!UICONTROL report suite]转发状态，则不支持此功能。 有关Audience Analytics的详细信息，请参阅[文档](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示{#additional-resources}

如上面的视频所述，将所有[!UICONTROL report suites]设置为转发给Audience Manager后，您应联系Adobe客户服务或Adobe咨询部门并让他们禁用[!UICONTROL tracking server]转发。 这样做并非紧急情况，因为同时进行[!UICONTROL tracking server]转发和[!UICONTROL report suite]转发不会导致重复点击。 但是，最好只启用[!UICONTROL report suite]转发。 如果您保持[!UICONTROL tracking server]转发状态，它不仅会转发您不希望转发的[!UICONTROL report suites]数据，而且将来，在您(和您公司的所有人)忘记[!UICONTROL tracking server]转发处于开启状态后，您可能认为数据没有转发特定[!UICONTROL report suite]（因为在报告包级别未打开），而且数据仍在转发，因为&lt;a1/>a4/>。 [!UICONTROL tracking server]然后，您将浪费时间和金钱去了解它转发的原因，并为您意想不到的AAM服务器呼叫付费。 因此，只要您将所有[!UICONTROL report suites]设置为转发，就可以立即禁用[!UICONTROL tracking server]转发，这对您的业务需求有意义。
