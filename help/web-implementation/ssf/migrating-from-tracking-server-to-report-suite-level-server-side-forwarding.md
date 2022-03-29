---
title: トラッキングサーバーからレポートスイートレベルのサーバー側転送への移行
description: Adobe AnalyticsデータのAudience Managerへのサーバー側転送を、トラッキングサーバーレベルではなく、レポートスイートレベルで有効にする方法を説明します。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 1%

---

# トラッキングサーバーからレポートスイートレベルのサーバー側転送への移行 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

この記事とビデオでは、 [!DNL Analytics] データからAudience Managerへ [!UICONTROL report suite] ではなくレベル [!UICONTROL tracking server] レベル。

## はじめに {#introduction}

Adobe Audience Manager AND Adobe Analyticsを使用している場合、 [!DNL Analytics] データをAudience Managerに送信します。 つまり、ページが 2 つのヒット (1 つは [!DNL Analytics] (1 つはAudience Manager)、 [!DNL Analytics]、および [!DNL Analytics] がそのデータをAudience Managerに転送します。

既にこれを実行していて、2017 年 10 月より前に有効にして実装している場合、サーバー側転送は [!UICONTROL Tracking Server]:AdobeカスタマーケアまたはAdobeコンサルティングによって有効にされる必要がありました。 2017 年 10 月以降、サーバー側転送を自分で設定できるようになり、レポートスイートレベル（レポートスイートごとの転送）でおこなえるようになりました。 これには大きな利点がありますが、次に説明します。

## [!UICONTROL Tracking server] 転送 {#tracking-server-forwarding}

お使いの [!UICONTROL tracking server] は、 [!DNL Analytics] データ、およびイメージリクエストと cookie が書き込まれるドメインも含まれます。 DTM またはで設定する必要があります。 [!DNL Experience Platform Launch]または [!DNL AppMeasurement.js] ファイルで、通常は次のようになります。サイトまたはビジネス名は「mysite」に置き換えられます。

`s.trackingServer = "mysite.sc.omtrdc.net";`

サーバー側転送が [!UICONTROL tracking server] レベル、このに送信されるヒット [!UICONTROL tracking server] (Experience CloudID サービスも有効な場合 ) は、Audience Managerに転送されます。 これは、AdobeカスタマーケアまたはAdobeコンサルティングが有効にする必要がありました。 また、 [!UICONTROL report suite] 転送（以下で説明）

不明な場合は [!DNL tracking server forwarding] が有効になっている場合は、AdobeカスタマーケアまたはAdobeコンサルティングにお問い合わせください。お問い合わせいただければ、お知らせいたします。

## [!UICONTROL Report-suite] — レベルのサーバー側転送 {#report-suite-level-server-side-forwarding}

に移行する最大のメリットの 1 つ [!UICONTROL report suite] 転送 [!UICONTROL tracking server] 転送は、Audience Managerを転送する機能である「Audience Analytics」を使用できるようになります [!UICONTROL segments] Adobe Analyticsに戻ると、詳細なセグメント分析が可能です。 この優れた機能は、 [!UICONTROL tracking server] 転送と未転送 [!UICONTROL report suite] 転送中。 詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=ja).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要なヒント {#additional-resources}

上のビデオで述べたように、次の [!UICONTROL report suites] をAudience Managerに転送するには、AdobeカスタマーケアまたはAdobeコンサルティングに連絡し、 [!UICONTROL tracking server] 転送中。 これを行うのは緊急ではありません。 [!UICONTROL tracking server] 転送と [!UICONTROL report suite] 転送では、ヒットが重複しません。 ただし、 [!UICONTROL report suite] 転送中

もしあなたが出て行くなら [!UICONTROL tracking server] 転送は、次のデータを転送するだけでなく、 [!UICONTROL report suites] 転送は望まないが、将来、あなた（およびあなたの会社のすべての人）が [!UICONTROL tracking server] 転送がオンの場合、特定の [!UICONTROL report suite]. これは、レポートスイートレベルでこの機能がオンになっていないのに、 [!UICONTROL tracking server]. その後、転送の理由を見つけ出し、予期しなかったAAMサーバーコールに対する支払いもお金と時間を無駄にします。 だから、無効にするのが良い考えです [!UICONTROL tracking server] すべての [!UICONTROL report suites] は、お客様のビジネスニーズに合わせて適切な方法で進めるように設定します。
