---
title: 如何识别您的合作伙伴ID或子域
description: 了解在实施某些Experience Cloud功能时如何识别您的合作伙伴ID或子域，以及可在Audience ManagerUI中获取此ID的两个大致位置。
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

# 如何识别您的Audience Manager子域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

在实施某些Experience Cloud功能时，您需要知道您的Audience Manager `Subdomain` 是(有时也称为 `client ID` 或 `Partner ID`)。 在本视频中，我们将向您显示两个您可以在Audience ManagerUI中获取此信息的位置。

## 正在放弃结局…… {#giving-away-the-ending}

如果你宁愿直接跳进去找到它，而不看这段短片，你可以找到你的 `Partner Subdomain` 在UI中的两个位置：

1. 如果您已创建 [!UICONTROL rule-based] 特征，单击 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位于该文件夹中特征列表的相应特征旁边，且URL将包含您在URL中的子域。
1. 如果您进入 **[!UICONTROL Tools]** > **[!UICONTROL Tags]** 界面并单击 **[!UICONTROL Get code]** 对于您的容器，子域将靠近Akamai行的结尾

如果您无法通过这些快速引用快速找到它，则视频是简短的时间承诺。 ：)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>为Adobe Experience Cloud的每个客户分配了一个数字ID，通常称为“PID”或合作伙伴ID。 这不是我们在本文和视频中谈论的ID。 相反，“合作伙伴子域”（有时称为合作伙伴ID）通常是客户端名称的版本，是数据发送到的服务器的子域。 例如，如果您的公司是“Bob&#39;s Knobs”（所有东西都是门把手，当然，哈哈），那么您的合作伙伴子域可能就是“bobsknobs”，而“PID”可能更像“12345”。 您通常不需要知道PID，但您的子域非常重要，因此您可以配置Audience Manager实施。
