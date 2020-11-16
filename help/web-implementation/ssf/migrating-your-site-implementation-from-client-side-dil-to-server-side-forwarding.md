---
title: クライアント側DILからサーバー側転送へのサイト実装の移行
description: このチュートリアルは、Adobe Audience Manager(AAM)とAdobe Analyticsの両方をお持ちで、現在、「DIL」(Data Integration Library)コードを使用してページからAAMにヒットを送信し、ページからのヒットをAdobe Analyticsに送信している場合に適用されます。 これらのソリューションは両方あり、両方ともAdobe Experience Cloudの一部なので、「サーバー側転送(SSF)」を有効にするベストプラクティスに従います。これにより、Analyticsデータ収集サーバーは、クライアント側コードでページからAAMにヒットを送信する代わりに、リアルタイムでサイト解析データをAudience Managerに転送できます。 このチュートリアルでは、古い「クライアント側DIL」から新しい「サーバ側転送」への切り替え手順を説明します。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# サイトのAAM導入を [!DNL Client-Side] DILから移行する [!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

このチュートリアルは、Adobe Audience Manager(AAM)とAdobe Analytics()の両方を持ち、現在、「DIL」([!DNL Data Integration Library])コードを使用してページのヒットをAAMに送信し、ページのヒットをAdobe Analyticsに送信している場合に適用されます。 これらのソリューションは両方あり、両方ともAdobe Experience Cloudの一部なので、「[!DNL Server-Side Forwarding] (SSF)」を有効にするベストプラクティスに従うことができます。これにより、 [!DNL Analytics][!DNL client-side] データ収集サーバーは、ページから別のヒットをAAMにコードで送信する代わりに、リアルタイムでサイト分析データをAudience Managerに転送できます。 このチュートリアルでは、古い「」実装から新しい「[!DNL Client-Side DIL][!DNL Server-Side forwarding]」メソッドに切り替える手順を説明します。

## [!DNL Client-Side] (DIL)と [!DNL Server-Side] {#client-side-dil-vs-server-side}

Adobe AnalyticsのデータをAAMに送り込む2つの方法を比較・対比する際には、まず次の図の違いを視覚化すると便利です。

![クライアント側からサーバー側へ](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DILの実装 {#client-side-dil-implementation}

この方法を使用してAdobe AnalyticsのデータをAAMに取り込む場合、ウェブページから2つのヒットがあることを意味します。1つは [!DNL Analytics]、もう1つはAAMに移動します(Webページに [!DNL Analytics] データをコピーした後)。 [!UICONTROL Segments] がAAMからページに返され、そこでパーソナライゼーションなどに使用できます。 これは「従来の」実装と見なされ、推奨されなくなりました。

これはベストプラクティスに従っていないことを除き、この方法を使用するデメリットは次のとおりです。

* 単一のヒットではなく、ページからの2つのヒット
* [!UICONTROL Server-Side Forwarding] はとのAAMオーディエンスのリアルタイム共有に必要なため [!DNL Analytics]、 [!DNL Client-side] 実装ではこの機能（および将来他の機能が使用される可能性がある）を許可しません。

AAM導入の [!UICONTROL Server-Side Forwarding] 方法に移行することをお勧めします。

### [!UICONTROL Server-Side Forwarding] 実装 {#server-side-forwarding-implementation}

上の図に示すように、WebページからAdobe Analyticsにヒットが届きます。 [!DNL Analytics] そのデータをリアルタイムでAAMに転送し、訪問者はAAM [!UICONTROL traits] に評価され、ページから直接ヒットが届いたかのように [!UICONTROL segments]なります。

[!UICONTROL Segments] は、同じリアルタイムヒットでに返され [!DNL Analytics]、訪問者の個人設定などを行うためにウェブページに応答を転送します。

サーバー側転送に移動するタイミングを下げることはできません。 Audience Managerを持ち、この実装方法を [!DNL Analytics] 利用する方は誰でもお勧めします。

## 2つの主なタスクがあります {#you-have-two-main-tasks}

このページにはかなり多くの情報が載っていて、もちろん重要なことです。 ただし、 **すべては、次の2つの主な必要事項に要約されます**。

1. コードを [!DNL Client-Side] DILコードからコードに変更し [!UICONTROL Server-Side Forwarding] ます
1. 開始でスイッチをフリップ [!DNL Analytics] し、実際のデータ転送 [!DNL Admin Console] を(1回 [!UICONTROL report suite]のみ)

この2つのうちどちらかをスキップすると、SSFは正しく動作しません。 このドキュメントには、これら2つの手順を正しく行うための手順と追加データが追加されています。

## 導入オプション {#implementation-options}

に移る際 [!DNL client-side] に、タスクの1つ [!DNL server-side]として、コードを新しい [!UICONTROL Server-Side Forwarding] コードに変更します。 これは、次のいずれかのオプションを使用して行います。

* Adobe Experience Platform Launch- Webプロパティの推奨される実装オプション。 これは非常に簡単なタスクで、すべての難しい作業 [!DNL Launch] をお前にやったことが分かるだろう
* ページ上 — 新しいSSFコードを `doPlugins`[!DNL appMeasurement.js] ファイル内の関数に直接配置することもできます(まだAdobe起動を使用していない場合)。
* その他のタグマネージャー — 他のタグマネージャーがコードを格納している場合は常に、SSFコードをに挿入するので、前の（ページ上の）オプション `doPlugins`と同じように扱うことができま [!DNL AppMeasurement] す。

「コードの更新」セクションで以下の各項目を確認します。

## 実装手順 {#implementation-steps}

### 手順0:前提条件：Experience CloudIDサービス(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

移行の主な前提条件は、Experience CloudIDサービス [!UICONTROL Server-Side Forwarding] を導入することです。 これは、Experience Platform Launchを使用している場合に最も簡単に行えます。この場合は、ECID拡張をインストールするだけで、その他の作業も行えます。

Adobe以外のTMSを使用している場合、またはTMSをまったく使用していない場合は、他のAdobeソリューションより **前に** ECIDを実装して実行してください。 詳しくは、 [ECIDのドキュメント](https://marketing.adobe.com/resources/help/ja_JP/mcvid/) を参照してください。 その他の前提条件はコードバージョンに関するものです。以下の手順で最新のバージョンのコードを適用するだけなので、問題ありません。

>[!NOTE]
>
>実装する前に、このドキュメント全体をお読みください。 以下の「タイミング」の節では、ECIDを含む各部品 *をいつ実装すべきかに関する重要な情報を紹介します* （まだ実装されていない場合）。

### 手順1:DILコードから現在使用されているオプションを記録 {#step-record-currently-used-options-from-dil-code}

DILコードから [!DNL Client-Side][!UICONTROL Server-Side Forwarding]DILコードに移行する準備が整ったら、まず、カスタム設定やAAMに送信されるデータなど、コードで行っているすべての操作を特定します。 次の点に注意し、考慮する必要があります。

* 通常 [!DNL Analytics] 変数、 [!DNL siteCatalyst.init] DILモジュールを使用 — この変数は、通常の [!DNL Analytics] 変数を送り返すだけで、SSFを有効にするだけで発生するので、心配する必要はありません。
* パートナーサブドメイン —DIL.create関数で、 `partner` パラメーターをメモしておきます。 これは、「パートナーサブドメイン」または「パートナーID」と呼ばれ、新しいSSFコードを配置する際に必要になります。
* [!DNL Visitor Service Namespace] - 「[!DNL Org ID]」または「[!DNL IMS Org ID]」とも呼ばれ、新しいSSFコードを設定する場合も同様に必要です。 書き留めておけ。
* containerNSID、uuidCookie、およびその他の高度なオプション — 使用しているその他の高度なオプションをメモしておくと、SSFコードでも設定できるようになります。
* 追加のページ変数 — AAMに他の変数を(siteCatalyst.initで処理される通常の [!DNL Analytics] 変数に加えて)ページから送信する場合、それらの変数をSSF経由で送信できるようにメモする必要があります(スポイラーの警告：変数を使用 [!DNL contextData] )。

### 手順2:コードの更新 {#step-updating-the-code}

上記の「導入オプション」というタイトルのセクションには、導入する方法/場所に関する複数のオプションが表示され [!UICONTROL Server-Side Forwarding]ます。 この節を有効にするためには、この節に分ける必要があります（2つの節を組み合わせて）。 ニーズに最も適した説明をこのセクションの手法に進みます。

#### Adobe Experience Platform Launch {#launch-by-adobe}

以下のビデオを見て、 [!DNL Client-Side] DILコードからExperience Platform Launchへの導入オプションの移行につ [!UICONTROL Server-Side Forwarding] いて学習してください。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「ページ上」または「Adobe Tag Manager以外」 {#on-the-page-or-non-adobe-tag-manager}

ファイルまたはAdobe以外のタグ管理システムに存在する [!DNL Client-Side] DILコードからコード [!UICONTROL Server-Side Forwarding][!DNL AppMeasurement] 内に実装オプションを移動する方法については、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 手順3:転送を有効にする(単位 [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

これまで、コードをコードからコードに切り替える作業に時間を費やしてき [!DNL Client-Side DIL] ました [!UICONTROL Server-Side Forwarding]。 それはより難しい部分だから良い。 この節は、非常に簡単ですが、コードを更新するのと同じくらい重要です。 このビデオでは、AnalyticsからAudience Managerへのデータの実際の転送を可能にするスイッチの切り替え方法を見ていきます。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** このビデオで説明したように、転送の有効化がExperience Cloudバックエンドで完全に実装されるまでに最大4時間かかることに注意してください。

## タイミング {#timing}

念のため、次の2つの主なタスクに従って、次の間を移動 [!DNL Client-Side DIL] し [!UICONTROL Server-Side Forwarding]ます。

1. コードの更新
1. スイッチを [!DNL Analytics] [!DNL Admin Console]

でも問題はどちらが先か？ 重要か？ 2つの質問です しかし答えは…それは違うし *、そうだ* 。 どうやって曖昧なの？ 分けてみよう しかし、まず、多数のサイトを持つ大きな組織では、次のような疑問が浮かび上がる可能性があります。一度に何もかもやらなきゃいけないの？ あれは少し楽です。 いいえ。 一つ一つで…できます:)

### もう少し深く潜る {#a-little-deeper-dive}

タイミングと順序が重要なのは、forwarding *really *worksの仕組みが原因で、以下の技術的事実に要約できる。

* Experience CloudIDサービス(ECID)を実装していて、 [!DNL Analytics] （「スイッチ」）のスイッチがオンの場合、コードをまだ更新していない場合でも、データはAAM [!DNL Admin Console][!DNL Analytics] に転送されます。
* ECIDを実装していない場合、スイッチがオンになっていて、SSFコードを持っていても、データは転送されません。
* SSFコード(ページ内 [!DNL Launch] かページ上かにかかわらず)は、応答を実際に処理し、もちろん移行を完了するために必要です。
* SSFスイッチはによって有効にされ [!UICONTROL Report Suite]ますが、コードはのプロパティで処理され [!DNL Launch][!DNL AppMeasurement] ます。または、 [!DNL Launch]

### ベストプラクティス {#best-practices}

これらの技術的な詳細に基づき、「何をすべきか」のタイミングに関する推奨事項を以下に示します。

#### ECIDをまだ実装していない場合 {#if-you-do-not-have-ecid-yet-implemented}

1. SSFを有効にするたび [!DNL Analytics] にスイッチ [!UICONTROL report suite] を入れます。

   1. ECIDがないので、転送はまだ開始されません

1. サイトごとに、コードをSSF [!DNL Client-Side DIL] に更新します(上の別のセクションで説明したように、コードが [!DNL Launch] ページ内またはページ上にある可能性があります)。

   1. 転送は、（ECIDを追加したので）フローするようになり、また、ビーコンに対する適切なJSON応答も受け取るはずです（詳しくは、以下の「検証とトラブルシューティング」の節を参照） [!DNL Analytics]

#### ECIDが実装されている場合 {#if-you-do-have-ecid-implemented}

1. SSFを有効にするために、コードをDILからSSF PERに更新する準備が整う [!UICONTROL report suite] ように準備および計画を立てます。

   1. SSFを有効にするには、スイッチ [!DNL Analytics] を入れます。

      1. ECIDが有効になっているため、転送WILL開始が発生する
   1. 可能な限り早く、コードをからSSF [!DNL Client-Side DIL] に更新します(上記の他のセクションで説明したように、ページ内 [!DNL Launch] またはページ上にあります)。

      1. ビーコンに対する適切なJSON応答を受け取る [!DNL Analytics] はずです（詳しくは、以下の「検証とトラブルシューティング」の節を参照してください）


**注1:** この2つの手順はできるだけ近い距離に置くことが重要です。上記の手順1と2の間には、データの重複がAAMに入るからです。 つまり、SSFはAAM [!DNL Analytics] へのデータ送信を開始し、DILコードがまだページ上にあるので、ページからAAMに直接ヒットを送ることもあり、データが2倍になります。 コードをDILからSSFに更新すると、この問題は緩和されます。

**注2:** データの重複が少なくてもデータにわずかな相違が生じる場合は、上記の手順1と2の順序を切り替えることができます。 コードをDILからSSFに移動すると、のSSFをオンにするスイッチを入れるまで、AAMへのデータフローが停止し [!UICONTROL report suite]ます。 一般的に、お客様は、訪問者をおよびに呼び込むのを見逃すよりも、データが少し倍増する方が [!UICONTROL traits] よいでし [!UICONTROL segments]ょう。

#### 移行のタイミング（多数のサイトがある場合）および [!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

このトピックでは、前の節で、主な戦略を以下にまとめて簡単に取り上げます。

一度に1つのサイト/[!UICONTROL report suite] (またはサイトのグループ/[!UICONTROL report suites])を移行します。

ただし、考えられるシナリオに基づいては、少し難しくなる場合があります。

* 複数の異なるサイトがある [!UICONTROL report suites]
* 複数のサイト(グローバル [!UICONTROL report suite] など [!UICONTROL report suite])を含む
* 1つのプロパティを使用して複数のサイトをカバーします [!DNL Launch]
* 異なるサイト用に異なる開発チームがある

これらの項目があるので、少し複雑になる可能性があります。 私が提案できる最善の方法は次のとおりです。

* 上で説明した内容に基づいて、SSFへの移行の戦略を立てるのに時間がかかります。
* 通常、 [!DNL Launch] (または単一の [!DNL AppMeasurement][!UICONTROL report suites]ファイルの)1つのプロパティが1つまたは2つの異なるグループに対して1つずつ対応付けられるという事実に基づいて、これらの異なるグループに対して1つずつ対応する計画を立て、企業をSSFに更新できる
* Adobeコンサルティングと連携している場合は、移行計画に関する相談を受け、必要に応じてコンサルティングの支援を受けるようにしてください

## 検証とトラブルシューティング {#validation-and-troubleshooting}

が起動して実行されていることを検証する主な方法 [!UICONTROL Server-Side Forwarding] は、アプリからのAdobe Analyticsヒットに対する応答を調べることです。

からAudience Managerへ [!UICONTROL server-side forwarding] のデータを行わない場合 [!DNL Analytics] 、（2x2ピクセル以外の） [!DNL Analytics] ビーコンには実際には応答しません。 ただし、SSFを実行している場合は、リクエストと応答で確認できる項目があります。この項目を使用すると、 [!DNL Analytics] Audience Managerと正しく通信していること、ヒットを転送していること、および応答を受け取っているこ [!DNL Analytics] とがわかります。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 誤った「成功」に気をつけてください。応答があり、すべてが機能しているように見える場合は、応答に「stuff」オブジェクトが含まれていることを確認してください。 そうしないと、次のメッセージが表示される場合があり [!DNL "status":"SUCCESS"]ます。 これは正しく機能していない配達確認です これが表示される場合は、またはでコードの更新が完了しているが、の転送がまだ完了していないこ [!DNL Launch] とを意味し [!DNL AppMeasurement][!DNL Analytics][!DNL Admin Console] ます。 この場合は、のでSSFが有効になっていることを確認する必要 [!DNL Analytics] があ [!DNL Admin Console] り [!UICONTROL report suite]ます。 バックエンドで必要な変更を行うのにそれほど時間がかかる場合があるので、まだ4時間も経っていない場合は、忍耐強くお願いします。

![誤った成功](assets/falsesuccess.png)

For more information about [!UICONTROL Server-Side Forwarding], please see the [documentation](https://marketing.adobe.com/resources/help/ja_JP/reference/ssf.html).
