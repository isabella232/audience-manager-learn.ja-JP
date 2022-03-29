---
title: アルゴリズム（類似）モデルを使用して ROAS を増やす
description: Audience Managerの類似モデリングの真の力は、セカンドパーティおよびサードパーティのデータソースから得られる、高品質で新しいユーザーセットに対してベースラインオーディエンスを拡大しようとするときです。 このチュートリアルでは、このデータからモデルを作成する手順を説明します。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# Audience Managerでアルゴリズム（類似）モデルを使用して ROAS を増やす {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Managerの類似 (look-alike) の真の力 [!UICONTROL Modeling] は、セカンドパーティおよびサードパーティのデータソースから提供される、品質の高い新しいユーザーセットに対してベースラインオーディエンスを拡大しようとするときに発生します。 このチュートリアルでは、このデータからモデルを作成するために必要な手順を説明します。

## Audience Marketplaceからのセカンドパーティまたはサードパーティのデータストリームの有効化 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

類似モデルでセカンドパーティとサードパーティのデータを使用するには、まず、Audience Managerインターフェイスでこのデータを有効にする必要があります。 Adobeには、多数のセカンドパーティおよびサードパーティのデータプロバイダーがあり、これらから選択できます。 これらは、AAMのセルフサービスインターフェイスで、Audience Marketplaceを介して使用できます。 Audience Marketplaceに移動し、可能性を参照します。 次のビデオでは、無料の「購入前に試す」ストリームを有効にする方法を含め、データプロバイダーの価格設定にコミットする前に組織で最も役立つデータをロックインする方法を示します。

また、使用するデータプロバイダーの調査と決定に役立つ重要なリソースは、です [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 理想的なユーザー（コンバージョン）の特性またはセグメントを特定または作成する {#identify-create-an-ideal-user-conversion-trait-or-segment}

サイト上で担当者に何をしてもらおうとしていますか？ コンバージョンイベントは何ですか？ もちろん、サイトのタイプ/バーティカル、組織の目標に応じて、この質問に対する様々な回答があります。 いずれの場合も、AAMでは、これらの条件を満たした訪問者の特性を作成するのが一般的です。

以下のビデオでは、コンバージョン特性の作成方法を示します。このチュートリアルを続けて類似モデルを作成する際に、この特性を適切に使用する方法を示します。

また、Adobe Analyticsイベントを使用して特性を作成する場合、特性に必要な数より多くのユーザーを収集しないように、主な注意事項があることに注意する必要があります。 次のビデオで大きなリビールをご覧ください。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 上のビデオでは、この例は、Adobe Analyticsを使用していることを前提としています。 明らかに、そうでないかもしれない。 Google Analytics(GA) をお持ちの場合は、AAMにデータを送信する際に使用できるモジュールが用意されています ( [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html)) を含め、サイト上のコンバージョンアクティビティが GA によってAAMに送信されている場合は、そこからコンバージョン特性を作成できます。 別の分析ソリューションがある場合（または分析ソリューションがない場合）も、DILコードと `submit` 機能等 ( [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html)) をクリックします。 次に、サイトでコンバージョンアクティビティが実行されたときに送信されたデータに基づいてコンバージョン特性を作成します。

## セカンドパーティまたはサードパーティのデータから類似 (look-alike) モデルを作成する {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

上記の手順を完了したら、アルゴリズム（類似）モデルを作成する準備が整いました。 モデルを設定する際に、コンバージョン特性をベース特性（複製したい主要訪問者）として使用し、有効なサードパーティデータストリームをプールとして使用します。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要なベストプラクティス {#an-important-best-practice}

Audience Managerでアルゴリズムモデルを作成する場合、明らかにモデルをできるだけ有効にしたいと考えます。 モデルでは基本特性/セグメントのメンバーが属するすべての特性が考慮されるので、すべての人が特性/セグメントに属している場合、モデルは役に立ちません。 したがって、超一般的な特性（サイトにアクセスしたすべてのユーザーや、自分から広告を受け取ったすべてのユーザーなど）がある場合は、その属するデータソースがモデルのデータソースに含まれていないことを確認します。 この記事の使用例では、新しいルックエイクのサードパーティデータを見ることに重点を置いていますが、とにかく言及する価値があり、すべてのアルゴリズムモデルに適用されるので、そうは思われません。

## [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

次に、  [!UICONTROL Algorithmic Trait]を使用して、モデルの結果を使用できるようにします。 特性を作成しないと、モデルは役に立ちません。 したがって、モデルの実行後は、必ず特性ダイアログで [!UICONTROL Algorithmic Trait]. 次のビデオでは、このビデオに関する手順を説明し、いくつかのヒントを示します。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## モデルデータからセグメントを作成し、DSPに送信する {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

以下を作成したら、 [!UICONTROL Algorithmic Trait]を使用すると、新しいセグメントを作成してデータをアクティブ化できます ( 特性をアクティブ化することはできませんが、 [!UICONTROL Algorithmic Trait] セグメントをアクティブ化（使用）できるようにする。

このアルゴリズム特性からセグメントを作成すると、サイト上で既にコンバージョン済みの人物のように見える、潜在的なクライアントのオーディエンスが得られます。 これで、このセグメントを、Audience Manager内の任意のDSP宛先にマッピングできます。 通常の一般公開よりもサイト上でコンバージョンに至る可能性の高いルックエイクにマーケティングのターゲットを絞り、広告費用対効果を高めることができます。
