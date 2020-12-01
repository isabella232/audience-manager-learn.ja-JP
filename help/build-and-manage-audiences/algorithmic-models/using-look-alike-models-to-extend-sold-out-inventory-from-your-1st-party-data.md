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


# 「そっくり[!UICONTROL Models]」を使って[!UICONTROL First Party]データから売り切れ在庫を拡張する{#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

このチュートリアルでは、「類似オーディエンス」を設定して使用する際に必要な手順を説明します。これにより、新しい類似画像を作成し、コンバージョン[!UICONTROL Models]の拡張として販売できます。[!UICONTROL segment]

## 使用事例の詳細{#use-case-details}

コンテンツの投稿者である。 サイト上のコンバータの在庫がすでに売り切れている場合は、そこでオポチュニティが終わると考えられるかもしれません。 AAM’s Look-Alike [!UICONTROL Models]と入力します。 この機能を使用すると、売り切れた在庫をさらに拡大し、まだコンバージョンを行っていないが、コンバージョンを行ったユーザーのように見たり行動したりするオーディエンスを販売できます。 このオーディエンス[!UICONTROL segment]は通常、実際のコンバータより少ないコンバータで販売されますが、広告をサイトに配置する広告主に追加のオーディエンスオプションを提供することで、収益を増やすことができます。 この使用例のその他の利点は、ファーストパーティデータでこのモデルを実行するのに費用がかからないことです。

このチュートリアルの手順は次のとおりです。

1. 理想的なユーザー（コンバージョン）を特定/作成[!UICONTROL trait]または[!UICONTROL segment]
1. この変換[!UICONTROL trait]/[!UICONTROL segment]を基本項目として使用して[!UICONTROL model]を作成します
1. [!UICONTROL model]内の[!UICONTROL First party]データソースを選択し、[!UICONTROL model]を実行します
1. [!UICONTROL model]結果からアルゴリズム[!UICONTROL Trait]を作成し、[!UICONTROL trait]を[!UICONTROL segment]に追加します
1. [!UICONTROL segment]を関心のある広告主にオファーして、コンバージョン[!UICONTROL segment]の売上を拡張します。

## 理想的なユーザー（コンバージョン）を特定/作成[!UICONTROL trait]または[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上で人々に何をしてもらおうとしているか。 コンバージョンイベントは何ですか。 もちろん、サイトのタイプ/業種や組織の目標に応じて、この質問に対する答えは多く異なります。 いずれにせよ、AAMでは、これらの条件を満たした訪問者用に[!UICONTROL trait]を作成するのが一般的です。

この使用例では、コンバーターである人の在庫が売り切れているので、既にこれが想定されています。 ただし、このチュートリアルの目的上、残りの使用事例の参考として、この内容について説明することをお勧めします。

また、イベントを使って[!UICONTROL traits]を作成する際には、主な了解事項を考慮する必要があります。その結果、収集するユーザ数が[!UICONTROL trait]よりも多くなることはありません。 次のビデオで大きなリビールを確認してください。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、この例はAdobe Analyticsをお持ちの方を想定しています。明らかに、そうでないかもしれない。 Google Analytics(GA)をお持ちの場合は、データをAAMに送信する際に使用できるモジュール（[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)を参照）があり、サイト上のコンバージョンアクティビティがGAによってAAMに送信される場合は、それからコンバージョン特性を作成できます。 別の解析ソリューションがある（または解析ソリューションがない）場合でも、DILコードや`submit`関数などを使用してAAMにデータを送信できます。 （[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)を参照）。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されるデータに基づいて、コンバージョンの特性を再度作成します。

## [!UICONTROL First Party]データ{#creating-a-look-alike-model-from-first-party-data}から類似データ[!UICONTROL Model]を作成する

この手順では、[!UICONTROL First Party]類似した[!UICONTROL Model]を作成します。 つまり、[!UICONTROL first party]コンバージョン[!UICONTROL trait]/[!UICONTROL segment]を基盤とする[!UICONTROL trait]/[!UICONTROL segment]を使うだけでなく、[!UICONTROL models]データのプールを調べるだけで、コンバータに似た人を増やすことができます。 [!UICONTROL first party][!UICONTROL second party]や[!UICONTROL third party]のデータは見ません。

この使用事例では、コンバーターのように見えるがまだコンバージョンが行われていないユーザーの[!UICONTROL segment]を作成しようとしているので、この類似した[!UICONTROL segment]を関心のある広告主に販売できます。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## アルゴリズムの作成[!UICONTROL Trait] {#creating-an-algorithmic-trait}

次に、[!UICONTROL model]の結果を使用できるように、アルゴリズム[!UICONTROL Trait]を作成する必要があります。 [!UICONTROL trait]を作成しないと、[!UICONTROL model]は役に立ちません。 [!UICONTROL model]の実行後は、必ず[!UICONTROL trait]ダイアログを開き、アルゴリズム[!UICONTROL Trait]を作成してください。 次のビデオでは、このビデオの概要を説明し、いくつかのヒントを紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## アルゴリズム[!UICONTROL Segment]を広告主に提供する{#offering-the-algorithmic-segment-to-advertisers}

アルゴリズム[!UICONTROL Trait]を作成したら、新しい[!UICONTROL segment]を作成してデータをアクティブにできます([!UICONTROL trait]はアクティブにできませんが、新しい[!UICONTROL trait] [!UICONTROL segment]を作成してアルゴリズム[!UICONTROL Trait]をアクティブに（使用）できます)。[!UICONTROL segment]

[!UICONTROL model]のような見た目に高いスコアを持つ[!UICONTROL first party]訪問者の[!UICONTROL segment] （コンバータのように見えるがまだコンバージョンに至っていない）を作成したら、サイト上の実際のコンバータの在庫をすべて売り切った後でも、この[!UICONTROL segment]をサイト上の広告主にオファーできます。 これは、このオーディエンスを拡張し、Audience Managerの「類似[!UICONTROL Models]」を使用して追加の売上高を得続ける優れた方法です。
