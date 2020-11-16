---
title: Audience Managerでアルゴリズム（類似）モデルを使用してROASを増やす
description: Audience Managerの「そっくりなモデリング」の本領は、第2者および第3者のデータソースから新しい品質のユーザー群に対して、ベースラインオーディエンスを拡大しようとするときに生まれます。 このチュートリアルでは、このデータからモデルを作成する手順を学習します。
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1849
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---


# Audience Managerでアルゴリズム（「類似」）を使用してROAS [!UICONTROL Models] を増やす {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Managerの「そっくりさ」の真の力 [!UICONTROL Modeling] は、およびの新しいユーザーセットに対してベースラインオーディエンスを拡張しようとするときに生じ [!UICONTROL second party] ま [!UICONTROL third party][!UICONTROL data sources]す。 このチュートリアルでは、このデータ [!UICONTROL model] からを作成する手順を説明します。

## Audience Marketplace [!UICONTROL Second Party] からの [!UICONTROL Third Party] データストリームの有効化 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

およ [!UICONTROL second party] びデータを類似したものとして使用するに [!UICONTROL third party][!UICONTROL model]は、まずこのデータをAudience Managerインターフェイスで有効にする必要があります。 Adobeには多数の [!UICONTROL second party] および [!UICONTROL third party] データプロバイダーがあり、それらから選択できます。 これらは、Audience Marketplaceを介して、AAMのセルフサービスインターフェイスで使用できます。 Audience Marketplaceに移動し、可能性を参照します。 次のビデオでは、無料の「購入する前に試す」ストリームを有効にし、データプロバイダーの価格に従う前に組織で最も役に立つデータをロックインできるようにする方法など、これを行う方法を示します。

また、どのデータプロバイダーを使用するかの調査や決定に役立つ重要なリソースがあり [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)ます。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 理想的なユーザー（コンバージョン）を特定/作成 [!UICONTROL trait] する、または [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上で人々に何をしてもらおうとしているか。 コンバージョンイベントは何ですか。 もちろん、サイトのタイプ/業種や組織の目標に応じて、この質問に対する答えは多く異なります。 いずれにせよ、AAMでは、これらの条件を満たした訪問者 [!UICONTROL trait] 用のフォームを作成するのが一般的です。

次のビデオでは、コンバージョンの作成方法を紹介します。このチュートリアルを進め [!UICONTROL trait]ながら、コンバージョンを作成し、そっくりなものを作成し [!UICONTROL model]ます。

また、Adobe Analyticsイベントを使用して作成する際 [!UICONTROL traits]には、ユーザの収集数が多くならないように、重要な了解事項を念頭に置いておく必要があり [!UICONTROL trait]ます。 次のビデオで大きなリビールを確認してください。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、例を見てみると、Adobe Analyticsがいると想定しています。 明らかに、そうでないかもしれない。 Google Analytics(GA)をお持ちの場合は、AAMへのデータ送信に使用できるモジュール( [ドキュメントを参照](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html))が用意されています。サイト上のコンバージョンアクティビティがGAによってAAMに送られる場合は、そこからコンバージョンを作成でき [!UICONTROL trait] ます。 別の解析ソリューションがある場合（または解析ソリューションがない場合）も、DILコードや `submit` 関数などを使用して、AAMにデータを送信できます。 ( [ドキュメントを参照](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html))。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに [!UICONTROL trait] 基づいてコンバージョンを作成します。

## データ [!UICONTROL Model] またはデータから類似し [!UICONTROL Second Party][!UICONTROL Third Party] たものを作成 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

上記の手順を完了したら、アルゴリズム（「類似」）を作成する準備が整い [!UICONTROL Model]ました。 を設定する際 [!UICONTROL model]、コンバージョンを基盤 [!UICONTROL trait] (重複したい主要訪問者 [!UICONTROL trait] )として使用し、有効な [!UICONTROL third party] データストリームを引き出し元の人々のプールとして使用します。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要なベストプラクティス {#an-important-best-practice}

Audience Managerでアルゴリズム [!UICONTROL model] を作成する際には、できる限り効果的なアルゴリズム [!UICONTROL model] を使用したいと考えています。 は、基地のメンバー [!UICONTROL model] / [!UICONTROL traits] が含まれるすべて [!UICONTROL trait]のことを検討しているので、すべてのユーザーが/に含まれている[!UICONTROL segment] 場合は役に立ちません [!UICONTROL model][!UICONTROL trait][!UICONTROL segment]。 したがって、サイトを訪問したすべての訪問者 [!UICONTROL traits] や、貴社から広告を受け取ったすべての訪問者など、超汎用的なユーザーがいる場合は、そのユーザー [!UICONTROL data source] の属するユーザーが貴社ののに含まれていないことを確認し [!UICONTROL data sources] てください [!UICONTROL model]。 この記事の使用例では、新しいルックエイリクスの [!UICONTROL third party] データを調べることに重点を置いているので、その可能性は低いと考えられますが、とにかく言及する価値はあります。また、アルゴリズムのすべてに適用され [!UICONTROL models]ます。

## アルゴリズムの作成 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

次に、アルゴリズムを作成し、その結果 [!UICONTROL Trait]を使用できるようにする必要 [!UICONTROL model] があります。 を作成しないと [!UICONTROL trait]、モデルは無用です。 したがって、 [!UICONTROL model] 実行後は、必ず [!UICONTROL trait] ダイアログを開き、アルゴリズムを作成してください [!UICONTROL Trait]。 次のビデオでは、このビデオの概要とヒントをいくつか紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## データ [!UICONTROL Segment] からの作成とDSPへの [!UICONTROL Model] 送信 {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

アルゴリズムを作成したら、新しいアルゴリズムを作成し [!UICONTROL Trait]てデータをアクティブ化できます(アクティブ化はできませんが、アルゴリズムを使用して新しい1つを作成し [!UICONTROL segment] て、アクティブ化（使用）できるようにし [!UICONTROL trait][!UICONTROL trait][!UICONTROL segment][!UICONTROL Trait][!UICONTROL segment]ます)。

このアルゴリズム [!UICONTROL segment][!UICONTROL trait]からを作成すると、サイト上でコンバージョンが完了した人のように見える潜在的な顧客のオーディエンスが得られます。 これで、Audience Manager内の任意のDSP [!UICONTROL segment] にこれをマッピング [!UICONTROL destinations] できます。 マーケティングのターゲットは、通常の一般ユーザーよりもサイト上でコンバージョンする可能性の高いルックエイリクに対して行え、広告費収益率(ROS)を増やすことができます。 頑張れ！
