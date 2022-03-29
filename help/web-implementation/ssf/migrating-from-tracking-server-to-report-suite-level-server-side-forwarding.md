---
title: 从跟踪服务器迁移到报表包级别的服务器端转发
description: 了解如何启用Adobe Analytics数据的服务器端转发以在报表包级别而不是在跟踪服务器级别Audience Manager。
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

本文和视频将向您展示如何启用的服务器端转发 [!DNL Analytics] 要Audience Manager的数据 [!UICONTROL report suite] 级别，而不是 [!UICONTROL tracking server] 级别。

## 简介 {#introduction}

如果您拥有Adobe Audience Manager和Adobe Analytics，则可以实施 [!DNL Analytics] Audience Manager。 这意味着，不是您的页面发送两次点击(一个 [!DNL Analytics] 和一个Audience Manager)，则会将点击发送到 [!DNL Analytics]和 [!DNL Analytics] 会将该数据转发到Audience Manager。

如果您已经启动并运行该功能，并且在2017年10月之前已启用/实施该功能，则您的服务器端转发可能基于 [!UICONTROL Tracking Server]，必须由Adobe客户关怀或Adobe咨询部门启用。 自2017年10月起，您现在可以自行配置服务器端转发，并在报表包级别（每个报表包的转发）执行此操作。 这有很大的好处，讨论如下。

## [!UICONTROL Tracking server] 转发 {#tracking-server-forwarding}

您的 [!UICONTROL tracking server] 是您发送 [!DNL Analytics] 数据，以及写入图像请求和cookie的域。 它应在DTM中设置，或 [!DNL Experience Platform Launch]，或 [!DNL AppMeasurement.js] 文件，且通常如下所示，您的网站或企业名称将替换为“mysite”：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果将服务器端转发设置为在 [!UICONTROL tracking server] 级别，发送到此的任何点击 [!UICONTROL tracking server] (如果Experience CloudID服务也处于启用状态)将转发到Audience Manager。 必须由Adobe客户关怀或Adobe咨询团队来启用此功能。 在您切换到 [!UICONTROL report suite] 转发，如下所述。

如果您不确定 [!DNL tracking server forwarding] ，请联系Adobe客户关怀或Adobe咨询，并告知您。

## [!UICONTROL Report-suite] — 级别的服务器端转发 {#report-suite-level-server-side-forwarding}

迁移到 [!UICONTROL report suite] 转发 [!UICONTROL tracking server] 转发是指您现在能够使用“Audience Analytics”，即转发Audience Manager [!UICONTROL segments] 返回Adobe Analytics以进行详细的区段分析。 如果您仍在运行，则不支持此功能 [!UICONTROL tracking server] 转发和不转发 [!UICONTROL report suite] 转发。 请参阅 [文档](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上面的视频所述，在您拥有 [!UICONTROL report suites] 如果您希望转发到Audience Manager，则应联系Adobe客户关怀或Adobe咨询部门，并让他们禁用 [!UICONTROL tracking server] 转发。 你这样做不是紧急事，因为你有了这两个 [!UICONTROL tracking server] 转发和转发 [!UICONTROL report suite] 转发不会导致重复点击。 但是，最佳做法是仅在 [!UICONTROL report suite] 转发开启。

如果你离开 [!UICONTROL tracking server] 转发开启，不仅会转发数据 [!UICONTROL report suites] 您不希望转发，但将来，在您（以及您公司的所有人）忘记 [!UICONTROL tracking server] 转发已开启，您可能认为数据不是针对特定 [!UICONTROL report suite]. 这是因为报表包级别未开启该功能，但仍会转发数据，因为 [!UICONTROL tracking server]. 然后，您将浪费时间和金钱来弄明白转发的原因，并为您意想不到的AAM服务器调用付费。 所以，最好禁用 [!UICONTROL tracking server] 在您拥有 [!UICONTROL report suites] 为您的业务需求提供合理的解决方案。
