---
title: IAB TCF 2.0在Audience Manager中支持
description: Adobe为您提供了通过选择加入功能以及IAB透明度和同意框架2.0(TCF 2.0)支持的Audience Manager插件管理和传达用户隐私选择的方法。 本文与文档一起使用，帮助您了解IAB TCF的Audience Manager插件，以及它如何与Adobe的Opt-in对象和您的同意管理提供商(CMP)一起使用。
feature: "Data Governance & Privacy"
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
translation-type: tm+mt
source-git-commit: 256edb05f68221550cae2ef7edaa70953513e1d4
workflow-type: tm+mt
source-wordcount: '1126'
ht-degree: 0%

---

# IAB TCF 2.0在Audience Manager{#iab-tcf-support-in-audience-manager}中支持

Adobe为您提供了通过选择加入功能以及IAB透明度和同意框架2.0(TCF 2.0)支持的Audience Manager插件管理和传达用户隐私选择的方法。 本文与文档一起使用，帮助您了解IAB TCF的Audience Manager插件，以及它如何与Adobe的Opt-in对象和您的同意管理提供商(CMP)一起使用。 要了解有关IAB的更多信息，请访问其网站：[https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：了解ECID的选择加入{#first-step-understand-ecid-s-opt-in}

要了解如何使用IAB TCF，您必须首先了解[!DNL Opt-in]功能，它是Experience Cloud ID Service(ECID)库的一部分。 如果您不熟悉选择加入的工作方式，请首先参阅[此有帮助的文章](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)。 您还应查看“选择加入[文档](https://docs.adobe.com/content/help/zh-Hans/id-service/using/implementation/opt-in-service/optin-overview.html)”。 浏览这些资源后，请返回此页并继续。

## IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}的Audience Manager插件

现在，您至少基本了解了选择加入服务的工作方式，Audience Manager可以将其[!DNL IAB Transparency and Consent Framework (TCF)]支持层叠起来，这可通过插件加入选择加入对象来实现。

IAB TCF的Audience Manager插件扩展了选择插件的功能，使AAM客户能够根据IAB TCF评估、认可用户隐私选择并将其转发给下游合作伙伴。 它提供了数据管理者(即Adobe客户)和供应商(DMP、DSP、SSP、广告服务器等)的标准 可以用来了解不同意的情况。

## 启用IAB TCF {#enabling-iab-tcf}

如果您使用Adobe Experience Platform Launch，则为IAB TCF启用Audience Manager插件很简单，因为它是一个简单的复选框，如以下简短视频所示：

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

或者，如果您不使用Launch，则可以在实例化Experience Cloud访客时使用`isIabContext=true`启用它。 这启动了IAB TCF流，即为同意收集添加了另一步骤，使用IAB TCF查询IAB TC字符串，并将其返回到Opt-in，Opt-in随后与Experience Cloud解决方案通信。

## IAB TC字符串{#iab-tcf-consent-string}

IAB提供的一个标准是“同意字符串”（也称为“DaisyBit”），它实际上是两个列表的集合：

1. 用途：**同意做什么？**
1. 供应商：**谁同意授予？**

### 用途{#purposes}

IAB TCF 2.0中有十种收集同意的&quot;目的&quot;(供应商可以对访客数据做什么)。 Adobe Audience Manager不要求所有十项，而是仅要求出于以下目的获得同意（除了供应商同意外）：

* **用途1:** 在设备上存储和/或访问信息；
* **目的10：开** 发和改进产品；
* **特殊目的1:** 确保安全、防止欺诈和调试。

这是IAB TC字符串的第一部分，仅记录为1和0，指示该目的/活动是否被批准。

>[!NOTE]
>
>根据IAB法规，特别用途1（确保安全、防止欺诈和调试）始终得到同意，用户不能反对。

### 供应商{#vendors}

IAB TC字符串的另一部分是由几百个供应商组成的长列表，因此访客可以向站点上有标签的适用供应商的列表显示，并可以选择要使用的供应商。 供应商在列表中保持其地位。 例如，Adobe Audience Manager在此列表上的供应商编号为565。 如果列表中的该数字为“1”，则Audience Manager可以从列表前面执行已批准的用途。 如果AAM点有“0”，则无法对数据执行任何操作。

**要让Audience Manager为使用IAB TCF来选择这些用途和供应商的客户提供UI，或批准/不批准所有活动，您必须使用已向IAB TCF注册的CMP合作伙伴，或构建支持IAB TCF并已向IAB TCF注册的CMP合作伙伴。**

## 选择加入：在IAB和Adobe解决方案之间转换{#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的好处之一是，上述标准用途可能使最终用户更多地了解他们正在批准的内容，而不是列表Adobe解决方案。 最终用户可能不知道“批准”Audience Manager或[!DNL Target]意味着什么，但“在设备上存储和/或访问信息”或“开发和改进产品”对他们来说可能更容易理解和同意。

为了Audience Manager获得批准(即，为了将IAB目的转换为选择加入，以给予AAM“是”票)，必须获得最终用户的同意，如上所列。 如果其中任何一项未获批准，或供应商未获批准，AAM将不执行像素点火或设置Cookie。 同样可喜的是，许多客户只是选择为最终用户提供“全部或无”UI，这当然会允许或禁止使用Audience Manager(和其他Experience Cloud解决方案)。

[documentation](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html)中有一些关于IAB TCFAudience Manager插件流如何应用于Publisher和Advertiser使用案例的重要信息。

## IAB:发送同意下游{#iab-sending-consent-downstream}

当使用IAB TCF的Audience Manager插件时，用户的同意选择也将发送到平台级（第三方）ID同步，以供全球供应商列表上的合作伙伴使用，以便合作伙伴获得用户同意信息并对其采取行动。 此信息以两个变量发送：

* gdpr = 1
* gdpr_connence = [已编码同意字符串]

注意的是，如果用户在IAB上下文中并且不提供同意（或提供负同意），则Audience Manager根本不会收集IAB TC字符串，因此会丢弃调用。 所以，在那种情况下……在下游不能通过同意。

## 演示 {#demo}

在以下视频中，查看ECID和解决方案的cookie和信标如何受IAB用户选择选择的影响。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

有关IAB TCF 2.0Audience Manager插件的更多详细信息，包括如何实现和测试、使用案例和工作流，请参阅[文档](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
