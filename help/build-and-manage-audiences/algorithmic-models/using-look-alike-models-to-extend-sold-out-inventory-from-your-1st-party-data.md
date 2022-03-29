---
title: 類似モデルを使用して、ファーストパーティデータから販売済みの在庫を拡張する
description: このチュートリアルでは、類似 (look-alike) モデルを設定して使用するための手順を説明します。これにより、類似 (look-alike) オーディエンスを新たに作成し、コンバージョンセグメントの拡張機能として販売できます。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 0%

---

# 類似モデルを使用して、ファーストパーティデータから販売済みの在庫を拡張する {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

このチュートリアルでは、類似 (look-alike) を設定して使用するために必要な手順について説明します [!UICONTROL Models]を使用して、新しい類似したオーディエンスを作成し、コンバージョンセグメントの拡張機能として販売できます。

## 使用例の詳細 {#use-case-details}

コンテンツの投稿者である。 サイト上のコンバータの在庫を既に売り切れている場合は、商談がそこに終わると考えるかもしれません。 AAM Look-Alike を入力 [!UICONTROL Models]. この機能を使用すると、売り切れの在庫をさらに拡張し、まだコンバージョンに至っていないが、コンバージョンに至った人のように見えたり行動したりする人のオーディエンスを販売することができます。 このオーディエンスセグメントは、通常、実際のコンバーターよりも少ない価格で販売されますが、サイトに広告を配置する広告主に対して追加のオーディエンスオプションを提供することで、を最終的に活用できます。 この使用例のその他の利点は、ファーストパーティデータでこのモデルを実行するのに何も費用がかからないことです。

このチュートリアルの手順は次のとおりです。

1. 理想的なユーザー（コンバージョン）の特性またはセグメントを特定/作成する
1. このコンバージョン特性/セグメントをベース項目として使用してモデルを作成する
1. 選択 [!UICONTROL First party] モデル内のデータソースとモデルの実行
1. の作成 [!UICONTROL Algorithmic Trait] モデルの結果から特性を削除し、セグメントに追加します。
1. 関心のある広告主にセグメントを提供してコンバージョンセグメントの売上を拡張する

## 理想的なユーザー（コンバージョン）の特性またはセグメントを特定または作成する {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上で担当者に何をしてもらおうとしていますか？ コンバージョンイベントは何ですか？ もちろん、サイトのタイプ/バーティカル、組織の目標に応じて、この質問に対する様々な回答があります。 いずれの場合も、AAMでは、これらの条件を満たした訪問者の特性を作成するのが一般的です。

この使用例では、コンバーターの担当者の在庫を売り切れたので、既にこれが想定されています。 ただし、このチュートリアルの目的上、残りの使用例に対する参考として説明することをお勧めします。

また、イベントを使用して特性を作成する場合、特性に必要な数より多くのユーザーを収集しないように、主な注意事項があることに注意する必要があります。 次のビデオで大きなリビールをご覧ください。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、この例は、Adobe Analyticsを使用していることを前提としています。 明らかに、そうでないかもしれない。 Google Analytics(GA) をお持ちの場合は、AAMにデータを送信する際に使用できるモジュールが用意されています ( [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)) を含め、サイト上のコンバージョンアクティビティが GA によってAAMに送信されている場合は、そこからコンバージョン特性を作成できます。 別の分析ソリューションがある場合（または分析ソリューションがない場合）も、DILコードと `submit` 機能等 ( [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)) をクリックします。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいて、コンバージョン特性を再度作成します。

## ファーストパーティデータから類似 (look-alike) モデルを作成する {#creating-a-look-alike-model-from-first-party-data}

この手順では、 [!UICONTROL First Party] 類似 (look-alike) モデル。 つまり、基本の特性/セグメントにファーストパーティのコンバージョン特性/セグメントを使用するだけでなく（ほとんどのモデルでは通常どおりです）、コンバーターに似た他のユーザー向けに、ファーストパーティデータのプールのみを調べます。 セカンドパーティやサードパーティのデータは対象となりません。

この使用例では、コンバーターに見えるがまだコンバージョンに至っていないユーザーのセグメントをサイト上に作成し、類似したセグメントを関心のある広告主に販売できるようにするので、これは重要です。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## アルゴリズム特性の作成 {#creating-an-algorithmic-trait}

次に、 [!UICONTROL Algorithmic Trait]を使用して、モデルの結果を使用できるようにします。 特性を作成しないと、モデルは役に立ちません。 モデルの実行後は、必ず特性ダイアログで [!UICONTROL Algorithmic Trait]. 次のビデオでは、このビデオに関する手順を説明し、いくつかのヒントを示します。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 次をオファー： [!UICONTROL Algorithmic Segment] 広告主 {#offering-the-algorithmic-segment-to-advertisers}

以下を作成したら、 [!UICONTROL Algorithmic Trait]を使用すると、新しいセグメントを作成してデータをアクティブ化できます ( 特性をアクティブ化することはできませんが、 [!UICONTROL Algorithmic Trait] セグメントをアクティブ化（使用）できるようにします。

類似モデルで高いスコアを獲得したファーストパーティ訪問者のセグメント（コンバーターに似ているが、まだコンバージョンに至っていない訪問者）を作成したら、サイトの実際のコンバーターの在庫をすべて売り切った後でも、このセグメントをサイトの広告主に提供できます。 これは、このオーディエンスを拡張し、類似 (look-alike) を使用して追加の売上高を見続けるのに最適な方法です [!UICONTROL Models] Audience Manager
