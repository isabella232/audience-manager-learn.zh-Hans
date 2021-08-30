---
title: 从跟踪服务器迁移到报表包级别的服务器端转发
description: 本文和视频将向您展示如何在报表包级别而不是在跟踪服务器级别启用Analytics数据的服务器端转发以Audience Manager。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# 从[!UICONTROL Tracking Server]迁移到[!UICONTROL Report Suite] — 级别[!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和视频将向您展示如何启用[!DNL Analytics]数据的[!UICONTROL server-side forwarding]以在[!UICONTROL report suite]级别而不是在[!UICONTROL tracking server]级别Audience Manager。

## 简介 {#introduction}

如果您拥有Adobe Audience Manager和Adobe Analytics，则可以实施[!DNL Analytics]数据的“[!UICONTROL Server-side Forwarding]”以Audience Manager。 这意味着，与您的页面发送2次点击(1次到[!DNL Analytics],1次到Audience Manager)不同，它只需将点击发送到[!DNL Analytics],[!DNL Analytics]会将该数据转发到Audience Manager。 如果您已经启动并运行该功能，并且在2017年10月之前启用/实施了该功能，则[!UICONTROL server-side forwarding]可能基于您的“[!UICONTROL Tracking Server]”，该功能必须由Adobe客户关怀或Adobe咨询团队启用。 自2017年10月起，您现在可以自行配置[!UICONTROL server-side forwarding] ，并在[!UICONTROL Report Suite]级别执行（转发PER [!UICONTROL Report Suite]）。 这有很大的好处，将在下文讨论。

## [!UICONTROL Tracking Server] 转发 {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是发送[!DNL Analytics]数据的位置，也是写入图像请求和Cookie的域。 它应在DTM或[!DNL Experience Platform Launch]中，或在[!DNL AppMeasurement.js]文件中进行设置，通常如下所示，您的网站或业务名称将替换为“mysite”：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果将[!UICONTROL server-side forwarding]设置为在[!UICONTROL tracking server]级别转发，则发送到此[!UICONTROL tracking server]的任何点击(如果还启用了Experience CloudID服务)都将转发到Audience Manager。 必须由Adobe客户关怀或Adobe咨询团队来启用此功能。 在您切换到[!UICONTROL report suite]转发后，也可以禁用该转发，如下所述。

如果您不确定是否为您启用了[!DNL tracking server forwarding]，请联系Adobe客户关怀或Adobe咨询，他们应该能够告知您。

## [!UICONTROL Report Suite]-级别 [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

从[!UICONTROL tracking server]转发移动到[!UICONTROL report suite]转发的最大好处之一是您现在可以使用“Audience Analytics”，即将Audience Manager[!UICONTROL segments]转发回Adobe Analytics以进行详细的[!UICONTROL segment]分析。 如果您仍处于[!UICONTROL tracking server]转发状态，而未处于[!UICONTROL report suite]转发状态，则不支持此功能。 有关Audience Analytics的更多信息，请参阅[文档](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html)。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上面的视频中所述，当您将所有[!UICONTROL report suites]设置为要转发到Audience Manager后，您应联系Adobe客户关怀或Adobe咨询团队，并让他们禁用[!UICONTROL tracking server]转发。 执行此操作不是紧急情况，因为同时进行[!UICONTROL tracking server]转发和[!UICONTROL report suite]转发不会导致重复点击。 但是，最好只启用[!UICONTROL report suite]转发。 如果您保持[!UICONTROL tracking server]转发处于打开状态，不仅会转发您不希望转发的[!UICONTROL report suites]数据，而且将来，当您（以及您公司的每个人）忘记开启[!UICONTROL tracking server]转发后，您可能会认为数据没有转发特定[!UICONTROL report suite]的内容（因为它未在报表包级别开启），但仍然会转发数据，因为使用[!UICONTROL tracking server]。 然后，您将浪费时间和金钱来弄明白转发的原因，并为您意想不到的AAM服务器调用付费。 因此，最好在将所有[!UICONTROL report suites]都设置为转发后立即禁用[!UICONTROL tracking server]转发，这对您的业务需求有意义。
