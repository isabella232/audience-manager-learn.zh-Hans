---
title: 如何识别合作伙伴ID或子域
description: 了解在实施某些Experience Cloud功能时如何识别合作伙伴ID或子域，以及有关在Audience ManagerUI中可获取此ID的两个位置。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# 如何识别Audience Manager子域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

在实施某些Experience Cloud功能时，您需要知道Audience Manager `Subdomain` 是(有时也称为 `client ID` 或 `Partner ID`)。 在此视频中，我们将向您展示在Audience ManagerUI中可获取此信息的两个位置。

## 放弃结局…… {#giving-away-the-ending}

如果您只想跳进来，在不观看这个短视频的情况下找到它，您可以找到 `Partner Subdomain` 在UI中的两个位置：

1. 如果您已经创建 [!UICONTROL rule-based] 特征，单击 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位于该文件夹特征列表中特征旁边，且URL将在URL中包含您的子域。
1. 如果您进入 **[!UICONTROL Tools]** > **[!UICONTROL Tags]** 界面，单击 **[!UICONTROL Get code]** 对于容器，子域位于Akamai行的末尾

如果您无法通过这些快速引用快速找到该视频，则该视频需要很短的时间。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>为Adobe Experience Cloud的每个客户分配了一个数字ID，这通常称为“PID”或合作伙伴ID。 这不是本文和视频中我们讨论的ID。 相反，“合作伙伴子域”（有时称为合作伙伴ID）通常是客户端名称的版本，并且是将数据发送到的服务器的子域。 例如，如果您的公司是“Bob&#39;s Knobs”（当然，哈哈），则您的合作伙伴子域可能是“bobsknobs”，而“PID”则更像“12345”。 您通常不需要知道PID，但子域对于了解这一点很重要，因此您可以配置Audience Manager实施。
