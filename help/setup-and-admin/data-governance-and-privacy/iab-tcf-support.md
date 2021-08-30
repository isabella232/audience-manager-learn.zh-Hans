---
title: IAB TCF 2.0支持Audience Manager
description: Adobe通过选择加入功能和IAB透明度与同意框架2.0(TCF 2.0)支持Audience Manager插件，为您提供用于管理和传达用户所做的隐私选择的方法。 本文将与文档结合使用，帮助您了解IAB TCFAudience Manager插件，以及它如何与Adobe的选择加入对象和同意管理提供商(CMP)一起使用。
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 086071ab04551c512c5415f091a8054123bc6445
workflow-type: tm+mt
source-wordcount: '1117'
ht-degree: 0%

---

# IAB TCF 2.0支持Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe通过选择加入功能和IAB透明度与同意框架2.0(TCF 2.0)支持Audience Manager插件，为您提供用于管理和传达用户所做的隐私选择的方法。 本文将与文档结合使用，帮助您了解IAB TCFAudience Manager插件，以及它如何与Adobe的选择加入对象和同意管理提供商(CMP)一起使用。 要进一步了解IAB，请访问其网站：[https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：了解ECID的选择加入 {#first-step-understand-ecid-s-opt-in}

要了解如何使用IAB TCF，您必须首先了解[!DNL Opt-in]功能，该功能是Experience CloudID服务(ECID)库的一部分。 如果您不熟悉选择加入的工作方式，请首先参阅[这篇有用的文章](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)。 您还应查看选择加入[文档](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html)。 完成这些资源后，请返回此页并继续。

## 适用于IAB TCF的Audience Manager插件 {#the-audience-manager-plug-in-for-iab-tcf}

现在，您至少已基本了解选择加入服务的工作方式，接下来，Audience Manager可以将[!DNL IAB Transparency and Consent Framework (TCF)]支持层叠在上面，这可以通过插件加入选择加入对象中来完成。

适用于IAB TCF的Audience Manager插件扩展了选择加入的功能，使AAM客户能够根据IAB TCF评估、尊重用户隐私选择并将其转发给下游合作伙伴。 它提供了一个标准，即数据控制者(即Adobe客户)和供应商(DMP、DSP、SSP、广告服务器等) 可用于在同意范围中了解同意。

## 启用IAB TCF {#enabling-iab-tcf}

如果您使用的是Adobe Experience Platform Launch，则启用适用于IAB TCF的Audience Manager插件会非常简单，因为它是一个简单的复选框，如以下简短视频所示：

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

或者，如果您没有使用Launch，则可以在实例化Experience Cloud访客时使用`isIabContext=true`启用它。 这会启动IAB TCF流程，即使用IAB TCF查询IAB TCF字符串并将其提供给选择加入，然后选择加入与Experience Cloud解决方案通信，从而向同意收集添加另一个步骤。

## IAB TC字符串 {#iab-tcf-consent-string}

IAB提供的其中一个标准是“同意字符串”（也称为“DaisyBit”），它实际上是两个列表的总和：

1. 目的：**同意做什么？**
1. 供应商：**谁**&#x200B;同意接受？

### 目的 {#purposes}

在IAB TCF 2.0中，有十个用于收集同意的“目的”（供应商可以对访客数据执行哪些操作）。 Adobe Audience Manager不要求所有十项，但除了供应商同意外，它仅要求用户出于以下目的同意：

* **用途1:** 在设备上存储和/或访问信息；
* **目的十：** 开发、改进产品；
* **特殊目的1:** 确保安全、防止欺诈和调试。

这是IAB TC字符串的第一部分，仅记录为1和0，指示该目的/活动是否获得批准。

>[!NOTE]
>
>根据IAB法规，特殊目的1（确保安全、防止欺诈和调试）始终获得同意，用户不能反对。

### 供应商 {#vendors}

IAB TC字符串的另一个部分是包含数百个供应商的长列表，以便向访客显示一个包含网站上标记的适用供应商列表，并可以选择要使用的供应商。 供应商在列表上保持着自己的位置。 例如，此列表上Adobe Audience Manager的供应商编号为565。 如果列表中的该数字具有“1”，则Audience Manager可以执行列表前面的已批准目的。 如果AAM竞价点具有“0”，则它无法对数据执行任何操作。

**要Audience Manager为客户提供UI以使用IAB TCF选择这些目的和供应商，或批准/不批准所有活动，您必须使用在IAB TCF中注册的CMP合作伙伴，或构建支持IAB TCF并在IAB TCF中注册的CMP合作伙伴。**

## 选择加入：在IAB和Adobe解决方案之间进行翻译 {#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的一个好处是，上述标准目的可能比Adobe解决方案列表让最终用户更了解他们正在批准的内容。 最终用户可能不知道“批准”Audience Manager或[!DNL Target]意味着什么，但是“在设备上存储和/或访问信息”或“开发和改进产品”可能更便于他们理解和同意。

为了获得批准(即，为了将IAB目的转换为选择加入对AAM进行“是”投票)，必须征得最终用户的同意，才能批准上述目的1和10。 如果其中任一项未获批准，或供应商未获批准，AAM将不执行像素触发或设置Cookie。 另外，很多客户只是选择为最终用户提供“全部或全部”UI，这当然会允许或禁止使用Audience Manager(和其他Experience Cloud解决方案)。

[文档](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html)中有一些关于适用于IAB TCF的Audience Manager插件流如何同时应用于发布者和广告商用例的重要信息。

## IAB:向下游发送同意 {#iab-sending-consent-downstream}

使用适用于IAB TCF的Audience Manager插件时，用户的同意选项还将发送到平台级别（第三方）ID同步，以便合作伙伴具有用户同意信息，并可以对其执行操作。 此信息以两个变量发送：

* gdpr = 1
* gdpr_consent = [已编码的同意字符串]

注意事项是，如果用户位于IAB上下文中并且未提供同意（或提供否定同意），则Audience Manager根本不会收集IAB TC字符串，因此会丢弃调用。 所以，在这种情况下，不要在下游通过同意。

## Demo {#demo}

在以下视频中，查看ECID和解决方案的Cookie和信标如何受IAB用户选择的影响。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

有关适用于IAB TCF 2.0的Audience Manager插件的更多详细信息（包括如何实施和测试、用例和工作流），请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
