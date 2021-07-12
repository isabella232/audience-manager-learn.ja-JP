---
title: Audience Managerでのアルゴリズム（類似）モデルを使用したROASの向上
description: Audience Managerの類似(look-alike)モデリングの真の力は、セカンドパーティおよびサードパーティのデータソースから得られる一連の新しいユーザーに対して、ベースラインオーディエンスを拡大しようとするときに得られます。 このチュートリアルでは、このデータからモデルを作成する手順を説明します。
feature: アルゴリズムモデル
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---

# Audience Managerでアルゴリズム(類似(look-alike) [!UICONTROL Models]を使用してROASを増やす {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Managerの類似(look-alike) [!UICONTROL Modeling]の実力は、[!UICONTROL second party]と[!UICONTROL third party] [!UICONTROL data sources]の質の高い、新しいユーザーセットに対してベースラインオーディエンスを拡大しようとするときに生じます。 このチュートリアルでは、このデータから[!UICONTROL model]を作成するために必要な手順を説明します。

## Audience Marketplaceから[!UICONTROL Second Party]または[!UICONTROL Third Party]データストリームを有効にする {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

[!UICONTROL second party]と[!UICONTROL third party]のデータを類似(look-alike)[!UICONTROL model]で使用するには、まずこのデータをAudience Managerインターフェイスで有効にする必要があります。 Adobeには、多数の[!UICONTROL second party]および[!UICONTROL third party]データプロバイダーがあり、ここから選択できます。 これらは、AAMのセルフサービスインターフェイスで、Audience Marketplaceを介して使用できます。 Audience Marketplaceに移動し、可能性を参照します。 次のビデオでは、無料の「購入前に試す」ストリームを有効にする方法を含め、データプロバイダーの価格設定にコミットする前に組織で最も役立つデータをロックインする方法を示します。

また、使用するデータプロバイダーの調査と決定に役立つように、[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)が大きなリソースとなります。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 理想的なユーザー（コンバージョン）を特定/作成する[!UICONTROL trait]または[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上でユーザーに何をしてもらおうとしていますか。 コンバージョンイベントの種類 もちろん、サイトのタイプ/垂直方式や組織の目標に応じて、この質問に対する様々な回答があります。 いずれの場合も、AAMでは、これらの条件を満たした訪問者の[!UICONTROL trait]を作成するのが一般的です。

以下のビデオでは、コンバージョン[!UICONTROL trait]の作成方法を紹介します。このチュートリアルを続けて類似した[!UICONTROL model]を作成する際に、このコンバージョンを用意しておきます。

また、Adobe Analyticsイベントを使用して[!UICONTROL traits]を作成する場合、[!UICONTROL trait]に収集するよりも多くのユーザーを収集しないように、主な了解事項に留意する必要があります。 次のビデオで大きなリビールをご覧ください。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、この例は、Adobe Analyticsがあると仮定しています。明らかに、これはそうでないかもしれない。 Google Analytics(GA)をお持ちの場合は、データをAAMに送信するために使用できるモジュールが用意されています（[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)を参照）。サイトのコンバージョンアクティビティがGAによってAAMに送信された場合は、そこからコンバージョン[!UICONTROL trait]を作成できます。 別のDILソリューションがある（または分析ソリューションがない）場合でも、分析コードや`submit`関数などを使用してAAMにデータを送信できます。 （[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)を参照）。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいてコンバージョン[!UICONTROL trait]を作成します。

## [!UICONTROL Second Party]または[!UICONTROL Third Party]データから類似(look-alike) [!UICONTROL Model]を作成します {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

上記の手順を完了したら、アルゴリズム（類似） [!UICONTROL Model]を作成する準備が整いました。 [!UICONTROL model]を設定する際に、コンバージョン[!UICONTROL trait]をベース[!UICONTROL trait]（複製したい主要訪問者）として使用し、有効になっている[!UICONTROL third party]データストリームをプル元の人々のプールとして使用します。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要なベストプラクティス {#an-important-best-practice}

Audience Managerでアルゴリズムの[!UICONTROL model]を作成する場合は、必ず[!UICONTROL model]をできるだけ有効にしておきたいと思います。 [!UICONTROL model]は、[!UICONTROL trait]/[!UICONTROL segment]のメンバーが含まれる[!UICONTROL traits]をすべて検討しているので、すべての人が[!UICONTROL trait]/[!UICONTROL segment]にいる場合、[!UICONTROL model]は役に立ちません。 したがって、（サイトを訪問した全員や、自分から広告を受け取った全員など）超汎用の[!UICONTROL traits]がある場合は、その属する[!UICONTROL data source]が[!UICONTROL model]の[!UICONTROL data sources]に含まれていないことを確認します。 この記事の使用例では、新しいルックエイリクスの[!UICONTROL third party]データを見ることに集中しているので、そうは思わないでしょうが、とにかく言及する価値があり、すべてのアルゴリズム[!UICONTROL models]に適用されます。

## アルゴリズム[!UICONTROL Trait]の作成 {#creating-an-algorithmic-trait}

次に、[!UICONTROL model]の結果を使用できるように、アルゴリズム[!UICONTROL Trait]を作成する必要があります。 [!UICONTROL trait]を作成しないと、モデルは役に立ちません。 したがって、[!UICONTROL model]の実行後は、必ず[!UICONTROL trait]ダイアログを開き、アルゴリズム[!UICONTROL Trait]を作成してください。 次のビデオでは、この機能に関する手順を説明し、いくつかのヒントを示します。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## [!UICONTROL Model]データから[!UICONTROL Segment]を作成し、DSPに送信する {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

アルゴリズム[!UICONTROL Trait]を作成したら、新しい[!UICONTROL segment]を作成してデータをアクティブ化できます([!UICONTROL trait]はアクティブ化できませんが、[!UICONTROL Trait]を含む1 ～ [!UICONTROL trait] [!UICONTROL segment]を新しく作成して、[!UICONTROL segment]をアクティブ化（使用）できます)。

このアルゴリズム[!UICONTROL trait]から[!UICONTROL segment]を作成すると、サイト上でコンバージョン済みの人のように見える、潜在的なクライアントのオーディエンスが得られます。 これで、この[!UICONTROL segment]を、Audience Manager内の任意のDSP [!UICONTROL destinations]にマッピングできます。 通常の一般ユーザーよりもサイト上でコンバージョンする可能性の高いルックエイクスにマーケティングのターゲットを設定し、広告費用対効果を高めることができます。 頑張れ！
