---
title: 類似モデルを使用したファーストパーティデータからの売り切れ在庫の拡張
description: このチュートリアルでは、「類似モデル」を設定して使用するために必要な手順を順を追って説明します。これにより、類似した新しいオーディエンスを作成し、コンバージョンセグメントの拡張として販売できます。
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1688
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 0%

---


# 「そっくり」を使用 [!UICONTROL Models] して、売り切れた在庫を [!UICONTROL First Party] データから拡張する {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

このチュートリアルでは、「類似」の設定と使用に必要な手順を順を追って説明します。これにより、類似した新しいオーディエンスを作成し、コンバージョンの延長として販売できるようになり [!UICONTROL Models][!UICONTROL segment]ます。

## 使用事例の詳細 {#use-case-details}

コンテンツの投稿者である。 サイト上のコンバータの在庫がすでに売り切れている場合は、そこでオポチュニティが終わると考えられるかもしれません。 AAM’s Look-Alikeと入力し [!UICONTROL Models]ます。 この機能を使用すると、売り切れた在庫をさらに拡大し、まだコンバージョンを行っていないが、コンバージョンを行ったユーザーのように見たり行動したりするオーディエンスを販売できます。 通常、このオーディエンス [!UICONTROL segment] は実際のコンバータより少ないコンバータで販売されますが、広告をサイトに配置する広告主に追加のオーディエンスオプションを提供することで、収益を増やすことができます。 この使用例のその他の利点は、ファーストパーティデータでこのモデルを実行するのに費用がかからないことです。

このチュートリアルの手順は次のとおりです。

1. 理想的なユーザー（コンバージョン）を特定/作成 [!UICONTROL trait] する、または [!UICONTROL segment]
1. このコンバージョン [!UICONTROL model] / [!UICONTROL trait][!UICONTROL segment] を基本品目として使用して、を作成します
1. で [!UICONTROL First party][!UICONTROL model] データソースを選択し、 [!UICONTROL model]
1. 結果 [!UICONTROL Trait] からアルゴリズムを作成 [!UICONTROL model][!UICONTROL trait] し、 [!UICONTROL segment]
1. 関心のある広告主 [!UICONTROL segment] にオファーして、コンバージョン [!UICONTROL segment] の販売を拡張します。

## 理想的なユーザー（コンバージョン）を特定/作成 [!UICONTROL trait] する、または [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上で人々に何をしてもらおうとしているか。 コンバージョンイベントは何ですか。 もちろん、サイトのタイプ/業種や組織の目標に応じて、この質問に対する答えは多く異なります。 いずれにせよ、AAMでは、これらの条件を満たした訪問者 [!UICONTROL trait] 用のフォームを作成するのが一般的です。

この使用例では、コンバーターである人の在庫が売り切れているので、既にこれが想定されています。 ただし、このチュートリアルの目的上、残りの使用事例の参考として、この内容について説明することをお勧めします。

また、イベントを使用して作成する場合 [!UICONTROL traits]は、に必要な数より多くのユーザーを収集しないように、主な了解事項に留意する必要があり [!UICONTROL trait]ます。 次のビデオで大きなリビールを確認してください。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、例を見てみると、Adobe Analyticsがいると想定しています。 明らかに、そうでないかもしれない。 Google Analytics(GA)をお持ちの場合は、データをAAMに送信する際に使用できるモジュール( [ドキュメントを参照](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html))があり、サイト上のコンバージョンアクティビティがGAによってAAMに送信される場合は、そこからコンバージョン特性を作成できます。 別の解析ソリューションがある場合（または解析ソリューションがない場合）も、DILコードや `submit` 関数などを使用して、AAMにデータを送信できます。 ( [ドキュメントを参照](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html))。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されるデータに基づいて、コンバージョンの特性を再度作成します。

## デー [!UICONTROL Model][!UICONTROL First Party] タからそっくりなものを作成 {#creating-a-look-alike-model-from-first-party-data}

この手順では、「 [!UICONTROL First Party] 類似」を作成し [!UICONTROL Model]ます。 つまり、コンバージョン/を基本 [!UICONTROL first party] / [!UICONTROL trait](ほとんどの場合は通常のこ[!UICONTROL segment] とです [!UICONTROL trait])に使用するだけでなく、コンバータに似たより多くの人々のために、[!UICONTROL segment][!UICONTROL models][!UICONTROL first party] データのプールを調べるだけです。 我々は見るこ [!UICONTROL second party] とはない [!UICONTROL third party] 。

この使用事例では、このような用語は重要です。これは、コンバーターのように見えるがまだコンバージョンが行われていないユーザーをサイト上に作成しようとしているので、興味のある広告主にこの類似した用語 [!UICONTROL segment][!UICONTROL segment] を販売できるからです。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## アルゴリズムの作成 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

次に、アルゴリズムを作成し [!UICONTROL Trait]て、の結果を使用できるようにする必要 [!UICONTROL model] があります。 を作成しない [!UICONTROL trait]と、は無用 [!UICONTROL model] です。 実行後は、必ず [!UICONTROL model] ダイアログを開き、アルゴリズムを作成してくだ [!UICONTROL trait][!UICONTROL Trait]さい。 次のビデオでは、このビデオの概要を説明し、いくつかのヒントを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## アルゴリズムの広告主 [!UICONTROL Segment] への提供 {#offering-the-algorithmic-segment-to-advertisers}

アルゴリズムを作成したら、新しいアルゴリズムを作成し [!UICONTROL Trait]てデータをアクティブにできます(アクティブにすることはできません [!UICONTROL segment] が、アルゴリズムを使用して新しいアルゴリズムを作成し、アクティブに（使用）でき [!UICONTROL trait]るようにし[!UICONTROL trait][!UICONTROL segment][!UICONTROL Trait][!UICONTROL segment]ます)。

コンバーターのように見えてもまだコンバージョンに至っていないなど、ルックアライクで高いスコアを示す [!UICONTROL segment] 訪問者を作成したら [!UICONTROL first party][!UICONTROL model][!UICONTROL segment] (サイト上の実際のコンバーターの在庫をすべて売り切った後でも、このオファーをサイト上の広告主にできます。 これは、このオーディエンスを拡張し、Audience Managerの「類似」を使用して追加の売上高を得続ける優れた方法 [!UICONTROL Models] です。
