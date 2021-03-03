---
title: Audience Managerでアルゴリズム（類似）モデルを使用してROASを増やす
description: Audience Managerの「そっくりなモデリング」の本領は、第2者および第3者のデータソースから新しい品質のユーザー群に対して、ベースラインオーディエンスを拡大しようとするときに生まれます。 このチュートリアルでは、このデータからモデルを作成する手順を学習します。
feature: アルゴリズムモデル
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: 「ビジネス実践者、開発者、データ・エンジニア、アーキテクト、データ・アーキテクト、管理者、リーダー」
level: 中間
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 0%

---


# Audience Manager{#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}のアルゴリズム（類似） [!UICONTROL Models]を使用してROASを増やす

Audience Managerの「みなさん[!UICONTROL Modeling]」の本当の力は、[!UICONTROL second party]と[!UICONTROL third party] [!UICONTROL data sources]の新しいユーザーセットに対してベースラインオーディエンスを拡張しようとするときに生まれます。 このチュートリアルでは、このデータから[!UICONTROL model]を作成するために必要な手順を学びます。

## Audience Marketplace{#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}から[!UICONTROL Second Party]または[!UICONTROL Third Party]データストリームを有効にする

[!UICONTROL second party]と[!UICONTROL third party]のデータを類似した[!UICONTROL model]で使用するには、まずこのデータをAudience Managerインターフェイスで有効にする必要があります。 Adobeには多数の[!UICONTROL second party]および[!UICONTROL third party]データプロバイダーがあり、これらから選択できます。 これらは、Audience Marketplaceを介して、AAMのセルフサービスインターフェイスで使用できます。 Audience Marketplaceに移動し、可能性を参照します。 次のビデオでは、無料の「購入する前に試す」ストリームを有効にし、データプロバイダーの価格に従う前に組織で最も役に立つデータをロックインできるようにする方法など、これを行う方法を示します。

また、使用するデータプロバイダーを調べて判断する際に役立つリソースは[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)です。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 理想的なユーザー（コンバージョン）を特定/作成[!UICONTROL trait]または[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上で人々に何をしてもらおうとしているか。 コンバージョンイベントは何ですか。 もちろん、サイトのタイプ/業種や組織の目標に応じて、この質問に対する答えは多く異なります。 いずれにせよ、AAMでは、これらの条件を満たした訪問者用に[!UICONTROL trait]を作成するのが一般的です。

次のビデオでは、コンバージョン[!UICONTROL trait]の作成方法を紹介します。このチュートリアルを進めながら、類似した[!UICONTROL model]を作成する際に使用したいコンバージョンを作成します。

また、Adobe Analyticsイベントを使って[!UICONTROL traits]を作成する際には、主な了解事項があるので、[!UICONTROL trait]より多くのユーザーを収集しないように注意する必要があります。 次のビデオで大きなリビールを確認してください。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、この例はAdobe Analyticsをお持ちの方を想定しています。明らかに、そうでないかもしれない。 Google Analytics(GA)をお持ちの場合は、データをAAMに送信する際に使用できるモジュール（[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)を参照）があり、サイトのコンバージョンアクティビティがGAによってAAMに送られる場合は、そこからコンバージョン[!UICONTROL trait]を作成できます。 別の解析ソリューションがある（または解析ソリューションがない）場合でも、DILコードや`submit`関数などを使用してAAMにデータを送信できます。 （[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)を参照）。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいて、コンバージョン[!UICONTROL trait]を作成します。

## [!UICONTROL Second Party]または[!UICONTROL Third Party]データ{#create-a-look-alike-model-from-2nd-or-3rd-party-data}から類似データ[!UICONTROL Model]を作成

上記の手順を完了したら、アルゴリズム（類似） [!UICONTROL Model]を作成する準備ができました。 [!UICONTROL model]を設定する際、コンバージョン[!UICONTROL trait]を基本[!UICONTROL trait](重複を希望する主要訪問者)として使用し、有効な[!UICONTROL third party]データストリームを引き出し元の人々のプールとして使用します。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要なベストプラクティス{#an-important-best-practice}

Audience Managerでアルゴリズム[!UICONTROL model]を作成する場合、[!UICONTROL model]をできるだけ有効にしたいと思っているのは明らかです。 [!UICONTROL model]は[!UICONTROL trait]/[!UICONTROL segment]のメンバーが含まれる[!UICONTROL traits]をすべて検討しているので、[!UICONTROL trait]/[!UICONTROL segment]に全ての人がいると[!UICONTROL model]を助けにはなりません。 したがって、（サイトを訪れた全員や、貴社から広告を受け取った全員などの）スーパージェネリック[!UICONTROL traits]を持っている場合は、その属する[!UICONTROL data source]が[!UICONTROL model]の[!UICONTROL data sources]に含まれていないことを確認します。 この記事の使用例では、新しいルックエリクスの[!UICONTROL third party]データを調べることに重点を置いているので、そうは行わないと思いますが、とにかく言及する価値があり、アルゴリズム[!UICONTROL models]のすべてに当てはまります。

## アルゴリズムの作成[!UICONTROL Trait] {#creating-an-algorithmic-trait}

次に、[!UICONTROL model]の結果を使用できるように、アルゴリズム[!UICONTROL Trait]を作成する必要があります。 [!UICONTROL trait]を作成しないと、モデルは役に立ちません。 したがって、[!UICONTROL model]の実行後は、必ず[!UICONTROL trait]ダイアログを開き、アルゴリズム[!UICONTROL Trait]を作成してください。 次のビデオでは、このビデオの概要とヒントをいくつか紹介します。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## [!UICONTROL Model]データから[!UICONTROL Segment]を作成し、DSPに送信する{#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

アルゴリズム[!UICONTROL Trait]を作成したら、新しい[!UICONTROL segment]を作成してデータをアクティブにできます([!UICONTROL trait]はアクティブにできませんが、新しい[!UICONTROL trait] [!UICONTROL segment]を作成してアルゴリズム[!UICONTROL Trait]をアクティブに（使用）できます)。[!UICONTROL segment]

このアルゴリズム[!UICONTROL trait]から[!UICONTROL segment]を作成すると、サイト上でコンバージョンが完了した人のように見える潜在的なクライアントのオーディエンスが得られます。 これで、この[!UICONTROL segment]をAudience Managerの任意のDSP [!UICONTROL destinations]にマップできます。 マーケティングのターゲットは、通常の一般ユーザーよりもサイト上でコンバージョンする可能性の高いルックエイリクに対して行え、広告費収益率(ROS)を増やすことができます。 頑張れ！
