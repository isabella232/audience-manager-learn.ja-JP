---
title: AAMにデータを送信する際のSPAページでのベストプラクティスの使用
description: このドキュメントでは、シングルページアプリ(SPA)からAdobe Audience Manager(AAM)にデータを送信する際に従い、注意が必要なベストプラクティスをいくつか説明します。 このドキュメントでは、Launch by Adobeの使用に重点を置いています。これは、推奨される実装方法です。
feature: implementation basics
topics: spa
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---


# AAMにデータを送信する際のSPAページでのベストプラクティスの使用 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

このドキュメントでは、 [!UICONTROL Single Page Applications] (SPA)からAdobe Audience Manager(AAM)にデータを送信する際に従い、注意が必要なベストプラクティスをいくつか説明します。 このドキュメントでは、の使用に重点を置い [!UICONTROL Experience Platform Launch]ており、これが推奨される実装方法です。

## 初期メモ

* 以下の項目は、を使用してサイトに実装していることを前提 [!DNL Platform Launch] としています。 を使用しない場合も考慮事項が存在しますが、実装方法に合わせ [!DNL Platform Launch]る必要があります。
* すべてのSPAは異なるので、お客様のニーズに最も合うように次の項目を調整する必要がある場合がありますが、ベストプラクティスをお客様と共有したいと思います。spaページからAudience Managerにデータを送信する際に考慮する必要があること。

## Experience Platform LaunchでのSPAとAAMの操作の簡単な図 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![～でのaamのスパ [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>これは、Adobe Audience Managerでの(Adobe Analyticsを除く)導入でのSPAページの処理方法を簡単に示した図 [!DNL Platform Launch]です。 ご覧の通り、かなり率直ですが、大きな決断は表示の変化（あるいは行動）をどのように伝えるかで [!DNL Platform Launch]す。

## SPAページ [!DNL Launch] からのトリガー {#triggering-launch-from-the-spa-page}

でルールをトリガーするための一般的な方法は、次の2つ [!DNL Platform Launch] です(したがって、Audience Managerにデータを送信する方法)。

* JavaScriptカスタムイベントの設定( [ここでは](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) 「Adobe Analyticsを使用する」を参照)
* を使用する [!UICONTROL Direct Call Rule]

このAudience Managerの例では、Audience Managerにヒットが発生するトリガー [!UICONTROL Direct Call rule] にinを使用 [!DNL Launch] します。 次の節で説明するように、を新しい値に設定して、で値を取得でき [!UICONTROL Data Layer] るようにすると、これは非常に役立ち [!UICONTROL Data Element][!DNL Platform Launch]ます。

## デモページ {#demo-page}

SPAページで行うように、での値の変更 [!DNL data layer] とAAMへの値の送信を示す小さなデモページを作成しました。 この機能は、必要に応じて、より複雑な変更をモデル化できます。 このデモページは [こちら](https://aam.enablementadobe.com/SPA-Launch.html)。

## フォルダー特性の [!DNL data layer] {#setting-the-data-layer}

前述したように、新しいコンテンツがページに読み込まれたとき、または誰かがサイトでアクションを実行したときに、BEFOREページの先頭に動的に設定する [!DNL data layer] 必要があります。これにより、の新しい値を取得してAudience Manager [!DNL Launch] に [!UICONTROL rules][!DNL Platform Launch][!DNL data layer] プッシュできます。

上記のデモサイトにアクセスし、ページのソースを確認すると、次のように表示されます。

* は、ページ [!DNL data layer] の先頭にあり、 [!DNL Platform Launch]
* シミュレーションされたSPAリンク内のJavaScriptによって、の呼び出し [!UICONTROL Data Layer]と、の呼び出し [!DNL Platform Launch] (_satellite.track()の呼び出し)が変更されます。 この代わりにJavaScriptのカスタムイベントを使用していた場合 [!UICONTROL Direct Call Rule]も、このレッスンは同じです。 最初にを変更 [!DNL data layer]し、次にを呼び出し [!DNL Launch]ます。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## その他のリソース{#additional-resources}

* [ADOBEフォーラムでのSPAの議論](https://forums.adobe.com/thread/2451022)
* [Launch by AdobeでのSPAの実装方法を示すリファレンスアーキテクチャサイト](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe AnalyticsでSPAを追跡する際のベストプラクティスの使用](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [この記事で使用するデモサイト](https://aam.enablementadobe.com/SPA-Launch.html)
