---
title: トラッキングサーバーからレポートスイートレベルのサーバー側転送への移行
description: この記事およびビデオでは、AnalyticsデータのAudience Managerへのサーバー側転送を、トラッキングサーバーレベルではなく、レポートスイートレベルで有効にする方法について説明します。
product: audience manager
feature: Adobe Analytics との統合
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 2%

---

# [!UICONTROL Tracking Server]から[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding]への移行 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

この記事とビデオでは、[!UICONTROL tracking server]レベルではなく[!UICONTROL report suite]レベルで[!DNL Analytics]データのAudience Managerを有効にする方法を説明します。[!UICONTROL server-side forwarding]

## はじめに {#introduction}

Adobe Audience ManagerとAdobe Analyticsがある場合は、[!DNL Analytics]データの「[!UICONTROL Server-side Forwarding]」をAudience Managerに実装できます。 つまり、ページが2つのヒット(1つは[!DNL Analytics]、もう1つはAudience Manager)を送信する代わりに、単に[!DNL Analytics]にヒットを送信するだけで、[!DNL Analytics]はそのデータをAudience Managerに転送します。 既にこの機能を実行し、2017年10月より前に有効化/実装している場合、[!UICONTROL server-side forwarding]は、AdobeカスタマーケアまたはAdobeコンサルティングが有効にする必要がある「[!UICONTROL Tracking Server]」に基づいている可能性があります。 2017年10月以降は、[!UICONTROL server-side forwarding]を自分で設定し、[!UICONTROL Report Suite]レベルで設定できるようになりました（転送は[!UICONTROL Report Suite]ごと）。 これには大きな利点があり、以下で説明します。

## [!UICONTROL Tracking Server] 転送 {#tracking-server-forwarding}

[!UICONTROL tracking server]は、[!DNL Analytics]データの送信先の場所であり、イメージリクエストとcookieが書き込まれるドメインでもあります。 DTMまたは[!DNL Experience Platform Launch]、または[!DNL AppMeasurement.js]ファイルで設定する必要があり、通常は次のようになります。サイトまたはビジネス名が「mysite」に置き換えられます。

`s.trackingServer = "mysite.sc.omtrdc.net";`

[!UICONTROL server-side forwarding]が[!UICONTROL tracking server]レベルで転送するように設定されている場合、この[!UICONTROL tracking server](Experience CloudIDサービスも有効な場合)に送信されるヒットがすべてAudience Managerに転送されます。 これは、AdobeカスタマーケアまたはAdobeコンサルティングが有効にする必要がありました。 [!UICONTROL report suite]転送に切り替えた後に、以下に説明するように、これを無効にすることもできます。

[!DNL tracking server forwarding]が有効になっているかどうかがわからない場合は、AdobeカスタマーケアまたはAdobeコンサルティングにお問い合わせください。

## [!UICONTROL Report Suite]-レベル [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

[!UICONTROL tracking server]転送から[!UICONTROL report suite]転送に移行する最大の利点の1つは、「Audience Analytics」を使用できるようになることです。これは、Audience Manager[!UICONTROL segments]を詳細な[!UICONTROL segment]分析のためにAdobe Analyticsに転送する機能です。 [!UICONTROL tracking server]転送中で[!UICONTROL report suite]転送中でない場合、この優れた機能はサポートされません。 Audience Analyticsの詳細については、[ドキュメント](https://marketing.adobe.com/resources/help/ja_JP/analytics/audiences/)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要なヒント {#additional-resources}

上記のビデオで述べたように、Audience Managerに転送する[!UICONTROL report suites]をすべて転送するように設定したら、AdobeカスタマーケアまたはAdobeコンサルティングに連絡し、[!UICONTROL tracking server]転送を無効にしてもらう必要があります。 [!UICONTROL tracking server]転送と[!UICONTROL report suite]転送の両方が重複するヒットを引き起こさないので、この操作は緊急の作業ではありません。 ただし、[!UICONTROL report suite]転送のみを行うことをお勧めします。 [!UICONTROL tracking server]転送をオンにした場合、[!UICONTROL report suites]からの転送は不要なデータを転送するだけでなく、[!UICONTROL tracking server]転送が有効になっているのを忘れた後で、（レポートスイートレベルでは有効になっていないので）[!UICONTROL report suite]のデータは転送中と思われます4/>. [!UICONTROL tracking server]その後、転送の理由を見つけ出すのに時間とお金を無駄にし、期待しなかったAAMサーバーコールにもお金を払うことになります。 したがって、ビジネスニーズに合わせて[!UICONTROL report suites]を転送するように設定したら、[!UICONTROL tracking server]転送をすぐに無効にすることをお勧めします。
