---
title: 類似(look-alike)モデルを使用して、ファーストパーティデータから販売済みの在庫を拡張する
description: このチュートリアルでは、類似(look-alike)モデルを設定して使用するための手順を順を追って説明します。これにより、類似(look-alike)オーディエンスを新たに作成し、コンバージョンセグメントの拡張機能として販売できます。
feature: アルゴリズムモデル
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 0%

---

# 類似(look-alike) [!UICONTROL Models]を使用して、[!UICONTROL First Party]データから売り切れ在庫を拡張する {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

このチュートリアルでは、類似(look-alike) [!UICONTROL Models]を設定して使用するための手順を順を追って説明します。これにより、類似(look-alike)オーディエンスを新たに作成し、コンバージョン[!UICONTROL segment]の拡張機能として販売できます。

## 使用例の詳細 {#use-case-details}

コンテンツの投稿者である。 サイト上のコンバーターの在庫を既に売り切れている場合は、その機会が終わると考えるかもしれません。 AAM Look-Alike [!UICONTROL Models]と入力します。 この機能を使用すると、売り切れの在庫をさらに拡張し、まだコンバージョンに至っていないが、コンバージョンに至った人のように見えたり行動したりする人のオーディエンスを販売することができます。 このオーディエンス[!UICONTROL segment]は、通常、実際のコンバーターよりも少ない価格で販売されますが、サイトに広告を配置する広告主に対して、追加のオーディエンスオプションを提供することで、を最終的に追加できます。 この使用例のその他の利点は、ファーストパーティデータでこのモデルを実行するのに費用がかからないことです。

このチュートリアルの手順は次のとおりです。

1. 理想的なユーザー（コンバージョン）を特定/作成する[!UICONTROL trait]または[!UICONTROL segment]
1. この変換[!UICONTROL trait]/[!UICONTROL segment]を基本項目として使用して[!UICONTROL model]を作成します
1. [!UICONTROL model]で[!UICONTROL First party]データソースを選択し、[!UICONTROL model]を実行します
1. [!UICONTROL model]結果からアルゴリズム[!UICONTROL Trait]を作成し、[!UICONTROL trait]を[!UICONTROL segment]に追加します。
1. 関心のある広告主に[!UICONTROL segment]を提供して、コンバージョン[!UICONTROL segment]の売上を拡張する

## 理想的なユーザー（コンバージョン）を特定/作成する[!UICONTROL trait]または[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上でユーザーに何をしてもらおうとしていますか。 コンバージョンイベントの種類 もちろん、サイトのタイプ/垂直方式や組織の目標に応じて、この質問に対する様々な回答があります。 いずれの場合も、AAMでは、これらの条件を満たした訪問者の[!UICONTROL trait]を作成するのが一般的です。

この使用例では、コンバーターである人々の在庫を売り切れたので、既にこれが想定されています。 ただし、このチュートリアルの目的上、残りの使用例については参考として説明することをお勧めします。

また、[!UICONTROL traits]を作成するためにイベントを使用する場合は、[!UICONTROL trait]に収集するよりも多くのユーザーを収集しないよう、注意が必要な主な了解事項があります。 次のビデオで大きなリビールをご覧ください。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、この例は、Adobe Analyticsがあると仮定しています。明らかに、これはそうでないかもしれない。 Google Analytics(GA)をお持ちの場合は、AAMにデータを送信する際に使用できるモジュールが用意されています（[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)を参照）。サイトのコンバージョンアクティビティがGAによってAAMに送信された場合は、そこからコンバージョン特性を作成できます。 別のDILソリューションがある（または分析ソリューションがない）場合でも、分析コードや`submit`関数などを使用してAAMにデータを送信できます。 （[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)を参照）。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいて、コンバージョン特性を作成します。

## [!UICONTROL First Party]データから類似(look-alike) [!UICONTROL Model]を作成する {#creating-a-look-alike-model-from-first-party-data}

この手順では、[!UICONTROL First Party]類似(look-alike) [!UICONTROL Model]を作成します。 つまり、基本的な[!UICONTROL trait]/[!UICONTROL segment]に[!UICONTROL first party]変換[!UICONTROL trait]/[!UICONTROL segment]を使用するだけでなく（ほとんどの場合は[!UICONTROL models]では通常の方法です）、コンバーターに似た人のために[!UICONTROL first party]データのプールのみを調べます。 [!UICONTROL second party]や[!UICONTROL third party]のデータは表示されません。

この使用例では、コンバーターに見えるがまだコンバージョンに至っていないユーザーの[!UICONTROL segment]を作成しようとしているので、この類似した[!UICONTROL segment]を関心のある広告主に販売できます。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## アルゴリズム[!UICONTROL Trait]の作成 {#creating-an-algorithmic-trait}

次に、[!UICONTROL model]の結果を使用できるように、アルゴリズム[!UICONTROL Trait]を作成する必要があります。 [!UICONTROL trait]を作成しないと、[!UICONTROL model]は役に立ちません。 そのため、[!UICONTROL model]の実行後は、必ず[!UICONTROL trait]ダイアログを開き、アルゴリズム[!UICONTROL Trait]を作成してください。 次のビデオでは、この機能に関する説明をいくつか示します。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## アルゴリズム[!UICONTROL Segment]の広告主への提供 {#offering-the-algorithmic-segment-to-advertisers}

アルゴリズム[!UICONTROL Trait]を作成したら、新しい[!UICONTROL segment]を作成してデータをアクティブ化できます([!UICONTROL trait]はアクティブ化できませんが、[!UICONTROL Trait]を含む1 ～ [!UICONTROL trait] [!UICONTROL segment]を新しく作成して、[!UICONTROL segment]をアクティブ化（使用）できます)。

類似[!UICONTROL model]で高いスコアを獲得した[!UICONTROL first party]訪問者の[!UICONTROL segment]を作成したら（例えば、コンバーターに似ているがコンバーターがまだコンバージョンに至っていない）、サイト上の広告主にこの[!UICONTROL segment]を提供できます。 これは、Audience Managerで類似(look-alike) [!UICONTROL Models]を使用して、このオーディエンスを拡張し、引き続き追加の売上高を確認する優れた方法です。
