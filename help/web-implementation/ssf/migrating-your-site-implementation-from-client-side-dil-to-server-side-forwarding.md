---
title: 将站点的AAM实施从客户端DIL迁移到服务器端转发
description: 本教程适用于您(如果您同时拥有Adobe Audience Manager(AAM)和Adobe Analytics，并且您当前使用“DIL”(Data Integration Library)代码从页面发送点击到AAM，并且还从页面发送点击到Adobe Analytics。 由于您有这两种解决方案，而且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循打开“服务器端转发(SSF)”的最佳实践，这使Analytics数据收集服务器能够将站点分析数据实时转发给Audience Manager，而不是让客户端代码从页面向AAM再发送点击。 本教程将引导您完成从旧的“客户端DIL”实现切换到新的“服务器端转发”方法的步骤。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# 将站点的AAM实施从[!DNL Client-Side]DIL迁移到[!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

本教程适用于您(如果您同时拥有Adobe Audience Manager(AAM)和Adobe Analytics，并且您当前使用“DIL”([!DNL Data Integration Library])代码从页面发送点击到AAM，并且还从页面发送点击到Adobe Analytics。 由于您有这两种解决方案，而且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循打开“[!DNL Server-Side Forwarding](SSF)”的最佳实践，这使[!DNL Analytics]数据收集服务器能够将站点分析数据实时转发给Audience Manager，而不是让[!DNL client-side]代码从页面向AAM发送额外的点击。 本教程将指导您完成从旧“[!DNL Client-Side DIL]”实现到较新“[!DNL Server-Side forwarding]”方法的切换步骤。

## [!DNL Client-Side] (DIL)与  [!DNL Server-Side] {#client-side-dil-vs-server-side}

在比较和对比这两种将Adobe Analytics数据导入AAM的方法时，首先可以在以下图像中直观显示差异：

![客户端到服务器端](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL实施  {#client-side-dil-implementation}

如果您使用此方法将Adobe Analytics数据导入AAM，则意味着您的网页有两个点击：一个转到[!DNL Analytics]，一个转到AAM（在将[!DNL Analytics]数据复制到网页上后）。 [!UICONTROL Segments] 将从AAM返回到页面，在页面中可用于个性化等。这被视为“旧”实施，不再建议这样做。

除了不遵循最佳做法之外，使用此方法的缺点还包括：

* 页面中有两次点击，而不是只有一次
* [!UICONTROL Server-Side Forwarding] 要求实时共享AAM受众到，因此 [!DNL Analytics]实 [!DNL Client-side] 施不允许此功能（未来可能还有其他功能）

建议移至AAM实现的[!UICONTROL Server-Side Forwarding]方法。

### [!UICONTROL Server-Side Forwarding] 实施 {#server-side-forwarding-implementation}

如上图所示，点击从网页到Adobe Analytics。 [!DNL Analytics] 然后将该数据实时转发到AAM,访客将被评估为AAM [!UICONTROL traits] 和 [!UICONTROL segments]，就像点击直接来自页面一样。

[!UICONTROL Segments] 返回到的内容， [!DNL Analytics]将响应转发到网页进行个性化等。

转到服务器端转发没有时间限制。 我们强烈建议任何同时具有Audience Manager和[!DNL Analytics]的人都使用此实现方法。

## 您有两个主任务{#you-have-two-main-tasks}

这页上有很多信息，当然，这都很重要。 但是，它&#x200B;**归结起来是您需要做的两件主要事情：**

1. 将代码从[!DNL Client-Side]DIL代码更改为[!UICONTROL Server-Side Forwarding]代码
1. 在[!DNL Analytics] [!DNL Admin Console]中翻转交换机，开始数据的实际转发（每[!UICONTROL report suite]）

如果跳过这两个选项之一，SSF将无法正常工作。 此文档中已添加步骤和其他数据，以帮助您正确执行这两个步骤以完成设置。

## 实施选项{#implementation-options}

当您从[!DNL client-side]移动到[!DNL server-side]时，您将拥有的任务之一将代码更改为新的[!UICONTROL Server-Side Forwarding]代码。 使用以下选项之一完成此操作：

* Adobe Experience Platform Launch-我们建议的Web属性实施选项。 您将看到这是一个非常简单的任务，因为[!DNL Launch]已经帮您完成了所有困难的工作。
* 在页面上——如果您尚未（尚未）使用Adobe启动，还可以将新的SSF代码直接放入[!DNL appMeasurement.js]文件的`doPlugins`函数中
* 其他标签管理器——这些标签管理器可以像上一个（在页面上）选项一样处理，因为您仍将SSF代码放在`doPlugins`中，而其他标签管理器正在存储[!DNL AppMeasurement]代码

我们将在“更新代码”部分查看以下各项。

## 实施步骤 {#implementation-steps}

### 步骤0:入门项目：Experience CloudID服务(ECID){#step-prerequisite-experience-cloud-id-service-ecid}

迁移到[!UICONTROL Server-Side Forwarding]的主要先决条件是实现Experience CloudID服务。 如果您使用Experience Platform Launch，则最容易完成此操作，在这种情况下，您只需安装ECID扩展，它将完成其余的操作。

如果您使用的是非AdobeTMS，或根本没有TMS，则请实施ECID以在&#x200B;**之前运行**&#x200B;任何其他Adobe解决方案。 有关详细信息，请参阅[ECID文档](https://marketing.adobe.com/resources/help/zh_CN/mcvid/)。 唯一的其他先决条件是有关代码版本，因此，只需在以下步骤中应用代码的最新版本，您就可以了。

>[!NOTE]
>
>请在实施之前阅读整个文档。 下面的“Timing”部分包含有关&#x200B;*何时*&#x200B;应实施每个片段（包括ECID）的重要信息（如果尚未实现）。

### 第1步：从DIL代码{#step-record-currently-used-options-from-dil-code}记录当前使用的选项

当您准备好从[!DNL Client-Side]DIL代码移动到[!UICONTROL Server-Side Forwarding]时，第一步是识别您正在使用DIL代码执行的所有操作，包括自定义设置和发送到AAM的数据。 需要注意和考虑的事项包括：

* 正常[!DNL Analytics]变量，使用[!DNL siteCatalyst.init]DIL模块——您无需担心此变量，因为它的工作只是发送普通的[!DNL Analytics]变量，而只需启用SSF即可实现。
* Partner Subdomain —— 在DIL.create函数中，记下`partner`参数。 这称为您的“合作伙伴子域”，有时也称为“合作伙伴ID”，在您放置新的SSF代码时将需要它。
* [!DNL Visitor Service Namespace] -也称为“”[!DNL Org ID]或“[!DNL IMS Org ID]”，在设置新的SSF代码时，您也需要此代码。记下来。
* containerNSID、uuidCookie和其他高级选项——记下您正在使用的任何其他高级选项，以便您也可以在SSF代码中设置它们。
* 其他页面变量——如果从页面将其他变量发送到AAM（除了siteCatalyst.init处理的普通[!DNL Analytics]变量外），您需要记下这些变量，以便通过SSF发送它们(扰乱程序警报：通过[!DNL contextData]变量)。

### 第2步：更新代码{#step-updating-the-code}

在上面标题为“实施选项”的部分中，将提供多个关于如何／何处实施[!UICONTROL Server-Side Forwarding]的选项。 为了使本节有效，我们需要将其分解为以下几节（其中两节合并）。 转到本节最能描述您需求的方法。

#### Adobe Experience Platform Launch{#launch-by-adobe}

观看以下视频，了解如何将实施选项从[!DNL Client-Side]DIL代码移入Experience Platform Launch中的[!UICONTROL Server-Side Forwarding]。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### “On the Page”或非Adobe Tag Manager{#on-the-page-or-non-adobe-tag-manager}

观看以下视频，了解如何将实施选项从[!DNL Client-Side]DIL代码移入[!DNL AppMeasurement]代码中的[!UICONTROL Server-Side Forwarding]，该代码驻留在文件或非Adobe标签管理系统中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 第3步：启用转发（每[!UICONTROL Report Suite]）{#step-enabling-the-forwarding-per-report-suite}

在本教程中，我们始终将代码从[!DNL Client-Side DIL]代码切换到[!UICONTROL Server-Side Forwarding]。 这没关系，因为这是更困难的部分。 此部分虽然非常简单，但与更新代码同样重要。 在此视频中，您将看到如何打开交换机，以实际将数据从Analytics转发到Audience Manager。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如视频中所述，要在Experience Cloud后端完全实现转发，需要4个小时的时间。

## 计时{#timing}

作为提醒，有两个主要任务用于从[!DNL Client-Side DIL]移至[!UICONTROL Server-Side Forwarding]:

1. 更新代码
1. 在[!DNL Analytics] [!DNL Admin Console]中翻转开关

但问题是，你先做哪个？ 重要吗？ 好，抱歉，那是两个问题。 但答案是……这取决于情况，是的，它&#x200B;*可能*&#x200B;很重要。 这样很模糊？ 让我们将其分解。 但首先，如果您是拥有大量站点的大型组织，还会出现一个额外的问题：我必须立刻做所有事吗？ 那个比较容易。 不。 你可以一个接一个地做。:)

### 更深入的潜水{#a-little-deeper-dive}

时间和顺序问题的原因在于转发的实际工作方式，这可以概括为以下几个技术事实：

* 如果已实现Experience CloudID服务(ECID)，且[!DNL Analytics] [!DNL Admin Console](“switch”)中的交换机已打开，则数据将从[!DNL Analytics]转发至AAM，即使您尚未更新代码。
* 如果未实现ECID，则数据将不会转发，即使您已打开开关并且具有SSF代码。
* SSF代码（无论是在[!DNL Launch]中还是在页面上）确实会处理响应，当然，完成迁移是必需的。
* 请记住，SSF交换机由[!UICONTROL Report Suite]启用，但代码由[!DNL Launch]中的属性处理，或者由[!DNL AppMeasurement]文件处理（如果未使用[!DNL Launch]）

### 最佳实践 {#best-practices}

根据这些技术细节，以下建议了“何时做什么”的时间安排：

#### 如果尚未实现ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 为SSF启用的每个[!UICONTROL report suite]打开[!DNL Analytics]中的交换机

   1. 转发尚未开始，因为您没有ECID

1. 对于每个站点，将您的代码从[!DNL Client-Side DIL]更新到SSF（该代码可能位于[!DNL Launch]中或页面上，如上面另一节所述）

   1. 转发现在将进行（因为您已添加了ECID），您还应收到对[!DNL Analytics]信标的正确JSON响应（有关详细信息，请参阅下面的验证和疑难解答部分）

#### 如果您已实现ECID {#if-you-do-have-ecid-implemented}

1. 准备并计划，以便您准备好将代码从DIL更新为SSF PER [!UICONTROL report suite]，您将为SSF启用该代码：

   1. 在[!DNL Analytics]中翻转交换机以启用SSF

      1. 转发WILL开始，因为您已启用ECID
   1. 尽快将您的代码从[!DNL Client-Side DIL]更新到SSF（该代码可能位于[!DNL Launch]中或页面上，如上面另一节所述）

      1. 您应收到对[!DNL Analytics]信标的正确JSON响应（有关更多详细信息，请参阅下面的验证和疑难解答部分）


**注意1:** 这两个步骤必须尽可能接近，因为在以上步骤1和步骤2之间，您将有数据传入AAM。换言之，SSF将开始从[!DNL Analytics]向AAM发送数据，由于DIL代码仍在页面上，因此也会有从页面直接进入AAM的点击，从而使数据翻倍。 一旦将代码从DIL更新到SSF，这将得到缓解。

**注意2:** 如果您希望在数据方面存在小的差异，而不是少量重复数据，可以切换上面步骤1和2的顺序。将代码从DIL移到SSF将停止数据流到AAM，直到您能够翻转交换机来打开[!UICONTROL report suite]的SSF。 通常，客户更希望将访客数据量翻一番，而不是错过将数据导入[!UICONTROL traits]和[!UICONTROL segments]的机会。

#### 具有多个站点和[!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}的迁移时间

在前几节中，本主题将作一简要介绍，主要战略可概括如下：

一次迁移一个站点/[!UICONTROL report suite]（或一组站点/[!UICONTROL report suites]）。

但是，根据一些可能的情况，这可能会变得有些棘手：

* 您有一个站点，其中包含几个不同的[!UICONTROL report suites]
* 您有一个[!UICONTROL report suite]，它包含多个站点（如全局[!UICONTROL report suite]）
* 使用一个[!DNL Launch]属性覆盖多个站点
* 您拥有针对不同站点的不同开发团队

由于这些项目，它会变得有点复杂。 我最能建议的是：

* 根据上述说明制定迁移到SSF的策略需要一些时间
* 根据[!DNL Launch]（或单个[!DNL AppMeasurement]文件）中的单个属性通常映射到1或2个不同的[!UICONTROL report suites]，您可能能够逐个针对这些不同的组制定计划，将您的企业更新到SSF
* 如果您与Adobe咨询部门合作，请与他们讨论您的迁移计划，以便他们能够根据需要提供帮助

## 验证和疑难解答{#validation-and-troubleshooting}

验证[!UICONTROL Server-Side Forwarding]是否已启动并正在运行的主要方法是查看您对来自应用程序的任何Adobe Analytics点击的响应。

如果您没有从[!DNL Analytics]到Audience Manager的[!UICONTROL server-side forwarding]数据，则对[!DNL Analytics]信标（除2x2像素外）确实没有响应。 但是，如果您正在执行SSF，则有一些项目可以在[!DNL Analytics]请求和响应中进行验证，这样您会知道[!DNL Analytics]正在与Audience Manager正确通信、转发点击并获得响应。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 谨防错误的“成功”-如果有响应并且一切都正常，请确保响应中包含“stuff”对象。如果不这样做，您可能会看到一条消息，说明[!DNL "status":"SUCCESS"]。 尽管这听起来很疯狂，但这实际上是验证，它不能正确工作。 如果您看到这一点，则表示您已完成[!DNL Launch]或[!DNL AppMeasurement]中的代码更新，但[!DNL Analytics] [!DNL Admin Console]中的转发尚未完成。 在这种情况下，您需要验证是否已为[!UICONTROL report suite]的[!DNL Analytics] [!DNL Admin Console]启用SSF。 如果您已经做过，而且还没有4小时，请耐心等待，因为可能需要这么长时间才能在后端进行所有必要的更改。

![虚假成功](assets/falsesuccess.png)

有关[!UICONTROL Server-Side Forwarding]的详细信息，请参见[文档](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)。
