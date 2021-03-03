---
title: 全局设备ID验证
description: 设备广告标识符（即iDFA、GAID、Roku ID）具有必须满足的格式化标准，才能在数字广告生态系统中使用。 现在，客户和合作伙伴可以以任何格式将ID上传到我们的全球数据源，而无需通知该ID是否格式正确。 此功能将对发送到全局数据源的设备ID进行验证，以获得正确的格式，并在ID格式不正确时提供错误消息。 我们将支持iDFA、Google Advertising和Roku ID在启动时的验证。
feature: “数据治理和隐私”
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: “开发人员、数据工程师、架构师”
level: 富有经验
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 1%

---


# 全局设备ID验证{#global-device-id-validation}

设备广告标识符（即iDFA、GAID、Roku ID）具有必须满足的格式化标准，才能在数字广告生态系统中使用。 现在，客户和合作伙伴可以以任何格式将ID上传到我们的Global [!UICONTROL data sources]，而不会收到ID格式是否正确的通知。 此功能将对发送到全局[!UICONTROL data sources]的设备ID进行验证，以获得正确的格式，并在ID格式不正确时提供错误消息。 我们支持在启动时对[!DNL iDFA]、[!DNL Google Advertising]和[!DNL Roku IDs]进行验证。

## 格式标准{#overview-of-format-standards}概述

以下是目前由AAM识别和支持的全局设备广告ID池。 这些功能以共享[!UICONTROL Data Sources]的形式实现，共享可用于与这些平台的用户关联的数据一起使用的任何客户或数据合作伙伴。

<table>
  <tr>
   <td>平台 </td>
   <td>AAM数据源ID </td>
   <td>ID格式 </td>
   <td>AAM PID </td>
   <td>注释 </td>
  </tr>
  <tr>
   <td>Google Android(GAID)</td>
   <td>20914</td>
   <td>32个十六进制数字，通常以8-4-4-4-12<em>示例，97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em>表示 </td>
   <td>1352</td>
   <td>此ID必须以原始/未哈希/未更改的表单引用收集 — <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32个十六进制数字，通常以8-4-4-4-12 <em>示例表示，6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>此ID必须以原始/未哈希/未更改的表单引用收集 — <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>罗库（里达）</td>
   <td>121963</td>
   <td>32个十六进制数，通常以8-4-4-4-12 <em>示例表示，</em> <em> fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>此ID必须以原始/未哈希/未更改的表单引用收集 — <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft广告ID(MAID)</td>
   <td>389146</td>
   <td>字母数字字符串</td>
   <td>14593</td>
   <td>此ID必须以原始/未哈希/未更改的表单引用收集 — <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>字母数字字符串示例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>此ID必须以原始/未哈希/未更改的表单引用收集 — <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## 在应用程序{#setting-an-advertising-identifier-in-the-app}中设置广告标识符

在应用程序中设置广告商ID实际上是一个两步过程，首先检索广告商ID，然后将其发送到Experience Cloud。 下面提供了执行这些步骤的链接。

1. 检索ID
   1. [!DNL Apple] 有关 [!DNL advertising ID] 的信 [息](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. 有关为[!DNL Android]开发人员设置[!DNL advertiser ID]的某些信息，可以找到[HERE](http://www.androiddocs.com/google/play-services/id.html)。
1. 使用SDK中的[!DNL setAdvertisingIdentifier]方法将其发送到Experience Cloud
   1. [文档](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier)中有关使用`setAdvertisingIdentifier`的信息，适用于[!DNL iOS]和[!DNL Android]。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## DCS错误消息，ID{#dcs-error-messaging-for-incorrect-ids}不正确

当将错误的全局设备ID（IDFA、GAID等）实时提交到Audience Manager时，点击时将返回错误代码。 以下是返回的错误示例，因为ID以[!DNL Apple IDFA]的形式发送，它应仅包含大写字母，而ID中却有小写“x”。

![错误图像](assets/image_4_.png)

有关错误代码的列表，请参见[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code)。

## 载入全局设备ID {#onboarding-global-device-ids}

除了实时提交全局设备ID外，您还可以针对ID“[!DNL onboard]”（上传）数据。 此过程与针对客户ID（通常通过键/值对）载入数据时相同，但您只需使用适当的数据源ID，以便将数据分配到全局设备ID。 有关入职过程的文档可在[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)中找到。 请记住使用全局[!UICONTROL data source] ID，具体取决于您所使用的平台。

如果通过入门过程提交了错误的全局设备ID，则[[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)中将显示错误。

以下是该报表中可能出现的错误示例：

![错误图像](assets/image_5_.png)