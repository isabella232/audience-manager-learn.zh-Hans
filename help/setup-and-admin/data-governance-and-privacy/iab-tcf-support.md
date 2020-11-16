---
title: IAB TCF 2.0Audience Manager支持
description: Adobe为您提供了通过选择加入功能以及IAB透明度和同意框架2.0(TCF 2.0)支持的Audience Manager插件管理和传达用户隐私选择的手段。 本文与文档一起使用，帮助您了解IAB TCF的Audience Manager插件，以及它如何与Adobe的选择加入对象和您的同意管理提供商(CMP)一起使用。
feature: data governance & privacy
topics: null
audience: implementer, architect
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
translation-type: tm+mt
source-git-commit: df8afb50ed3971dc47e6506d31a8222a7f488b25
workflow-type: tm+mt
source-wordcount: '1123'
ht-degree: 0%

---


# IAB TCF 2.0Audience Manager支持 {#iab-tcf-support-in-audience-manager}

Adobe为您提供了通过选择加入功能以及IAB透明度和同意框架2.0(TCF 2.0)支持的Audience Manager插件管理和传达用户隐私选择的手段。 本文与文档一起使用，帮助您了解IAB TCF的Audience Manager插件，以及它如何与Adobe的选择加入对象和您的同意管理提供商(CMP)一起使用。 要了解有关IAB的更多信息，请访问其网 [站https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：了解ECID的选择加入 {#first-step-understand-ecid-s-opt-in}

要了解如何使用IAB TCF，您首先必须了解 [!DNL Opt-in] 功能，它是Experience CloudID服务(ECID)库的一部分。 如果您不熟悉“选择加入”的工作方式，请首先查看 [此有帮助的文章](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) 。 您还应查看加入文 [档](https://docs.adobe.com/content/help/zh-Hans/id-service/using/implementation/opt-in-service/optin-overview.html)。 浏览完这些资源后，请返回此页并继续。

## The Audience Manager Plug-In for IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

现在，您至少基本了解了加入服务的工作方式，Audience Manager可以将支持层 [!DNL IAB Transparency and Consent Framework (TCF)] 叠在它上，这是通过加入加入加入对象的插件完成的。

IAB TCF的Audience Manager插件扩展了选择插件的功能，使AAM客户能够根据IAB TCF对下游合作伙伴的用户隐私权选择进行评估、认可和转发。 它提供了数据管理者(即Adobe客户)和供应商(DMP、DSP、SSP、广告服务器等)的标准 可用来了解同意情况。

## 启用IAB TCF {#enabling-iab-tcf}

如果您使用Adobe Experience Platform Launch，为IAB TCF启用Audience Manager插件很简单，因为它是一个简单的复选框，如以下简短视频所示：

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

或者，如果您不使用Launch，则可以在实 `isIabContext=true` 例化Experience Cloud访客时使用启用它。 这启动了IAB TCF流，即为同意收集添加了另一步骤，使用IAB TCF来查询IAB TC字符串，并将其返回到选择加入，而选择加入则与Experience Cloud解决方案通信。

## IAB TC字符串 {#iab-tcf-consent-string}

IAB提供的标准之一是“同意字符串”（也称为“DaisyBit”），它实际上是两个列表的集合：

1. 用途： **同意** ，该做什么？
1. 供应商： **谁同意** ?

### 用途 {#purposes}

IAB TCF 2.0提供了十种收集同意的“用途”(供应商可以对访客数据做什么)。 Adobe Audience Manager并不要求所有十项，而是仅要求出于以下目的获得同意（除供应商同意外）:

* **用途1:** 在设备上存储和／或访问信息；
* **目的十：** 开发和改进产品；
* **特别目的1:** 确保安全、防止欺诈和调试。

这是IAB TC字符串的第一部分，只记录为1和0，指示该目的/活动是否获得批准。

>[!NOTE]
>
>根据IAB法规，特殊用途1（确保安全、防止欺诈和调试）始终是允许的，用户不能反对。

### 供应商 {#vendors}

IAB TC字符串的另一部分是由几百个供应商组成的长列表，因此访客可以向一列表在网站上有标签的适用供应商展示，并可以选择使用哪些供应商。 供应商在列表中占据一席之地。 例如，Adobe Audience Manager在此列表的供应商编号为565。 如果列表中的该数字为“1”，则Audience Manager可以从列表前面执行已批准的操作。 如果AAM的位置为“0”，则不能对数据执行任何操作。

**要Audience Manager为使用IAB TCF来选择这些用途和供应商的客户提供UI，或者要批准／不批准所有活动，您必须使用在IAB TCF中注册的CMP合作伙伴，或构建支持IAB TCF并在IAB TCF中注册的合作伙伴。**

## 选择加入：在IAB和Adobe解决方案之间进行翻译 {#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的好处之一是上述标准用途可能比一列表Adobe解决方案更能让最终用户了解他们正在批准什么。 最终用户可能不知道“批准”Audience Manager或是指什 [!DNL Target]么，但“在设备上存储和／或访问信息”或“开发和改进产品”可能更容易理解和同意。

为了Audience Manager获得批准(即，为了将IAB目的转换为选择加入给AAM一次“是”投票)，必须征得最终用户的同意，如上所列，用途1和10。 如果其中任一项未获批准，或供应商未获批准，AAM将不执行像素点火或设置cookie。 同样，很多客户只需选择为最终用户提供“全部或无”UI，这当然会允许或禁止使用Audience Manager(以及其他Experience Cloud解决方案)。

文档中有一些关于IAB TCF [流的Audience Manager](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html) 插件如何应用于Publisher和Advertiser使用案例的重要信息。

## IAB:向下游发送同意 {#iab-sending-consent-downstream}

当使用IAB TCF的Audience Manager插件时，用户的同意选择也将发送到平台级（第三方）ID同步，以供在“全局供应商”列表中存在的合作伙伴使用，这样合作伙伴就拥有用户同意信息，并可以据此采取行动。 此信息以两个变量发送：

* gdpr = 1
* gdpr_connence =编码 [同意字符串]

注意的是，如果用户处于IAB上下文中且未提供同意（或提供负面同意），则Audience Manager根本不会收集IAB TC字符串，因此会丢弃调用。 所以，在那种情况下……下游不能通过同意。

## 演示 {#demo}

在以下视频中，查看ECID和解决方案的cookie和信标如何受IAB用户选择选择的影响。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

有关IAB TCF 2.0Audience Manager插件的更多详细信息，包括如何实现和测试、使用案例和工作流程，请参阅文 [档](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
