---
title: 将站点的AAM实施从客户端DIL迁移到服务器端转发
description: 如果您同时拥有Adobe Audience Manager(AAM)和Adobe Analytics，并且您当前正在使用“DIL”(Data Integration Library)代码将点击从页面发送到AAM，并且还从页面将点击从页面发送到Adobe Analytics，则本教程适用于您。 由于您有这两款解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您可以遵循打开“服务器端转发(SSF)”的最佳实践，这使Analytics数据收集服务器能够将站点分析数据实时转发到Audience Manager，而不是让客户端代码从页面向AAM发送额外的点击。 本教程将引导您完成从旧的“客户端DIL”实现切换到较新的“服务器端转发”方法的步骤。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: "Developer, Data Engineer"
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
translation-type: tm+mt
source-git-commit: 256edb05f68221550cae2ef7edaa70953513e1d4
workflow-type: tm+mt
source-wordcount: '2322'
ht-degree: 0%

---

# 将您网站的AAM实施从[!DNL Client-Side]DIL迁移到[!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同时具有Adobe Audience Manager(AAM)和Adobe Analytics，并且您当前使用“DIL”([!DNL Data Integration Library])代码从页面向AAM发送点击，并且还从页面向Adobe Analytics发送点击，则本教程将适用于您。 由于您有这两种解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循打开“[!DNL Server-Side Forwarding](SSF)”的最佳实践，这使[!DNL Analytics]数据收集服务器能够将站点分析数据实时转发到Audience Manager，而不是让[!DNL client-side]代码从页面向AAM发送额外的点击。 本教程将指导您完成从旧的“[!DNL Client-Side DIL]”实现切换到较新的“[!DNL Server-Side forwarding]”方法的步骤。

## [!DNL Client-Side] (DIL)与  [!DNL Server-Side] {#client-side-dil-vs-server-side}

在比较和对比将Adobe Analytics数据导入AAM的两种方法时，首先可以直观地显示以下图像中的差异：

![客户端到服务器端](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL实施  {#client-side-dil-implementation}

如果您使用此方法将Adobe Analytics数据导入AAM，则意味着您的网页有两个点击：一个转到[!DNL Analytics]，一个转到AAM（在将[!DNL Analytics]数据复制到网页上后）。 [!UICONTROL Segments] 从AAM返回到页面，在页面中可用于个性化等。这被视为“旧式”实施，不再建议这样做。

除了不遵循最佳做法之外，使用此方法的缺点还包括：

* 页面点击量为2次，而非仅点击1次
* [!UICONTROL Server-Side Forwarding] 是将AAM受众实时共享到所必需的，因 [!DNL Analytics]此实 [!DNL Client-side] 施不允许使用此功能（以及将来可能使用的其他功能）

建议移至AAM实现的[!UICONTROL Server-Side Forwarding]方法。

### [!UICONTROL Server-Side Forwarding] 实施 {#server-side-forwarding-implementation}

如上图所示，点击从网页到Adobe Analytics。 [!DNL Analytics] 然后将该数据实时转发到AAM,访客会被评估为AAM [!UICONTROL traits] 和 [!UICONTROL segments]，就像点击直接来自页面一样。

[!UICONTROL Segments] 返回到，将响应转 [!DNL Analytics]发到网页进行个性化等。

移动到服务器端转发没有时间方面的缺点。 我们强烈建议任何同时具有Audience Manager和[!DNL Analytics]的用户都使用此实现方法。

## 您有两个主任务{#you-have-two-main-tasks}

这页面上有很多信息，当然，这都很重要。 但是，它&#x200B;**归结为您需要做的两件主要事情：**

1. 将您的代码从[!DNL Client-Side]DIL代码更改为[!UICONTROL Server-Side Forwarding]代码
1. 翻转[!DNL Analytics] [!DNL Admin Console]中的交换机以开始数据的实际转发（每[!UICONTROL report suite]）

如果您跳过其中任一步骤，SSF将无法正常工作。 此文档中已添加了步骤和其他数据，以帮助您针对设置正确执行这两个步骤。

## 实施选项{#implementation-options}

当您从[!DNL client-side]移动到[!DNL server-side]时，您将拥有的任务之一是将代码更改为新的[!UICONTROL Server-Side Forwarding]代码。 使用以下选项之一完成此操作：

* Adobe Experience Platform Launch — 我们建议的Web属性实施选项。 您会发现这是一个非常简单的任务，因为[!DNL Launch]已经为您完成了所有的艰难工作。
* 在页面上 — 如果您尚未使用Adobe Launch，也可以将新的SSF代码直接放入[!DNL appMeasurement.js]文件的`doPlugins`函数中
* 其他标签管理器 — 这些标签管理器可以像上一个（在页面上）选项一样处理，因为您仍将将SSF代码放在`doPlugins`中，而其他标签管理器存储[!DNL AppMeasurement]代码

我们将在“更新代码”部分查看以下各项。

## 实施步骤 {#implementation-steps}

### 步骤0:先决条件：Experience CloudID服务(ECID){#step-prerequisite-experience-cloud-id-service-ecid}

迁移到[!UICONTROL Server-Side Forwarding]的主要先决条件是实现Experience CloudID服务。 如果您使用Experience Platform Launch，则最容易完成此操作，在这种情况下，您只需安装ECID扩展，它就会完成其余的操作。

如果您使用的是非AdobeTMS，或根本没有TMS，则请实施ECID以在&#x200B;**之前运行**&#x200B;任何其他Adobe解决方案。 有关详细信息，请参阅[ECID文档](https://marketing.adobe.com/resources/help/zh_CN/mcvid/)。 唯一的其他先决条件是与代码版本有关，因此只需在以下步骤中应用代码的最新版本，您就可以了。

>[!NOTE]
>
>请在实施之前阅读此文档。 下面的“计时”部分包含有关&#x200B;*何时*&#x200B;应实施每个片段（包括ECID）的重要信息（如果尚未实施）。

### 第1步：从DIL代码{#step-record-currently-used-options-from-dil-code}记录当前使用的选项

在您准备从[!DNL Client-Side]DIL代码移动到[!UICONTROL Server-Side Forwarding]时，第一步是识别您正在对DIL代码执行的所有操作，包括自定义设置和发送到AAM的数据。 需要注意和考虑的事项包括：

* 使用[!DNL siteCatalyst.init]DIL模块的常规[!DNL Analytics]变量 — 您无需担心此变量，因为它的工作只是发送普通的[!DNL Analytics]变量，而只需启用SSF即可实现。
* Partner Subdomain — 在DIL.create函数中，记下`partner`参数。 这称为您的“合作伙伴子域”，有时称为“合作伙伴ID”，在您放置新的SSF代码时将需要它。
* [!DNL Visitor Service Namespace]  — 也称为“”[!DNL Org ID]或“[!DNL IMS Org ID]”，在设置新的SSF代码时，您也需要此代码。记下来。
* containerNSID、uuidCookie和其他高级选项 — 记下您使用的任何其他高级选项，以便您也可以在SSF代码中设置它们。
* 其他页面变量 — 如果从页面将其他变量发送到AAM（除了由siteCatalyst.init处理的常规[!DNL Analytics]变量外），您需要记下这些变量，以便通过SSF(扰乱程序警报：)。[!DNL contextData]

### 第2步：更新代码{#step-updating-the-code}

在上面标题为“实施选项”的部分中，提供了有关如何/何处实施[!UICONTROL Server-Side Forwarding]的多个选项。 为了使本部分有效，我们需要将其分为以下几部分（其中两部分合并）。 转到最适合您需求的本节方法。

#### Adobe Experience Platform Launch {#launch-by-adobe}

观看以下视频，了解如何将实施选项从[!DNL Client-Side]DIL代码移入Experience Platform Launch中的[!UICONTROL Server-Side Forwarding]。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;On the Page&quot; or Non-Adobe Tag Manager {#on-the-page-or-non-adobe-tag-manager}

观看以下视频，了解如何将实施选项从[!DNL Client-Side]DIL代码移入[!DNL AppMeasurement]代码中的[!UICONTROL Server-Side Forwarding](位于文件或非Adobe标签管理系统中)。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 第3步：启用转发（每[!UICONTROL Report Suite]）{#step-enabling-the-forwarding-per-report-suite}

在本教程中，到目前为止，我们一直在将代码从[!DNL Client-Side DIL]代码切换到[!UICONTROL Server-Side Forwarding]。 这没关系，因为这是更困难的部分。 此部分虽然非常简单，但与更新代码一样重要。 在此视频中，您将了解如何切换开关，以启用从Analytics到Audience Manager的数据实际转发。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如视频中所述，要在Experience Cloud后端上完全实现转发功能，最多需要4小时。

## 计时{#timing}

作为提醒，有两个主要任务可从[!DNL Client-Side DIL]移至[!UICONTROL Server-Side Forwarding]:

1. 更新代码
1. 在[!DNL Analytics] [!DNL Admin Console]中翻转开关

但问题是，你先做哪个？ 重要吗？ 好吧，抱歉，那是两个问题。 但答案……这取决于情况，是的，它&#x200B;*可能*&#x200B;很重要。 这个词怎么说？ 我们把它拆了。 但首先，如果您是拥有大量站点的大型组织，还会出现另外一个问题：我必须立刻做所有事吗？ 那个比较容易。 不。 你可以一个接一个地做，差不多。:)

### 更深入的潜{#a-little-deeper-dive}

时间和顺序之所以如此重要，是因为转发*实际上*工作方式，可以用以下几个技术事实概括：

* 如果您已实现Experience CloudID服务(ECID)，且[!DNL Analytics] [!DNL Admin Console](&quot;switch&quot;)中的交换机已打开，则数据将从[!DNL Analytics]转发到AAM，即使您尚未更新代码。
* 如果尚未实现ECID，则即使您已打开交换机并且具有SSF代码，数据也将无法转发。
* SSF代码（无论是在[!DNL Launch]中还是在页面上）实际处理响应，当然，完成迁移是必需的。
* 请记住，SSF交换机由[!UICONTROL Report Suite]启用，但代码由[!DNL Launch]中的属性处理，或由[!DNL AppMeasurement]文件处理（如果您不使用[!DNL Launch]）

### 最佳实践 {#best-practices}

根据这些技术细节，以下建议了&quot;何时做什么&quot;的时机：

#### 如果尚未实现ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 为SSF启用的每个[!UICONTROL report suite]在[!DNL Analytics]中翻转交换机

   1. 转发尚未开始，因为您没有ECID

1. 对于每个站点，将您的代码从[!DNL Client-Side DIL]更新到SSF（该代码可能位于[!DNL Launch]中，或在页面上，如上面另一节所述）

   1. 转发现在将进行（因为您已添加了ECID），您还应收到对[!DNL Analytics]信标的正确JSON响应（有关详细信息，请参阅下面的验证和疑难解答部分）

#### 如果ECID已实现{#if-you-do-have-ecid-implemented}

1. 准备并计划，以便您准备好将您的代码从DIL更新为SSF PER [!UICONTROL report suite]，您将为SSF启用该代码：

   1. 在[!DNL Analytics]中翻转交换机以启用SSF

      1. 转发将开始，因为您已启用ECID
   1. 请尽快将您的代码从[!DNL Client-Side DIL]更新到SSF（该代码可能位于[!DNL Launch]中，或在页面上，如上面另一节所述）

      1. 您应收到对[!DNL Analytics]信标的正确JSON响应（有关更多详细信息，请参阅下面的验证和疑难解答部分）


**注意1:** 这两个步骤应尽可能接近，因为在以上步骤1和步骤2之间，您将有进入AAM的重复数据。换句话说，SSF将开始从[!DNL Analytics]向AAM发送DIL，由于页面上仍有代码，因此也会有从页面直接到AAM的点击，从而使数据翻倍。 一旦将代码从DIL更新到SSF，这将得到缓解。

**注意2:** 如果您希望在数据方面略有差异，而不是少量重复数据，可以切换上面步骤1和2的顺序。将代码从DIL移到SSF将停止数据流进AAM，直到您能够翻转交换机以打开[!UICONTROL report suite]的SSF。 通常，客户宁愿将访客数据翻一番，也不愿错过将数据导入[!UICONTROL traits]和[!UICONTROL segments]的过程。

#### 有多个站点和[!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}时的迁移时间

在前几节中，本主题有简要的介绍，主要战略可以概括如下：

一次迁移一个站点/[!UICONTROL report suite]（或一组站点/[!UICONTROL report suites]）。

但是，基于一些可能的情况，这可能会有些棘手：

* 您的站点包含多个不同的[!UICONTROL report suites]
* 您有一个[!UICONTROL report suite]，它包含多个站点（如全局[!UICONTROL report suite]）
* 使用一个[!DNL Launch]属性覆盖多个站点
* 您拥有针对不同站点的不同开发团队

由于这些项目，它可能会变得有点复杂。 我最能建议的是：

* 根据上述说明制定迁移到SSF的策略需要一些时间
* 根据[!DNL Launch]（或单个[!DNL AppMeasurement]文件）中的单个属性通常映射到1或2个不同的[!UICONTROL report suites]，您可能可以逐个制定一个计划，将您的企业更新到SSF
* 如果您与Adobe Consulting合作，请与他们讨论您的迁移计划，以便他们能够根据需要提供帮助

## {#validation-and-troubleshooting}验证和疑难解答

验证[!UICONTROL Server-Side Forwarding]是否已启动并正在运行的主要方法是查看对来自应用程序的任何Adobe Analytics点击的响应。

如果您没有从[!DNL Analytics]到Audience Manager的[!UICONTROL server-side forwarding]数据，则对[!DNL Analytics]信标（除2x2像素外）确实没有响应。 但是，如果您正在执行SSF，则有些项目可以在[!DNL Analytics]请求和响应中进行验证，这些项目会让您知道[!DNL Analytics]与Audience Manager正确通信、转发点击并获得响应。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 提防虚假的“成功” — 如果有响应，且一切似乎都正常，请确保响应中包含“stuff”对象。如果不这样做，您可能会看到一条消息说明[!DNL "status":"SUCCESS"]。 尽管这听起来很疯狂，但这实际上是验证，它不能正确工作。 如果您看到这一点，则表示您已完成[!DNL Launch]或[!DNL AppMeasurement]中的代码更新，但[!DNL Analytics] [!DNL Admin Console]中的转发尚未完成。 在这种情况下，您需要验证您是否已为[!UICONTROL report suite]在[!DNL Analytics] [!DNL Admin Console]中启用了SSF。 如果您已经做过了，而且还没有4小时，请耐心等待，因为在后端进行所有必要的更改可能需要这么长时间。

![虚假成功](assets/falsesuccess.png)

有关[!UICONTROL Server-Side Forwarding]的详细信息，请参阅[文档](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)。
