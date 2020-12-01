---
title: 如何识别Audience Manager合作伙伴ID或子域
description: 实施某些Experience Cloud功能时，您需要了解Audience Manager“合作伙伴ID”是什么（有时也称为“客户ID”或“子域”）。 在此视频中，我们将向您展示两个在Audience ManagerUI中可以获得此ID的位置。
feature: implementation basics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---


# 如何识别Audience Manager子域{#how-to-identify-your-audience-manager-partner-id-or-subdomain}

实施某些Experience Cloud功能时，您需要了解Audience Manager`Subdomain`是什么（有时也称为`client ID`或`Partner ID`）。 在此视频中，我们将向您展示在Audience ManagerUI中可以获取此信息的两个位置。

## 放弃结尾…… {#giving-away-the-ending}

如果您只是跳入并查找它，而不观看此简短的视频，您可以在UI的两个位置找到`Partner Subdomain`:

1. 如果已创建[!UICONTROL rule-based] [!UICONTROL trait]，请单击&#x200B;**[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位于该文 [!UICONTROL trait] 件夹列表 [!UICONTROL traits] 中的旁边，URL将在URL中包含您的子域。
1. 如果进入&#x200B;**[!UICONTROL Tools]** > **[!UICONTROL Tags]**&#x200B;接口并单击&#x200B;**[!UICONTROL Get code]**&#x200B;容器，则子域将位于Akamai行的末尾

如果您无法通过这些快速参考快速找到该视频，则视频将是一个短暂的任务。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>为Adobe Experience Cloud的每位客户分配了一个数字ID，它通常称为“PID”或合作伙伴ID。 这不是本文和视频中讨论的ID。 相反，“合作伙伴子域”（有时也称为合作伙伴ID）通常是客户端名称的版本，并且是向其发送数据的服务器的子域。 例如，如果您的公司是“Bob&#39;s Knobs”（当然，哈哈），则您的伙伴子域可能是“bobsknobs”，而“PID”则更像“12345”。 您通常不需要知道PID，但子域很重要，因此您可以配置Audience Manager实现。

