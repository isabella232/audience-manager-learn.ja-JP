---
title: AAMにデータを送信する際のSPAページでのベストプラクティスの使用
description: このドキュメントでは、シングルページアプリケーション(SPA)からAdobe Audience Manager(AAM)にデータを送信する際に従って注意する必要がある、いくつかのベストプラクティスについて説明します。 このドキュメントでは、推奨される実装方法であるLaunch by Adobeの使用に焦点を当てます。
feature: 実装の基本
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# AAMにデータを送信する際のSPAページでのベストプラクティスの使用 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

このドキュメントでは、[!UICONTROL Single Page Applications](SPA)からAdobe Audience Manager(AAM)にデータを送信する際に従って注意する必要がある、いくつかのベストプラクティスについて説明します。 このドキュメントでは、[!UICONTROL Experience Platform Launch]の使用に焦点を当てています。これは、推奨される実装方法です。

## 初期メモ

* 以下の項目は、サイトに[!DNL Platform Launch]を実装することを前提としています。 [!DNL Platform Launch]を使用しない場合でも、考慮事項は存在しますが、実装方法に合わせて調整する必要があります。
* すべてのSPAは異なるので、ニーズに合わせて以下の項目を調整する必要が生じる場合がありますが、ベストプラクティスをいくつかお客様と共有したいと考えています。SPAページからAudience Managerにデータを送信する際に考慮する必要がある事項。

## SPAとAAMのExperience Platform Launchでの簡単な操作図 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![aam用のspa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>前述のとおり、[!DNL Platform Launch]を使用した(Adobe Analyticsを使用しない)Adobe Audience Manager実装でのSPAページの処理方法を示す簡略図です。 ご覧の通り、大きな決断は[!DNL Platform Launch]にビューの変更（またはアクション）を伝える方法です。

## SPAページから[!DNL Launch]をトリガーする {#triggering-launch-from-the-spa-page}

[!DNL Platform Launch]でルールをトリガーする(したがって、データをAudience Managerに送信する)ための、より一般的な方法は次の2つです。

* JavaScriptカスタムイベントの設定(Adobe Analyticsを使用した[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)の例を参照)
* [!UICONTROL Direct Call Rule]の使用

このAudience Managerの例では、[!DNL Launch]で[!UICONTROL Direct Call rule]を使用して、Audience Managerに入るヒットをトリガーします。 次の節で説明するように、[!UICONTROL Data Layer]を新しい値に設定すると、[!DNL Platform Launch]の[!UICONTROL Data Element]で取得できるようになります。

## デモページ {#demo-page}

SPAページでおこなうように、 [!DNL data layer]の値を変更し、AAMに送信する方法を示す小さなデモページを作成しました。 この機能は、必要に応じて、より詳細な変更をモデル化できます。 このデモページ[HERE](https://aam.enablementadobe.com/SPA-Launch.html)を見つけることができます。

## フォルダー特性の [!DNL data layer] {#setting-the-data-layer}

前述のように、新しいコンテンツがページに読み込まれる場合やサイトで操作が実行される場合は、[!DNL data layer]をページの先頭に動的に設定してから[!DNL Launch]を呼び出し、[!UICONTROL rules]を実行し、[!DNL Platform Launch]が[!DNL data layer]から新しい値を取得してAudience Managerにプッシュします。

上記のデモサイトに移動し、ページのソースを見ると、次のようになります。

* [!DNL data layer]は、[!DNL Platform Launch]を呼び出す前に、ページの先頭に配置されています。
* シミュレートされたSPAリンク内のJavaScriptによって、 [!UICONTROL Data Layer]が変更され、THENが[!DNL Platform Launch] (_satellite.track()呼び出し)を呼び出します。 この[!UICONTROL Direct Call Rule]の代わりにJavaScriptのカスタムイベントを使用していた場合のレッスンは同じです。 まず[!DNL data layer]を変更し、次に[!DNL Launch]を呼び出します。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## その他のリソース {#additional-resources}

* [SPAフォーラムでのAdobe](https://forums.adobe.com/thread/2451022)
* [Launch by AdobeでのSPAの実装方法を示すリファレンスアーキテクチャのサイト](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe AnalyticsでSPAをトラッキングする際のベストプラクティスの使用](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [この記事で使用するデモサイト](https://aam.enablementadobe.com/SPA-Launch.html)
