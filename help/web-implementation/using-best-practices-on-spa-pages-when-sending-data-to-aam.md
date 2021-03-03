---
title: AAMにデータを送信する際のSPAページでのベストプラクティスの使用
description: このドキュメントでは、シングルページアプリ(SPA)からAdobe Audience Manager(AAM)にデータを送信する際に従い、注意が必要なベストプラクティスをいくつか説明します。 このドキュメントでは、Launch by Adobeの使用に重点を置いています。これは、推奨される実装方法です。
feature: 導入の基本
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: 「開発者、データ・エンジニア」
level: 経験豊富な
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}にデータを送信する際のSPAページでのベストプラクティスの使用

このドキュメントでは、[!UICONTROL Single Page Applications] (SPA)からAdobe Audience Manager(AAM)にデータを送信する際に従い、注意が必要なベストプラクティスをいくつか説明します。 このドキュメントでは、[!UICONTROL Experience Platform Launch]の使用に重点を置いており、これは導入方法として推奨されます。

## 初期メモ

* 以下の項目は、[!DNL Platform Launch]を使用してサイトに実装していることを前提としています。 [!DNL Platform Launch]を使用しない場合も、考慮事項は存在しますが、実装方法に合わせる必要があります。
* すべてのSPAは異なるので、お客様のニーズに最も合うように次の項目を調整する必要がある場合がありますが、ベストプラクティスをお客様と共有したいと思います。SPAページからAudience Managerにデータを送信する際に考慮する必要があること。

## Experience Platform Launch{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}でのSPAとAAMの操作の簡単な図

![～でのaamのスパ  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>これは、Adobe Audience Managerでの実装(Adobe Analyticsなし)でのSPAページの[!DNL Platform Launch]による処理方法を簡単に示した図です。 ご覧の通り、かなり率直で、表示の変化（あるいは行動）を[!DNL Platform Launch]に伝える方法が大きな決断です。

## SPAページ{#triggering-launch-from-the-spa-page}から[!DNL Launch]をトリガー

[!DNL Platform Launch]でルールをトリガーする(したがってAudience Managerにデータを送信する)ための、より一般的な2つの方法は次のとおりです。

* JavaScriptカスタムイベントの設定(例[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)をAdobe Analyticsと共に参照)
* [!UICONTROL Direct Call Rule]を使用

このAudience Managerの例では、[!DNL Launch]の[!UICONTROL Direct Call rule]を使用して、Audience Managerに入るヒットをトリガーします。 次の節で見るように、これは[!UICONTROL Data Layer]を新しい値に設定して[!DNL Platform Launch]の[!UICONTROL Data Element]で取り出せるようにすると、本当に役立ちます。

## デモページ{#demo-page}

SPAページで行うように、[!DNL data layer]内の値を変更し、AAMに送信する方法を示す小さなデモページを作成しました。 この機能は、必要に応じて、より複雑な変更をモデル化できます。 このデモページ[HERE](https://aam.enablementadobe.com/SPA-Launch.html)を見つけることができます。

## フォルダー特性の [!DNL data layer] {#setting-the-data-layer}

前述したように、新しいコンテンツがページに読み込まれたときやサイトで操作を行うときは、[!DNL data layer]をページの先頭に動的に設定して[!DNL Launch]が呼び出され[!UICONTROL rules]が実行され、[!DNL Platform Launch]が[!DNL data layer]から新しい値を取得してAudience Managerに送信できます。

上記のデモサイトにアクセスし、ページのソースを確認すると、次のように表示されます。

* [!DNL data layer]は、[!DNL Platform Launch]への呼び出しの前に、ページの先頭にあります。
* シミュレートされたSPAリンク内のJavaScriptは、[!UICONTROL Data Layer]を変更し、THENは[!DNL Platform Launch](_satellite.track()呼び出し)を呼び出します。 この[!UICONTROL Direct Call Rule]の代わりにJavaScriptのカスタムイベントを使用していた場合も、レッスンは同じです。 まず[!DNL data layer]を変更し、次に[!DNL Launch]を呼び出します。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## その他のリソース{#additional-resources}

* [AdobeフォーラムでのSPAの議論](https://forums.adobe.com/thread/2451022)
* [Launch by AdobeでのSPAの実装方法を示すリファレンスアーキテクチャサイト](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe AnalyticsでSPAを追跡する際のベストプラクティスの使用](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [この記事で使用するデモサイト](https://aam.enablementadobe.com/SPA-Launch.html)
