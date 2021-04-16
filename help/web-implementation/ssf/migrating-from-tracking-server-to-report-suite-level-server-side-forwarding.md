---
title: 从跟踪服务器迁移到报表包级别的服务器端转发
description: 本文和视频将向您介绍如何在报表包级别而不是在跟踪服务器级别启用将Analytics数据的服务器端转发到Audience Manager。
product: audience manager
feature: Adobe Analytics 集成
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
translation-type: tm+mt
source-git-commit: 256edb05f68221550cae2ef7edaa70953513e1d4
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 0%

---

# 从[!UICONTROL Tracking Server]迁移到[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和视频将向您介绍如何启用[!DNL Analytics]的[!UICONTROL server-side forwarding]数据以在[!UICONTROL report suite]级别而不是[!UICONTROL tracking server]级别Audience Manager。

## 简介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，则可以将[!DNL Analytics]数据的“[!UICONTROL Server-side Forwarding]”实施到Audience Manager。 这意味着，与发送2次点击(1到[!DNL Analytics],1到Audience Manager)的页面不同，它只需将点击发送到[!DNL Analytics]，而[!DNL Analytics]会将该数据转发到Audience Manager。 如果您已经启动并运行了此项，并且在2017年10月之前启用/实施了此项，则您的[!UICONTROL server-side forwarding]可能基于您的“[!UICONTROL Tracking Server]”，而“”必须由Adobe客户服务或Adobe咨询部门启用。 自2017年10月起，您现在可以自行配置[!UICONTROL server-side forwarding]，并在[!UICONTROL Report Suite]级别（转发PER [!UICONTROL Report Suite]）进行配置。 这有很大的好处，将在下文讨论。

## [!UICONTROL Tracking Server] 转发  {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是您发送[!DNL Analytics]数据的位置，也是写入图像请求和Cookie的域。 它应在DTM或[!DNL Experience Platform Launch]中，或在[!DNL AppMeasurement.js]文件中设置，并且通常会如此，您的站点或业务名称将替换为“mysite”：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果将[!UICONTROL server-side forwarding]设置为在[!UICONTROL tracking server]级别转发，则发送到此[!UICONTROL tracking server]的任何点击(如果还启用了Experience CloudID服务)将转发到Audience Manager。 这必须由Adobe客户关怀或Adobe咨询来实现。 也可以禁用它，在您切换到[!UICONTROL report suite]转发后，如下所述。

如果您不确定是否为您启用了[!DNL tracking server forwarding]，请与Adobe客户服务或Adobe咨询部门联系，他们应该能让您知道。

## [!UICONTROL Report Suite] — 级别  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

从[!UICONTROL tracking server]转发转到[!UICONTROL report suite]转发的最大好处之一是您现在可以使用“Audience Analytics”，即将Audience Manager[!UICONTROL segments]转发回Adobe Analytics以进行详细的[!UICONTROL segment]分析。 如果您仍在进行[!UICONTROL tracking server]转发，而不是进行[!UICONTROL report suite]转发，则不支持此功能。 请参阅[文档](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)中有关Audience Analytics的更多信息。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示{#additional-resources}

如上面的视频所述，将所有[!UICONTROL report suites]设置为转发以转发到Audience Manager后，您应联系Adobe客户服务或Adobe咨询部门并让他们禁用[!UICONTROL tracking server]转发。 这样做不是紧急情况，因为同时进行[!UICONTROL tracking server]转发和[!UICONTROL report suite]转发不会导致重复点击。 但是，最好只打开[!UICONTROL report suite]转发。 如果您保持[!UICONTROL tracking server]转发打开状态，它不仅会转发您不希望转发的[!UICONTROL report suites]数据，而且将来，在您(和您公司的所有人)忘记[!UICONTROL tracking server]转发打开状态后，您可能认为数据没有转发特定[!UICONTROL report suite]（因为在报表包级别未打开），但仍在转发数据，因为a4/>。 [!UICONTROL tracking server]然后，您将浪费时间和金钱来了解它转发的原因，并为您预料不到的AAM服务器呼叫付费。 因此，最好在所有[!UICONTROL report suites]设置为转发后立即禁用[!UICONTROL tracking server]转发，这对您的业务需求有意义。
