---
title: サイトのAudience Manager実装をクライアント側DILからサーバー側転送に移行する
description: クライアント側のAudience Managerからサーバー側の転送に、サイトのDIL(AAM) 実装を移行する方法について説明します。 このチュートリアルは、AAMとAdobe Analyticsの両方を使用し、DIL(Data Integration Library) コードを使用してページからAAMにヒットを送信し、ページからAdobe Analyticsにヒットを送信する場合に適用されます。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# サイトのAudience Manager実装をクライアント側DILからサーバー側転送に移行する {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

このチュートリアルは、Adobe Audience Manager(AAM) とAdobe Analyticsの両方を使用し、現在、DIL([!DNL Data Integration Library]) コードで始まり、また、ページからAdobe Analyticsにヒットを送信することもできます。 これらの両方のソリューションを使用しており、両方ともAdobe Experience Cloudに含まれているので、サーバー側転送を有効にするベストプラクティスに従うことができます。これにより、 [!DNL Analytics] データ収集サーバーを使用して、クライアント側のコードでページからAAMに追加のヒットを送信する代わりに、リアルタイムでサイト分析データをAudience Managerに転送することができます。 このチュートリアルでは、古いクライアント側のDIL実装から新しいサーバー側転送方法に切り替える手順について説明します。

## クライアント側 (DIL) とサーバー側 {#client-side-dil-vs-server-side}

Adobe AnalyticsのデータをAAMに取り込むこれら 2 つの方法を比較および対照する場合、まず次の画像の違いを視覚化すると役に立ちます。

![クライアント側からサーバー側へ](assets/client-side_vs_server-side_aam_implementation.png)

### クライアント側DILの実装 {#client-side-dil-implementation}

この方法を使用してAdobe AnalyticsデータをAAMに送信する場合、Web ページから 2 つのヒットが取得されます。1 つは [!DNL Analytics]（コピー後に）AAMに移動 [!DNL Analytics] データを Web ページに貼り付けます。 [!UICONTROL Segments] はAAMからページに返され、そこでパーソナライゼーションなどに使用できます。 これはレガシー実装と見なされ、お勧めしません。

これはベストプラクティスに従っていないという点以外に、この方法を使用するデメリットには次のものがあります。

* 1 つではなく、ページからの 2 つのヒット
* AAMオーディエンスをにリアルタイムで共有するには、サーバー側転送が必要です [!DNL Analytics]そのため、クライアント側の実装では、この機能（および将来他の機能になる可能性がある）は許可されません

AAM実装のサーバー側転送方法に移行することをお勧めします。

### サーバー側転送の実装 {#server-side-forwarding-implementation}

上の画像に示すように、Web ページからAdobe Analyticsにヒットが届きます。 [!DNL Analytics] 次に、そのデータをリアルタイムでAAMに転送し、訪問者はAAMの特性と [!UICONTROL segments]：ヒットがページから直接来た場合と同様です。

[!UICONTROL Segments] が [!DNL Analytics]：パーソナライゼーション用に Web ページに応答を転送します。

サーバー側転送に移行するタイミングが下がりません。 Adobeは、Audience Managerと [!DNL Analytics] はこの実装方法を使用します。

## 主なタスクは 2 つあります {#you-have-two-main-tasks}

このページにはかなり多くの情報があり、もちろんすべてが重要です。 しかし、 **すべては、あなたがする必要がある 2 つの主な事につながる**:

1. コードをクライアント側のDILコードからサーバー側転送コードに変更する
1. スイッチを [!DNL Analytics] [!DNL Admin Console] ( [!UICONTROL report suite])

これらのタスクのどちらかをスキップすると、サーバー側転送は正しく機能しません。 このドキュメントに、設定に合わせてこれら 2 つの手順を正しく実行するための手順と追加データが追加されました。

## 実装オプション {#implementation-options}

クライアント側からサーバー側転送に移行する際のタスクの 1 つとして、コードを新しいサーバー側転送コードに変更します。 これは、次のいずれかのオプションを使用しておこないます。

* Adobe Experience Platformタグ — Web プロパティに対するAdobeが推奨する実装オプションです。 Platform タグがすべての困難な作業をおこなったので、これは簡単な作業です。
* ページ上 — 新しい SSF コードを、 `doPlugins` 関数を `appMeasurement.js` ファイル ( まだAdobeLaunch を使用していない場合 )
* その他のタグマネージャー — SSF コードは、引き続き `doPlugins`( 他のタグマネージャーが [!DNL AppMeasurement] コード

以下の各項目について、 _コードの更新_ 」セクションに入力します。

## 実装の手順 {#implementation-steps}

次の手順は、の実装を説明します。

### 手順 0:前提条件：Experience CloudID サービス (ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

サーバー側転送に移行するための主な前提条件は、Experience CloudID サービスを実装することです。 これは、Experience Platform Launchを使用している場合に最も簡単におこなえます。その場合は、ECID 拡張機能をインストールするだけで、残りの操作を実行できます。

Adobe以外の TMS を使用している場合、または TMS をまったく使用していない場合は、実行する ECID を実装してください **前** その他のAdobeソリューション 詳しくは、 [ECID ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) を参照してください。 その他の前提条件は、コードバージョンに関するものです。そのため、次の手順で最新バージョンのコードを適用するだけで問題ありません。

>[!NOTE]
>
>実装する前に、このドキュメント全体をお読みください。 以下の「タイミング」の項では、 *when* ECID を含む各要素を実装する必要があります（まだ実装されていない場合）。

### 手順 1:現在使用されているレコードのDILコード {#step-record-currently-used-options-from-dil-code}

クライアント側のDILコードからサーバー側の転送に移行する準備が整ったら、最初の手順は、AAMに送信されるカスタム設定やデータなど、DILコードでおこなっているすべての操作を識別することです。 注意事項と考慮事項は次のとおりです。

* 標準 [!DNL Analytics] 変数、使用 `siteCatalyst.init` DILモジュール — このモジュールは、通常の [!DNL Analytics] 変数を渡す場合と、単にサーバー側転送を有効にしている場合に発生します。
* パートナーサブドメイン — 内 `DIL.create` 関数、 `partner` パラメーター。 これは「パートナーサブドメイン」または「パートナー ID」と呼ばれ、新しいサーバー側転送コードを配置する際に必要になります。
* [!DNL Visitor Service Namespace] - 「[!DNL Org ID]&quot;または&quot;[!DNL IMS Org ID]」と表示された場合は、新しいサーバー側転送コードを設定する際にも必要になります。 メモを取ります。
* containerNSID、uuidCookie およびその他の詳細設定オプション — サーバー側転送コードでも設定できるよう、使用しているその他の詳細設定オプションをメモしておきます。
* 追加のページ変数 — 通常の変数に加えて、他の変数がページからAAMに送信される場合。 [!DNL Analytics] 変数を siteCatalyst.init で処理する場合 )、サーバー側転送 ( スポイラーアラート：経由 [!DNL contextData] 変数 ) を参照してください。

### 手順 2:コードの更新 {#step-updating-the-code}

In [実装オプション](#implementation-options) （上記）サーバー側転送を実装する方法と場所に関して、複数のオプションが表示されます。 このセクションを有効にするには、このセクションに分割する必要があります（そのうち 2 つを組み合わせたセクション）。 必要に応じて、この節の方法を参照してください。

#### Adobe Experience Platformタグ {#launch-by-adobe}

Experience Platform Launchでのクライアント側のDILコードからサーバー側転送に実装オプションを移行する方法については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「ページ上」またはAdobe以外のタグマネージャー {#on-the-page-or-non-adobe-tag-manager}

でのクライアント側のDILコードからサーバー側転送への実装オプションの移行については、以下のビデオをご覧ください。 [!DNL AppMeasurement] ファイル内または非Adobeタグ管理システム内に存在するコード。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 手順 3:転送の有効化 ( [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

このチュートリアルでは、常に、コードをクライアント側のDILコードからサーバー側の転送に切り替えるのに時間を費やしました。 それはより難しい部分なので結構です。 この節は非常に簡単ですが、コードを更新するのと同じくらい重要です。 このビデオでは、Analytics からAudience Managerへのデータの実際の転送を可能にするスイッチの反転方法を確認します。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** このビデオで述べたように、転送の有効化がExperience Cloudのバックエンドで完全に実装されるまでに最大 4 時間かかることに注意してください。

## タイミング {#timing}

クライアント側の転送からサーバー側の転送に移行する際の主なタスクは 2 つあります。

1. コードの更新
1. スイッチを [!DNL Analytics] [!DNL Admin Console]

でも問題はどちらが先か？ 問題なの？ 申し訳ありませんが 2 つの質問でした しかし答えは…それは依存しています *can* 問題。 どうやって曖昧なの？ 分けてみましょう。 しかし、多数のサイトを持つ大規模な組織の場合に浮上する可能性のある追加の質問が 1 つ目です。私は一度に全てを行わなければなりませんか？ それは少し簡単です。 いいえ。 これは一つ一つできます

### もう少し深く潜る {#a-little-deeper-dive}

タイミングと注文が重要なのは、転送の仕組みが原因です _本当に_ 以下の技術的事実に要約できる。

* Experience CloudID サービス (ECID) が実装され、 [!DNL Analytics] [!DNL Admin Console] （「スイッチ」）がオンの場合、データは次の場所から転送されます： [!DNL Analytics] をAAMに送信します（まだコードを更新していない場合）。
* ECID を実装していない場合、スイッチをオンにしていて、サーバー側転送コードを使用していても、データは転送されません。
* サーバー側転送コード（Platform タグ内でもページ上でも）は、応答を実際に処理するので、移行の完了に必要です。
* サーバー側転送スイッチは、 [!UICONTROL report suite]に設定されていますが、このコードは Platform タグのプロパティ、または [!DNL AppMeasurement] ファイルを作成します。

### ベストプラクティス {#best-practices}

技術的な詳細に基づき、実行するタイミングと実行するタイミングに関する推奨事項を次に示します。

#### ECID がまだ実装されていない場合は、 {#if-you-do-not-have-ecid-yet-implemented}

1. スイッチをに入れます。 [!DNL Analytics] 各 [!UICONTROL report suite] を有効にして、サーバー側転送を有効にします。

   1. ECID がないので、転送はまだ開始されていません。

1. サイトごとに、コードをクライアント側DILからサーバー側転送（Platform タグに含まれる場合もあります）またはページ上で、上記の別の節で説明したように更新します。

   1. 今すぐ転送はフロー（ECID を追加したとおり）になり、 [!DNL Analytics] ビーコン（詳しくは、以下の「検証とトラブルシューティング」の節を参照してください）。

#### ECID が実装されている場合 {#if-you-do-have-ecid-implemented}

1. DILからサーバー側転送へのコードの更新準備が整うよう、準備と計画をおこないます。 [!UICONTROL report suite] サーバー側転送を有効にします。

   1. スイッチをに入れます。 [!DNL Analytics] サーバー側転送を有効にする。

      1. ECID が有効になっているので、転送が開始されます。
   1. 可能な限り早く、クライアント側DILからシングル側転送にコードを更新します（これは、前述の別の節で説明したように、Platform タグまたはページ上にある可能性があります）。

      1. 以下に示すように、 [!DNL Analytics] ビーコン ( [検証とトラブルシューティング](#validation-and-troubleshooting) 詳しくは、以下の節を参照してください )。


>[!NOTE]
>
>この 2 つの手順は、できるだけ近くにおこなうことが重要です。上記の手順 1 と 2 の間では、AAMへのデータの重複が発生するからです。 つまり、シングル側転送では、 [!DNL Analytics] また、AAMに移行する際に、DILコードがまだページ上にあるので、ページからAAMに直接ヒットが発生し、データが 2 倍になります。 DILからサーバー側転送にコードを更新すると、この問題は軽減されます。

>[!NOTE]
>
>データが少し重複するのではなく、データに小さな相違がある場合は、上記の手順 1 と 2 の順序を切り替えることができます。 DILからサーバー側転送にコードを移動すると、のサーバー側転送をオンにするスイッチを切り替えられるまで、AAMへのデータフローが停止します。 [!UICONTROL report suite]. 通常、お客様は、訪問者を特性や [!UICONTROL segments].

#### 多くのサイトが存在し、 [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

このトピックでは、前の節で簡単に取り上げ、主な戦略を次のように要約できます。

サイトを 1 つ移行/[!UICONTROL report suite] ( またはサイトのグループ/[!UICONTROL report suites]) を一度にクリックします。

ただし、考えられるシナリオに基づいては、これは少し難しくなる可能性があります。

* 複数のユニークなを含むサイトがある [!UICONTROL report suites]
* 次の条件を満たしている： [!UICONTROL report suite] 複数のサイト ( グローバルな [!UICONTROL report suite])
* 1 つの Platform タグプロパティを使用して、複数のサイトを対象とします。
* サイトごとに異なる開発チームがある

これらの項目のため、少し複雑になる可能性があります。 私が提案できる最善の方法は次のとおりです。

* 上記で説明した内容に基づいて、サーバー側転送への移行戦略を策定するには、しばらく時間をかけます
* Platform タグの 1 つのプロパティ ( または単一の [!DNL AppMeasurement] （ファイル）は通常、1 または 2 個の個別にマッピングされます [!UICONTROL report suites]では、企業をサーバー側転送に更新して、これらの個別のグループで 1 つずつ機能する計画を立てることができます
* Adobeコンサルティングと連携している場合は、移行計画について相談し、必要に応じてサポートを提供してください

## 検証とトラブルシューティング {#validation-and-troubleshooting}

サーバー側転送がインストールおよび導入されていることを検証する主な方法は、アプリからのAdobe Analyticsヒットへの応答を確認することです。

からのデータのサーバー側転送をおこなっていない場合 [!DNL Analytics] Audience Managerの場合、 [!DNL Analytics] ビーコン（2x2 ピクセル以外）に追加します。 ただし、サーバー側転送をおこなっている場合は、 [!DNL Analytics] を知らせるリクエストと応答 [!DNL Analytics] は、Audience Managerと正しく通信し、ヒットを転送して、応答を取得しています。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>誤った「Success」に注意してください。 応答があり、すべてが機能しているように見える場合は、 `stuff` オブジェクトを返します。 そうしないと、次のようなメッセージが表示される場合があります。 `"status":"SUCCESS"`. 変に思えますが、実際には、これは正しく機能していない証拠です。
>
>これが表示される場合は、Platform タグでコードの更新が完了しているか、 [!DNL AppMeasurement]しかし [!DNL Analytics] [!DNL Admin Console] はまだ完了していません。 この場合、 [!DNL Analytics] [!DNL Admin Console] の [!UICONTROL report suite]. まだ 4 時間が経過していない場合は、バックエンドで必要な変更をすべておこなうのにそれほど時間がかかる可能性があるので、しばらくお待ちください。


![偽成功](assets/falsesuccess.png)

サーバー側転送について詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=ja).
