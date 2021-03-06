---
title: 監視パターン
description: クラウドネイティブアプリケーションの可観測性パターン
ms.date: 02/05/2020
ms.openlocfilehash: a821235835b4553760b19887d500a29ca75e133e
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77448525"
---
# <a name="observability-patterns"></a>監視パターン

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

アプリケーションのコードのレイアウトを支援するために開発されたパターンと同じように、信頼性の高い方法でアプリケーションを運用するためのパターンがあります。 アプリケーションの管理において、**ログ記録**、**監視**、**アラート**という3つの便利なパターンが登場しました。

## <a name="when-to-use-logging"></a>ログ記録を使用する場合

どのように注意しても、アプリケーションはほとんどの場合、実稼働環境では予期しない方法で動作します。 ユーザーがアプリケーションに関する問題を報告する場合は、問題が発生したときにアプリの状況を確認できるようにすると非常に便利です。 アプリケーションが実行されているときの動作に関する情報をキャプチャする方法として最もよく見られるものの1つは、アプリケーションが実行している内容を書き留めておくことです。 このプロセスはログ記録と呼ばれます。 運用環境で障害や問題が発生した場合は、非運用環境でエラーが発生した状況を再現することが目標となります。 適切なログ記録を使用することにより、開発者はテストと実験が可能な環境で問題を再現するためのロードマップが得られます。

### <a name="challenges-when-logging-with-cloud-native-applications"></a>クラウドネイティブアプリケーションでのログ記録に関する課題

従来のアプリケーションでは、通常、ログファイルはローカルコンピューターに格納されます。 実際、Unix のようなオペレーティングシステムでは、すべてのログを保持するために定義されたフォルダー構造があります (通常は `/var/log`の下)。

モノリシックアプリのファイルにログ記録 ![ます。**図 7-1**を](./media/single-monolith-logging.png)
。 モノリシックアプリ内のファイルにログを記録する。

1台のコンピューター上のフラットファイルへのログ記録の有用性は、クラウド環境で大幅に削減されます。 ログを生成するアプリケーションはローカルディスクにアクセスできない場合があります。また、コンテナーは物理マシンに対してシャッフルされるため、ローカルディスクが非常に一時的なものである可能性があります。 複数のノード間でモノリシックアプリケーションを簡単にスケールアップすると、適切なファイルベースのログファイルを見つけることが困難になる場合があります。

スケーリングされたモノリシックアプリのファイルにログ記録 ![。**図 7-2**を](./media/multiple-node-monolith-logging.png)
。 スケーリングされたモノリシックアプリ内のファイルにログを記録する。

マイクロサービスアーキテクチャを使用して開発されたクラウドネイティブアプリケーションにも、ファイルベースの logger に関するいくつかの課題があります。 ユーザーの要求が、異なるコンピューター上で実行される複数のサービスにまたがることができるようになりました。また、ローカルファイルシステムへのアクセスをまったく持たないサーバーレス機能を含めることもできます。 このような多くのサービスやコンピューターで、ユーザーまたはセッションのログを相互に関連付けることは、非常に困難です。

マイクロサービスアプリのローカルファイルにログ記録 ![ます。**図 7-3**を](./media/local-log-file-per-service.png)
。 マイクロサービスアプリのローカルファイルにログを記録する。

最後に、一部のクラウドネイティブアプリケーションのユーザー数が多くなっています。 各ユーザーがアプリケーションにログインすると、500行のログメッセージが生成されると想像してください。 分離では、管理は容易ですが、10万ユーザーとログの量を超えると、ログの効果的な使用をサポートするために特別なツールが必要になります。

### <a name="logging-in-cloud-native-applications"></a>クラウドネイティブアプリケーションでのログ記録

すべてのプログラミング言語には、ログの書き込みを可能にするツールが用意されています。通常、これらのログを書き込むためのオーバーヘッドは低くなります。 ログ記録ライブラリの多くは、実行時にチューニングできるさまざまな種類の criticalities のログ記録を提供しています。 たとえば、 [Serilog ライブラリ](https://serilog.net/)は、次のログレベルを提供する .net 用の一般的な構造化ログライブラリです。

* "詳細"
* デバッグ
* Information
* 警告
* エラー
* 致命的

これらのさまざまなログレベルにより、ログの粒度が向上します。 アプリケーションが運用環境で正常に機能している場合は、重要なメッセージのみをログに記録するように構成できます。 アプリケーションの動作が不適切な場合は、より詳細なログが収集されるようにログレベルを上げることができます。 これにより、デバッグが容易になるというバランスでパフォーマンスが低下します。

ログツールの高パフォーマンスと、詳細レベルのチューニング機能により、開発者は頻繁にログを記録することをお勧めします。 多くの場合、各メソッドのエントリと終了をログに記録するパターンを優先します。 このアプローチは過剰な場合があるかもしれませんが、開発者がログ記録の量を少なくすることはあまりありません。 実際には、問題のあるメソッドのログ記録を追加することだけを目的としてデプロイを実行することは珍しくありません。 ログ記録が過剰になっていますが、それほど多くはありません。 この種のログを自動的に提供するために使用できるツールもあります。

クラウドネイティブアプリでファイルベースのログを使用することには問題があるため、一元化されたログを使用することをお勧めします。 ログは、アプリケーションによって収集され、ログのインデックスを作成して格納する中央ログアプリケーションに付属しています。 このクラスのシステムは、毎日数万のログを取り込むことができます。

また、多くのサービスにまたがるログ記録を作成する際には、いくつかの標準的なプラクティスに従うことをお勧めします。 たとえば、時間のかかる相互作用の開始時に[相関 ID](https://blog.rapid7.com/2016/12/23/the-value-of-correlation-ids/)を生成し、その相互作用に関連するメッセージごとにログを記録すると、関連するすべてのメッセージを検索しやすくなります。 1つのメッセージだけを検索し、相関 ID を抽出して、関連するすべてのメッセージを検索する必要があります。 別の例として、使用する言語やログライブラリにかかわらず、すべてのサービスでログ形式が同じであることを確認します。 この標準化により、ログの読み取りがはるかに簡単になります。 図7-4 は、マイクロサービスアーキテクチャがワークフローの一部として集中ログを活用する方法を示しています。

さまざまなソースからの ![ログは、集中管理されたログストアに取り込まれたます。](./media/centralized-logging.png)
**図 7-4**。 さまざまなソースからのログは、一元化されたログストアに取り込まれたます。

## <a name="challenges-with-detecting-and-responding-to-potential-app-health-issues"></a>アプリの正常性に関する問題の検出と対応に関する課題

アプリケーションによっては、ミッションクリティカルではないものもあります。 これらは内部的にのみ使用され、問題が発生した場合、ユーザーはチームに連絡して、アプリケーションを再起動できる可能性があります。 しかし、多くの場合、顧客は使用するアプリケーションに対して高い期待をしています。 ユーザーがアプリケーションを使用*する前*、またはユーザーに通知する前に、問題が発生したことを把握しておく必要があります。 それ以外の場合、最初に問題が発生したのは、アプリケーションまたは組織に deriding たソーシャルメディアの投稿が発生した場合です。

次のようなシナリオが考えられます。

- アプリケーションの1つのサービスで障害が発生して再起動し続けるため、応答が断続的に遅くなります。
- 1日のうちに、アプリケーションの応答時間が遅くなることがあります。
- 最近の配置の後、データベースに対する負荷は tripled しています。

適切に実装すると、問題の原因となる条件についての監視が可能になり、ユーザーが大きな影響を与える前に基になる条件に対処できます。

### <a name="monitoring-cloud-native-apps"></a>クラウドネイティブアプリの監視

一元化されたログ記録システムには、純粋なログ以外でテレメトリを収集するための追加の役割があります。 データベースクエリの実行時間、web サーバーからの平均応答時間、オペレーティングシステムによって報告された CPU 負荷の平均とメモリの負荷など、メトリックを収集できます。 これらのシステムでは、ログと組み合わせて、システム内のノードとアプリケーション全体の正常性を総合的に把握することができます。

監視ツールのメトリック収集機能は、アプリケーション内から手動で行うこともできます。 新規ユーザーのサインアップや注文の発注など、特に関心のあるビジネスフローは、中央監視システムのカウンターをインクリメントするようにインストルメント化することができます。 これにより、アプリケーションの正常性を監視するだけでなく、ビジネスの正常性を監視するために、監視ツールのロックが解除されます。

クエリは、特定の統計またはパターンを検索するために、ログ集計ツールで作成できます。これは、カスタムダッシュボードにグラフィカルな形式で表示できます。 多くの場合、チームは、アプリケーションに関連する統計情報をローテーションする、大きな壁にマウントされたディスプレイに投資します。 これにより、発生した問題を簡単に確認できます。

クラウドネイティブの監視ツールは、シングルプロセスのモノリシックアプリケーションであるか、分散マイクロサービスアーキテクチャであるかに関係なく、アプリに対してリアルタイムのテレメトリと洞察を提供します。 これには、アプリからのデータ収集を可能にするツールと、アプリの正常性に関する情報を照会して表示するためのツールが含まれます。

## <a name="challenges-with-reacting-to-critical-problems-in-cloud-native-apps"></a>クラウドネイティブアプリにおける重大な問題への対応に関する課題

アプリケーションの問題に対処する必要がある場合は、適切な担当者に通知する何らかの方法が必要です。 これは、3番目のクラウドネイティブアプリケーション可観測性パターンであり、ログ記録と監視に依存します。 アプリケーションでは、問題の診断を可能にするためにログを記録し、場合によっては監視ツールにフィードする必要があります。 アプリケーションメトリックと正常性データを1か所で集計するには、監視が必要です。 この設定が完了すると、特定のメトリックが許容可能なレベルを超えたときにアラートをトリガーするルールを作成できます。

一般に、アラートは、特定の条件によって緊急の問題をチームメンバーに通知する適切なアラートがトリガーされるように、監視の上位に階層化されます。 アラートが必要になるシナリオとしては、次のようなものがあります。

- 1分間のダウンタイムが発生しても、アプリケーションのサービスの1つが応答していません。
- アプリケーションが、1% を超える要求に対して失敗した HTTP 応答を返しています。
- アプリケーションのキーエンドポイントの平均応答時間が2000ミリ秒を超えています。

### <a name="alerts-in-cloud-native-apps"></a>クラウドネイティブアプリのアラート

監視ツールに対するクエリを作成して、既知のエラー状態を調べることができます。 たとえば、クエリで受信ログを検索して、web サーバー上の問題を示す HTTP 状態コード500を検出することができます。 これらのうちの1つが検出されるとすぐに、調査を開始できる送信元サービスの所有者に電子メールまたは SMS を送信することができます。

ただし、通常、1つの500エラーでは、問題が発生したことを特定するのに十分ではありません。 これは、ユーザーがパスワードを入力したか、形式が正しくないデータを入力したことを意味します。 警告クエリは、500エラーの平均値を超えた場合にのみ発生するように細工できます。

アラートに含まれる最も損傷のあるパターンの1つは、人が調査するアラートの数が多すぎることです。 サービスの所有者は、前に調査したエラーや問題が発生していないことがすぐに desensitized になります。 これにより、真のエラーが発生したときに、何百も偽陽性が発生した場合のノイズが失われます。 [かわいくウルフの人](https://en.wikipedia.org/wiki/The_Boy_Who_Cried_Wolf)は、この非常に危険なことを警告するために、多くの場合、子供に伝えます。 発生するアラートが実際の問題を示すものであることを確認することが重要です。

>[!div class="step-by-step"]
>[前へ](monitoring-health.md)
>[次へ](logging-with-elastic-stack.md)
