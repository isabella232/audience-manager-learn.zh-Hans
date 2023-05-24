---
title: 从跟踪服务器迁移到报表包级别的服务器端转发
description: 了解如何启用Adobe Analytics数据的服务器端转发，以便在报表包级别而不是Audience Manager服务器级别跟踪数据。
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
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# 从跟踪服务器迁移到报表包级别的服务器端转发 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和视频将介绍如何启用服务器端转发 [!DNL Analytics] 要Audience Manager的数据 [!UICONTROL report suite] 级别而不是 [!UICONTROL tracking server] 级别。

## 简介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，则可以实施 [!DNL Analytics] Audience Manager数据。 这意味着，您的页面不会发送两次点击(一次到 [!DNL Analytics] 一个用于Audience Manager)，它可以向发送点击 [!DNL Analytics]、和 [!DNL Analytics] 会将该数据转发给Audience Manager。

如果您已经启动并运行了此功能，并且在2017年10月之前启用了/实施了此功能，则您的服务器端转发可能会基于 [!UICONTROL Tracking Server]，必须由Adobe客户关怀团队或Adobe咨询团队启用。 自2017年10月起，您现在可以自行配置服务器端转发，并在报表包级别执行此操作（每个报表包的转发）。 这样做有重大好处，具体讨论如下。

## [!UICONTROL Tracking server] 转发 {#tracking-server-forwarding}

您的 [!UICONTROL tracking server] 是您向其发送 [!DNL Analytics] 数据，以及写入图像请求和Cookie的域。 应在DTM中设置或 [!DNL Experience Platform Launch]，或在 [!DNL AppMeasurement.js] 文件，通常如下所示，您的网站或业务名称将取代“mysite”：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果将服务器端转发设置为在 [!UICONTROL tracking server] 级别，任何发送到此的点击 [!UICONTROL tracking server] (如果还启用了Experience CloudID服务)将转发到Audience Manager。 必须由Adobe客户关怀团队或Adobe咨询团队启用此功能。 在您切换到 [!UICONTROL report suite] 转发，如下所述。

如果您不确定是否 [!DNL tracking server forwarding] 已为您启用，请联系Adobe客户关怀或Adobe咨询部门，他们应该能够让您知道。

## [!UICONTROL Report-suite] — 级别服务器端转发 {#report-suite-level-server-side-forwarding}

迁移至以下位置的最大优势之一： [!UICONTROL report suite] 转发自 [!UICONTROL tracking server] 转发是指您现在能够使用“Audience Analytics”，即转发Audience Manager的功能 [!UICONTROL segments] 返回Adobe Analytics以进行详细的区段分析。 如果您仍在使用，则不支持此强大功能 [!UICONTROL tracking server] 转发而非 [!UICONTROL report suite] 转发。 欲了解有关Audience Analytics的更多信息，请参阅 [文档](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上面的视频中所述，一旦您拥有 [!UICONTROL report suites] 设置转发以转发至Audience Manager，则应联系Adobe客户关怀或Adobe咨询团队，让他们禁用 [!UICONTROL tracking server] 转发。 你这样做并非紧急，因为同时拥有两者 [!UICONTROL tracking server] 转发和 [!UICONTROL report suite] 转发不会导致重复点击。 但是，最佳实践是只要 [!UICONTROL report suite] 转发打开。

如果您离开 [!UICONTROL tracking server] 启用转发，不仅可以从以下位置转发数据： [!UICONTROL report suites] 您不希望进行转发，但在将来，在您（以及您公司的每个人）忘记以下内容之后 [!UICONTROL tracking server] 转发功能已启用，您可能认为数据未针对特定目标进行转发 [!UICONTROL report suite]. 这是因为未在报表包级别启用该功能，但数据仍会转发，这是因为 [!UICONTROL tracking server]. 然后，您将浪费时间和金钱来查明它转发的原因，并支付您未预期的AAM服务器调用。 所以，最好是禁用 [!UICONTROL tracking server] 一旦您拥有所有 [!UICONTROL report suites] 致力于满足您的业务需求。
