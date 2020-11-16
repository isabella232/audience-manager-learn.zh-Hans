---
title: 全局设备ID验证
description: 设备广告标识符（即iDFA、GAID、Roku ID）具有必须满足的格式标准，才能在数字广告生态系统中使用。 现在，客户和合作伙伴可以以任何格式将ID上传到我们的全球数据源，而无需通知该ID是否格式正确。 此功能将引入对发送到全局数据源的设备ID的验证，以获得正确的格式，并在ID格式不正确时提供错误消息。 我们将支持iDFA、Google Advertising和Roku ID在启动时的验证。
feature: data governance & privacy
topics: mobile
audience: implementer, developer, architect
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---


# 全局设备ID验证 {#global-device-id-validation}

设备广告标识符（即iDFA、GAID、Roku ID）具有必须满足的格式标准，才能在数字广告生态系统中使用。 现在，客户和合作伙伴可以以任何格式将ID [!UICONTROL data sources] 上传到我们的Global，而不会收到ID格式是否正确的通知。 此功能将引入对发送到全局设备ID的验证，以 [!UICONTROL data sources] 获得正确的格式，并在ID格式不正确时提供错误消息。 我们支持验证 [!DNL iDFA], [!DNL Google Advertising] 并在 [!DNL Roku IDs] 启动时。

## 格式标准概述 {#overview-of-format-standards}

以下是AAM当前识别和支持的全局设备广告ID池。 这些功能以共享方式实 [!UICONTROL Data Sources] 现，共享方式可供与这些平台用户关联的数据一起使用的任何客户或数据合作伙伴使用。

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
   <td>32个十六进制数，通常以8-4-4-4-12<em>示例显示，97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>此ID必须在原始／未散列／未更改的表单引用中收集- <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32个十六进制数，通常以8-4-4-4-12 <em>示例显示，6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>此ID必须在原始／未散列／未更改的表单引用中收集- <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku(RIDA)</td>
   <td>121963</td>
   <td>32个十六进制数，通常以8-4-4-4-12 <em>示例</em> , <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>此ID必须在原始／未散列／未更改的表单引用中收集- <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID(MAID)</td>
   <td>389146</td>
   <td>字母数字字符串</td>
   <td>14593</td>
   <td>此ID必须在原始／未散列／未更改的表单引用中收集- <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">advertisingidhttps://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>字母数字字符串示例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>此ID必须在原始／未散列／未更改的表单引用中收集- <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## 在应用程序中设置广告标识符 {#setting-an-advertising-identifier-in-the-app}

在应用程序中设置广告商ID实际上是一个两步过程，首先检索广告商ID，然后将其发送给Experience Cloud。 下面提供了用于执行这些步骤的链接。

1. 检索ID
   1. [!DNL Apple] 有关此 [!DNL advertising ID] 项的信 [息](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. 有关为开发人员 [!DNL advertiser ID] 设置 [!DNL Android] 的一些信息，请 [在此处](http://www.androiddocs.com/google/play-services/id.html)。
1. 使用SDK中的方法 [!DNL setAdvertisingIdentifier] 将其发送到Experience Cloud
   1. 有关使用的 `setAdvertisingIdentifier` 信息，请 [参阅](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) （和） [!DNL iOS] 文档 [!DNL Android]。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## DCS错误消息传递错误ID  {#dcs-error-messaging-for-incorrect-ids}

当将错误的全局设备ID（IDFA、GAID等）实时提交到Audience Manager时，点击时将返回错误代码。 以下是返回的错误示例，因为ID以大写字 [!DNL Apple IDFA]母形式发送，它只应包含大写字母，而ID中有小写“x”。

![错误图像](assets/image_4_.png)

请参阅 [列表](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) ，以了解错误代码。

## 载入全局设备ID {#onboarding-global-device-ids}

除了实时提交全局设备ID外，您还可以[!DNL onboard]根据ID“上传”数据。 此过程与针对客户ID（通常通过键／值对）托管数据时相同，但您只需使用正确的数据源ID，以便将数据分配给全局设备ID。 有关入门过程的文档可在文档中 [找到](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)。 只需记住使用全 [!UICONTROL data source] 局ID，具体取决于您所使用的平台。

如果通过入门过程提交错误的全局设备ID，则错误将显示在中 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)。

以下是该报告可能出现的错误示例：

![错误图像](assets/image_5_.png)