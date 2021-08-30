---
title: クライアント側のDILからサーバー側の転送へのサイトのAAM実装の移行
description: このチュートリアルは、Adobe Audience Manager(AAM)とAdobe Analyticsの両方を持ち、現在、「DIL」(Data Integration Library)コードを使用してページのヒットをAAMに送信し、ページのヒットをAdobe Analyticsに送信している場合に適用されます。 これらのソリューションは両方ともAdobe Experience Cloudに含まれるので、「サーバー側転送(SSF)」をオンにするベストプラクティスに従えば、Analyticsデータ収集サーバーは、クライアント側コードでページからAAMに追加のヒットを送信する代わりに、リアルタイムでサイト分析データをAudience Managerに転送できます。 このチュートリアルでは、古い「クライアント側DIL」実装から新しい「サーバー側転送」方法に切り替える手順を説明します。
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
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '2318'
ht-degree: 0%

---

# サイトのAAM実装を[!DNL Client-Side]DILから[!DNL Server-Side Forwarding]に移行する {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

このチュートリアルは、Adobe Audience Manager(AAM)とAdobe Analyticsの両方を使用し、現在、「DIL」([!DNL Data Integration Library])コードを使用してページからAAMにヒットを送信し、さらにページからAdobe Analyticsにヒットを送信している場合に適用されます。 これらのソリューションは両方ともAdobe Experience Cloudに含まれているので、ベストプラクティスに従う機会があります。「[!DNL Server-Side Forwarding](SSF)」をオンにすると、[!DNL client-side]コードでページからAAMに追加のヒットを送信する代わりに、[!DNL Analytics]データ収集サーバーがリアルタイムでサイト分析データをAudience Managerに転送できます。 このチュートリアルでは、古い「[!DNL Client-Side DIL]」実装から新しい「[!DNL Server-Side forwarding]」メソッドへの切り替えの手順を説明します。

## [!DNL Client-Side] (DIL)と  [!DNL Server-Side] {#client-side-dil-vs-server-side}

Adobe AnalyticsのデータをAAMに送信するこれら2つの方法を比較および対比する場合、まず次の画像の違いを視覚化すると役に立ちます。

![クライアント側からサーバー側へ](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL の実装 {#client-side-dil-implementation}

この方法を使用してAdobe AnalyticsデータをAAMに送信する場合、Webページから2つのヒットが得られます。1つは[!DNL Analytics]に移動し、もう1つはWebページ上の[!DNL Analytics]データをコピーした後にAAMに移動します。 [!UICONTROL Segments] はAAMからページに返され、そこでパーソナライゼーションなどに使用できます。これは「レガシー」実装と見なされ、お勧めしません。

これはベストプラクティスに従っていない点以外に、この方法を使用するデメリットは次のとおりです。

* 1つではなく、ページからの2つのヒット
* [!UICONTROL Server-Side Forwarding] は、AAMオーディエンスをにリアルタイムで共有する [!DNL Analytics]場合に必要なの [!DNL Client-side] で、実装では、この機能（および将来他の機能が使用される可能性がある）を許可しません

AAM実装の[!UICONTROL Server-Side Forwarding]メソッドに移行することをお勧めします。

### [!UICONTROL Server-Side Forwarding] 実装 {#server-side-forwarding-implementation}

上の画像に示すように、WebページからAdobe Analyticsにヒットが送信されます。 [!DNL Analytics] 次に、そのデータをリアルタイムでAAMに転送し、訪問者は、ページから直接ヒットが来たかのよう [!UICONTROL traits] に、 [!UICONTROL segments] AAMとに評価されます。

[!UICONTROL Segments] は、同じリアルタイムヒットでに返されま [!DNL Analytics]す。これは、パーソナライゼーション用にWebページに応答を転送します。

サーバー側転送に移行するタイミングを下げることはできません。 Audience Managerと[!DNL Analytics]の両方を持つすべてのユーザーに、この実装方法を利用することを強くお勧めします。

## 主なタスクは2つあります {#you-have-two-main-tasks}

このページにはかなりの情報が含まれていますが、もちろんすべてが重要です。 ただし、**すべては、**&#x200B;する必要がある2つの主なことに要約されます。

1. コードを[!DNL Client-Side]DILコードから[!UICONTROL Server-Side Forwarding]コードに変更します
1. [!DNL Analytics] [!DNL Admin Console]のスイッチを反転して、（[!UICONTROL report suite]ごとに）実際のデータ転送を開始します。

この2つのいずれかをスキップすると、SSFは正しく動作しません。 このドキュメントに手順と追加データが追加され、お使いの設定でこれら2つの手順を正しく実行できるようになりました。

## 実装オプション {#implementation-options}

[!DNL client-side]から[!DNL server-side]に移行する際には、次の作業の1つで、コードを新しい[!UICONTROL Server-Side Forwarding]コードに変更します。 これは、次のいずれかのオプションを使用しておこないます。

* Adobe Experience Platform Launch - Webプロパティに推奨される実装オプションです。 [!DNL Launch]が全ての難しい作業を行ったので、これは非常に簡単な作業です。
* ページ —AdobeLaunchを使用していない（まだ使用していない）場合は、新しいSSFコードを[!DNL appMeasurement.js]ファイル内の`doPlugins`関数に直接配置することもできます
* 他のタグマネージャー — これらは、他のタグマネージャーが[!DNL AppMeasurement]コードを保存している場所にSSFコードを`doPlugins`に配置するので、前の（ページ上の）オプションと同じように扱うことができます

コードの更新の節で以下の各項目を確認します。

## 導入手順 {#implementation-steps}

### 手順0:前提条件：Experience CloudIDサービス(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

[!UICONTROL Server-Side Forwarding]に移行する主な前提条件は、Experience CloudIDサービスを実装することです。 これは、Experience Platform Launchを使用している場合に最も簡単におこなえます。その場合は、ECID拡張機能をインストールするだけで、残りの操作を実行できます。

Adobe以外のTMSを使用している場合、またはTMSがまったくない場合は、**他のAdobeソリューションの前に**&#x200B;を実行するようにECIDを実装してください。 詳しくは、[ECIDのドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html)を参照してください。 その他の前提条件はコードバージョンに関するものです。以下の手順で最新バージョンのコードを適用するだけで問題ありません。

>[!NOTE]
>
>実装する前に、このドキュメント全体をお読みください。 以下の「タイミング」の節は、ECIDを含む&#x200B;*タイミング*&#x200B;に関する重要な情報を示しています（まだ実装されていない場合）。

### 手順1:現在使用されているオプションをDILコードから記録 {#step-record-currently-used-options-from-dil-code}

[!DNL Client-Side]DILコードから[!UICONTROL Server-Side Forwarding]に移行する準備が整ったら、最初の手順は、AAMに送信されるカスタム設定やデータなど、DILコードでおこなっているすべての操作を特定することです。 注意事項と考慮事項は次のとおりです。

* [!DNL siteCatalyst.init]DILモジュールを使用した通常の[!DNL Analytics]変数 — この変数について心配する必要はありません。これは、通常の[!DNL Analytics]変数を送り返すだけで、SSFが有効になっているだけで発生するからです。
* Partnerサブドメイン —DIL.create関数で、`partner`パラメーターをメモします。 これは「パートナーサブドメイン」または「パートナーID」と呼ばれ、新しいSSFコードを配置する際に必要になります。
* [!DNL Visitor Service Namespace] - 「[!DNL Org ID]」や「[!DNL IMS Org ID]」とも呼ばれ、新しいSSFコードを設定する際にもこれが必要です。書き留めておけ。
* containerNSID、uuidCookie、およびその他の高度なオプション — 使用しているその他の高度なオプションをメモしておき、SSFコードでも設定できるようにします。
* 追加のページ変数 — （siteCatalyst.initで処理される通常の[!DNL Analytics]変数に加えて）ページからAAMに他の変数が送信される場合、SSF(スポイラーアラート：[!DNL contextData]変数を使用)。

### 手順2:コードの更新 {#step-updating-the-code}

上記の「実装オプション」の節では、[!UICONTROL Server-Side Forwarding]を実装する方法と場所に関して複数のオプションが表示されます。 この節を有効にするには、これらの節に分割する必要があります（そのうち2つを組み合わせる）。 必要に応じて、この節の方法を参照してください。

#### Adobe Experience Platform Launch {#launch-by-adobe}

Experience Platform Launchの[!DNL Client-Side]DILコードから[!UICONTROL Server-Side Forwarding]に実装オプションを移行する方法については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「ページ上」またはAdobe Tag Manager以外 {#on-the-page-or-non-adobe-tag-manager}

ファイル内またはAdobe以外のタグ管理システム内にある[!DNL Client-Side]DILコードから[!DNL AppMeasurement]コード内の[!UICONTROL Server-Side Forwarding]に実装オプションを移行する方法については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 手順3:転送の有効化（[!UICONTROL Report Suite]ごと） {#step-enabling-the-forwarding-per-report-suite}

これまで、コードを[!DNL Client-Side DIL]コードから[!UICONTROL Server-Side Forwarding]コードに切り替えるのに時間をかけてきました。 それは難しい部分だからいい。 この節は、非常に簡単ですが、コードを更新するのと同じくらい重要です。 このビデオでは、AnalyticsからAudience Managerへの実際のデータ転送を可能にするスイッチの切り替え方法を確認します。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 転送がExperience Cloudバックエンドで完全に実装されるまで、最大4時間かかることに注意してください。

## タイミング {#timing}

注意： [!DNL Client-Side DIL]から[!UICONTROL Server-Side Forwarding]に移行する際の主な作業は次の2つです。

1. コードの更新
1. [!DNL Analytics] [!DNL Admin Console]でスイッチを反転

でも問題はどちらが先か？ 問題か？ 2つの質問です しかし、答えは…に依存しているし、**&#x200B;問題になる。 どうです？ 分けてみよう。 しかし、まず、多くのサイトを持つ大規模な組織の場合に出る可能性のある追加の質問を次に示します。私は一度に全てを行わなければなりませんか？ それは少し簡単です。 いいえ。 一つ一つで…:)

### もう少し深い潜り込み {#a-little-deeper-dive}

タイミングと注文が重要な理由は、次の技術的事実に要約できる、転送の仕組みが原因です。

* Experience CloudIDサービス(ECID)を実装済みで、[!DNL Analytics] [!DNL Admin Console](「switch」)のスイッチがオンの場合、コードをまだ更新していなくても、データは[!DNL Analytics]からAAMにWILL転送されます。
* ECIDが実装されていない場合、スイッチがオンでSSFコードが含まれていても、データは転送されません。
* SSFコード（[!DNL Launch]内かページ上かに関わらず）は、応答を実際に処理し、もちろん移行を完了する必要があります。
* SSFスイッチは[!UICONTROL Report Suite]によって有効になりますが、コードは[!DNL Launch]のプロパティまたは[!DNL AppMeasurement]ファイルで処理されます（[!DNL Launch]を使用しない場合）。

### ベストプラクティス {#best-practices}

これらの技術的な詳細に基づき、次に「どのようなタイミングで」をおこなうかを推奨します。

#### ECIDがまだ実装されていない場合 {#if-you-do-not-have-ecid-yet-implemented}

1. SSFに対して有効にする[!UICONTROL report suite]ごとに[!DNL Analytics]でスイッチを入れます。

   1. ECIDがないので、転送はまだ開始されません

1. サイトごとに、コードを[!DNL Client-Side DIL]からSSFに更新します（上記の別の節で説明したように、[!DNL Launch]またはページ上にあります）。

   1. 転送は（ECIDを追加したので）流れるようになり、[!DNL Analytics]ビーコンに対する適切なJSON応答も受け取るはずです（詳しくは、以下の検証とトラブルシューティングの節を参照）。

#### ECIDが実装されている場合 {#if-you-do-have-ecid-implemented}

1. SSFを有効にするために、DILからSSF PER [!UICONTROL report suite]にコードを更新する準備が整うように準備および計画します。

   1. [!DNL Analytics]でスイッチを反転し、SSFを有効にします。

      1. ECIDが有効になっているので、転送が開始されます
   1. 可能な限り早く、コードを[!DNL Client-Side DIL]からSSFに更新します（上記の別の節で説明したように、[!DNL Launch]またはページ上にある可能性があります）。

      1. [!DNL Analytics]ビーコンに対する適切なJSON応答を受け取る必要があります（詳しくは、以下の検証とトラブルシューティングの節を参照してください）。


**注意1:** これら2つの手順をできるだけ近くに実行することが重要です。上記の手順1 ～ 2の間では、AAMに入るデータの重複が生じるからです。つまり、SSFは[!DNL Analytics]からAAMへのデータの送信を開始し、DILコードがまだページにあるので、ページからAAMに直接ヒットする場合もあります。その結果、データが2倍になります。 コードをDILからSSFに更新すると、これが軽減されます。

**注意2:** データの重複が少なくなるのではなく、データに小さな相違がある場合は、上記の手順1と2の順序を切り替えることができます。DILからSSFにコードを移動すると、[!UICONTROL report suite]のSSFをオンにするスイッチを入れるまで、データフローがAAMに送られなくなります。 通常、訪問者が[!UICONTROL traits]と[!UICONTROL segments]に入らないようにするよりも、データが少し倍増します。

#### 多数のサイトが存在し、[!UICONTROL Report Suites]がある場合の移行タイミング {#migration-timing-when-you-have-many-sites-and-report-suites}

このトピックは、前の節で簡単に取り上げ、主な戦略を次のように要約できます。

一度に1つのサイト/[!UICONTROL report suite]（またはサイトのグループ/[!UICONTROL report suites]）を移行します。

ただし、考えられるシナリオに基づいては、これは少し難しくなる可能性があります。

* 複数の異なる[!UICONTROL report suites]を含むサイトがある
* 複数のサイト（グローバル[!UICONTROL report suite]など）を含む[!UICONTROL report suite]がある場合
* 1つの[!DNL Launch]プロパティを使用して、複数のサイトを対象にします
* 異なるサイト用に異なる開発チームがいる

これらのアイテムのため、少し複雑になる可能性があります。 私が提案できる最善の事は次のとおりです。

* 上記で説明した内容に基づいて、SSFへの移行戦略を策定するには、しばらく時間がかかります
* [!DNL Launch]（または1つの[!DNL AppMeasurement]ファイル）の1つのプロパティが通常1つまたは2つの個別の[!UICONTROL report suites]にマッピングされるので、エンタープライズをSSFに更新し、これらの個別のグループで1つずつ動作する計画を立てることができます
* Adobeコンサルティングと連携している場合は、移行計画について相談し、必要に応じてサポートを提供してください

## 検証とトラブルシューティング {#validation-and-troubleshooting}

[!UICONTROL Server-Side Forwarding]が起動および実行されていることを検証する主な方法は、アプリからのAdobe Analyticsヒットへの応答を調べることです。

[!DNL Analytics]からAudience Managerに対して[!UICONTROL server-side forwarding]データを実行していない場合、（2x2ピクセル以外に）[!DNL Analytics]ビーコンに対する応答はありません。 ただし、SSFを実行している場合は、[!DNL Analytics]リクエストと応答で確認できる項目があり、[!DNL Analytics]がAudience Managerと正しく通信していること、ヒットを転送して応答を取得していることを確認できます。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 偽の「成功」に注意してください — 応答があり、すべてが機能しているように見える場合は、応答に「stuff」オブジェクトがあることを確認してください。そうしないと、[!DNL "status":"SUCCESS"]というメッセージが表示される場合があります。 変に思えますが、これは実際には正しく機能していない証拠です。 これが表示された場合、[!DNL Launch]または[!DNL AppMeasurement]のコード更新は完了していますが、[!DNL Analytics] [!DNL Admin Console]の転送はまだ完了していません。 この場合は、[!UICONTROL report suite]の[!DNL Analytics] [!DNL Admin Console]でSSFを有効にしていることを確認する必要があります。 バックエンドで必要な変更をすべておこなうのにそれほど時間がかかる場合があり、まだ4時間が経過していない場合は、しばらくお待ちください。

![誤った成功](assets/falsesuccess.png)

[!UICONTROL Server-Side Forwarding]について詳しくは、[ドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)を参照してください。
