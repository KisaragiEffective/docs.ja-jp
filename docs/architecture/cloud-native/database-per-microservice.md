---
title: マイクロサービスごとのデータベース
description: モノリシックアプリケーションとクラウドネイティブアプリケーションでのデータストレージのコントラストを実現します。
author: robvet
ms.date: 01/22/2020
ms.openlocfilehash: c0c5611fa866d70f155e4bdad2eee1181b13c065
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79141446"
---
# <a name="database-per-microservice"></a>マイクロサービスごとのデータベース

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

このマニュアルで説明したように、クラウド ネイティブのアプローチによって、アプリケーションの設計、デプロイ、および管理方法が変わります。 また、データの管理方法や保存方法も変更されます。

図 5-1 は、その違いを対比しています。

![クラウドネイティブアプリケーションでのデータストレージ](./media/distributed-data.png)

**図 5-1**. クラウドネイティブアプリケーションでのデータ管理

経験豊富な開発者は、図 5-1 の左側にあるアーキテクチャを簡単に認識できます。 この*モノリシック アプリケーション*では、ビジネス サービス コンポーネントは共有サービス層にまとめて配置され、単一のリレーショナル データベースのデータを共有します。

多くの点で、単一のデータベースはデータ管理をシンプルに保ちます。 複数のテーブル間でデータをクエリするのは簡単です。 データ更新の変更をまとめて行うか、すべてロールバックします。 [ACID トランザクションは、強力で](https://docs.microsoft.com/windows/desktop/cossdk/acid-properties)即時の一貫性を保証します。

クラウド ネイティブ向けの設計では、別のアプローチを採用しています。 図 5-1 の右側で、ビジネス機能が小規模で独立したマイクロサービスにどのように分離されているのかを確認します。 各マイクロサービスは、特定のビジネス機能と独自のデータをカプセル化します。 モノリシック データベースは、多数の小規模なデータベースを持つ分散データ モデルに分解され、それぞれがマイクロサービスに合わせて配置されます。 煙が晴れると、*マイクロサービスごとにデータベース*を公開するデザインが現れます。

## <a name="why"></a>なぜですか?

マイクロサービスごとにこのデータベースは、特に急速に進化し大規模な規模をサポートする必要があるシステムに多くの利点をもたらします。 このモデルで.

- ドメイン データはサービス内にカプセル化されます。
- データ スキーマは他のサービスに直接影響を与えることなく進化できます。
- 各データ ストアは個別に拡張可能
- 1 つのサービスでデータ ストアの障害が発生しても、他のサービスに直接影響を与えることはありません。

データの分離により、各マイクロサービスは、ワークロード、ストレージのニーズ、および読み取り/書き込みパターンに最適なデータ ストアの種類を実装できます。 選択肢には、リレーショナル、ドキュメント、キー値、グラフベースのデータストアなどがあります。

図 5-2 は、クラウドネイティブ システムにおけるポリグロットの持続性の原則を示しています。

![多項グロデータの永続性](./media/polyglot-data-persistence.png)

**図 5-2**. 多項グロデータの永続性

前の図では、各マイクロサービスが異なる種類のデータ ストアをサポートする方法に注意してください。

- 製品カタログ マイクロサービスは、基になるデータの豊富なリレーショナル構造に対応するためにリレーショナル データベースを使用します。
- ショッピング カート マイクロサービスは、シンプルなキー値データ ストアをサポートする分散キャッシュを使用します。
- 注文マイクロサービスは、大量の読み取り操作に対応するために、書き込み操作に対して NoSql ドキュメント データベースと、高度に非正規化されたキー/値ストアの両方を使用します。
  
リレーショナル データベースは複雑なデータを持つマイクロサービスに関連していますが、NoSQL データベースはかなりの人気を集めています。 大規模な規模と高可用性を提供します。 スキーマを持たない性質により、開発者は、変更にコストと時間がかかる型指定されたデータ クラスや ORM のアーキテクチャから離れて移動できます。 この章では、後ほど NoSQL データベースについて説明します。

 データを別々のマイクロサービスにカプセル化することで、俊敏性、パフォーマンス、およびスケーラビリティを向上させることができますが、多くの課題も伴います。 次のセクションでは、これらの課題と、それらを克服するためのパターンと実践について説明します。  

## <a name="cross-service-queries"></a>クロスサービス クエリ

マイクロサービスは独立しており、在庫、出荷、発注などの特定の機能に重点を置いていますが、多くの場合、他のマイクロサービスとの統合が必要になります。 多くの場合、統合には、あるマイクロサービスが別のマイクロサービス*にデータを照会する*必要があります。 図 5-3 にシナリオを示します。

![マイクロサービス間でのクエリ](./media/cross-service-query.png)

**図 5-3**. マイクロサービス間でのクエリ

前の図では、ユーザーの買い物かごにアイテムを追加するショッピング バスケット マイクロサービスが表示されます。 このマイクロサービスのデータ ストアにはバスケットと品目のデータが含まれていますが、商品データや価格データは保持されません。 代わりに、これらのデータ項目はカタログと価格マイクロサービスによって所有されます。 これは問題を提示します。 買い物かごマイクロサービスは、そのデータベースに製品や価格設定データがない場合、どのようにユーザーの買い物かごに製品を追加できますか?

第 4 章で説明したオプションの 1 つは、ショッピング カートからカタログおよび価格マイクロサービスへの[直接 HTTP 呼び出し](service-to-service-communication.md#queries)です。 しかし、第 4 章では、同期 HTTP 呼び出し*はマイクロサービスを結合*し、その自律性を低下させ、アーキテクチャ上の利点を減らすことを説明しました。

また、各サービスに対して、個別の受信キューと送信キューを持つ要求/応答パターンを実装することもできます。 ただし、このパターンは複雑であり、要求メッセージと応答メッセージを関連付けるために配管が必要です。
バックエンド マイクロサービス呼び出しの分離は行われますが、呼び出し元のサービスは、呼び出しが完了するまで同期的に待機する必要があります。 ネットワークの輻輳、一時的な障害、または過負荷のマイクロサービスが発生し、長時間実行され、さらに失敗する可能性があります。

その代わり、クロスサービスの依存関係を削除するための広く受け入れられているパターンは、図 5-4 に示す[マテリアライズビューパターン](https://docs.microsoft.com/azure/architecture/patterns/materialized-view)です。

![マテリアライズビューパターン](./media/materialized-view-pattern.png)

**図 5-4**. マテリアライズビューパターン

このパターンでは、買い物バスケット サービスにローカル データ テーブル (*読み取りモデル*) を配置します。 このテーブルには、製品および価格マイクロサービスから必要なデータの非正規化コピーが含まれています。 データをショッピング バスケット マイクロサービスに直接コピーすると、コストのかかるクロスサービス呼び出しが不要になります。 サービスに対してローカルなデータを使用すると、サービスの応答時間と信頼性が向上します。 さらに、データの独自のコピーを持つことで、ショッピング バスケット サービスの弾力性が向上します。 カタログ サービスが利用できなくなった場合、ショッピング バスケット サービスに直接影響を与えることはありません。 買い物かごは、自分の店舗からのデータで操作を続けることができます。

このアプローチのキャッチは、システム内に重複データが存在することです。 しかし、クラウドネイティブシステムでデータを*戦略的に*複製することは確立された慣行であり、アンチパターンや悪い習慣とは考えられません。 *1 つのサービスだけが*データ・セットを所有し、そのデータ・セットに対する権限を持つことができることを覚えておいてください。 レコードのシステムが更新されたときに読み取りモデルを同期する必要があります。 同期は、通常、[パブリッシュ/サブスクライブ パターン](service-to-service-communication.md#events)を使用した非同期メッセージングを介して実装されます (図 5.4 を参照)。

## <a name="distributed-transactions"></a>分散トランザクション

マイクロサービス間でデータを照会するのは困難ですが、複数のマイクロサービス間でのトランザクションの実装はさらに複雑になります。 異なるマイクロサービス内の独立したデータ ソース間でデータの一貫性を維持するという固有の課題は、過小評価することはできません。 クラウド ネイティブ アプリケーションで分散トランザクションが不足している場合は、分散トランザクションをプログラムで管理する必要があります。 *あなたは、即時の一貫性*の世界から*最終的な一貫性*の世界に移動します。

図 5-5 に問題を示します。

![サガ パターンでのトランザクション](./media/saga-transaction-operation.png)

**図 5-5**. マイクロサービス間でのトランザクションの実装

前の図では、5 つの独立したマイクロサービスが、注文を作成する分散トランザクションに参加しています。 各マイクロサービスは、独自のデータ ストアを保持し、そのストアのローカル トランザクションを実装します。 注文を作成するには、*個々*のマイクロサービスのローカル トランザクションが成功するか、*または操作をすべて*中止してロールバックする必要があります。 組み込みのトランザクション サポートは各マイクロサービス内で利用できますが、データの一貫性を維持するために 5 つのサービス全体にわたる分散トランザクションのサポートはありません。

代わりに、この分散トランザクションを*プログラムで*構築する必要があります。

分散トランザクションサポートを追加する一般的なパターンは、Sagaパターンです。 ローカル トランザクションをプログラムでグループ化し、各トランザクションを順次呼び出すことによって実装されます。 いずれかのローカル トランザクションが失敗した場合、Saga は操作を中止し、一連の[補正トランザクションを](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction)呼び出します。 補正トランザクションは、前のローカル トランザクションによって行われた変更を元に戻し、データの一貫性を復元します。 図 5-6 は、Saga パターンを使用して失敗したトランザクションを示しています。

![サガパターンでロールバック](./media/saga-rollback-operation.png)

**図 5-6**. トランザクションのロールバック

前の図では、*在庫の更新*操作は、インベントリ のマイクロサービスで失敗しました。 佐賀は、一連の補正トランザクション (赤色) を呼び出して、在庫数を調整し、支払と注文をキャンセルし、各マイクロサービスのデータを一貫した状態に戻します。

佐賀パターンは、通常、関連する一連のイベントとして振り付け、または関連するコマンドのセットとして調整されます。 第 4 章では、オーケストレーションサガ実装の基礎となるサービス アグリゲータ パターンについて説明しました。 また、振り付けサガの実装の基礎となる Azure サービス バスと Azure イベント グリッドのトピックと共にイベントについて説明しました。

## <a name="high-volume-data"></a>大量データ

大規模なクラウド ネイティブ アプリケーションは、多くの場合、大量のデータ要件をサポートします。 このようなシナリオでは、従来のデータ ストレージ手法によってボトルネックが発生する可能性があります。 大規模に展開する複雑なシステムでは、コマンドとクエリ責任の分離 (CQRS) とイベント ソーシングの両方で、アプリケーションのパフォーマンスが向上する可能性があります。  

### <a name="cqrs"></a>CQRS

[CQRS](https://docs.microsoft.com/azure/architecture/patterns/cqrs)は、パフォーマンス、スケーラビリティ、およびセキュリティを最大限に高めるアーキテクチャ パターンです。 このパターンは、データを読み取る操作と、データを書き込む操作を分離します。

通常のシナリオでは、同じエンティティ モデルとデータ リポジトリ オブジェクトが読み取りと書き込みの*両方*の操作に使用されます。

ただし、大量のデータ シナリオでは、読み取りと書き込みの個別のモデルとデータ テーブルの利点があります。 パフォーマンスを向上させるために、読み取り操作は、データの高度に非正規化された表現に対してクエリを実行し、負荷のかかる反復的なテーブル結合とテーブル ロックを回避できます。 *コマンド*と呼ばれる*書き込み*操作は、一貫性を保証する完全に正規化されたデータ表現に対して更新されます。 次に、両方の表現を同期するメカニズムを実装する必要があります。通常、書き込みテーブルは、変更が変更されるたびに、読み取りテーブルに変更をレプリケートするイベントを発行します。

図 5-7 は、CQRS パターンの実装を示しています。

![コマンドとクエリの責任の分離](./media/cqrs-implementation.png)

**図 5-7**. CQRS の実装

前の図では、個別のコマンド モデルとクエリ モデルが実装されています。 各データ書き込み操作は書き込みストアに保存され、その後、読み取りストアに反映されます。 データ伝播プロセスが[最終的な一貫性](http://www.cloudcomputingpatterns.org/eventual_consistency/)の原則にどのように作用するかに細心の注意を払う。 読み取りモデルは最終的に書き込みモデルと同期しますが、プロセスに若干の遅れがある可能性があります。 最終的な一貫性については、次のセクションで説明します。

この分離により、読み取りと書き込みを個別にスケーリングできます。 読み取り操作ではクエリ用に最適化されたスキーマが使用されますが、書き込みでは更新用に最適化されたスキーマが使用されます。 読み取りクエリは非正規化されたデータに対して行われますが、複雑なビジネス ロジックは書き込みモデルに適用できます。 また、読み取りを公開する場合よりも、書き込み操作に対して厳しいセキュリティを課す場合もあります。

CQRS を実装すると、クラウド ネイティブ サービスのアプリケーション パフォーマンスを向上させることができます。 ただし、より複雑な設計になります。 この原則は、クラウドネイティブアプリケーションのセクションに慎重かつ戦略的に適用します。 CQRS の詳細については、 Microsoft の著書[「.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ](https://docs.microsoft.com/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/apply-simplified-microservice-cqrs-ddd-patterns)」を参照してください。

### <a name="event-sourcing"></a>イベントソーシング

大量のデータ シナリオを最適化するもう 1 つの方法として[は、イベント ソーシング](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)が含まれます。

システムは通常、データ エンティティの現在の状態を格納します。 たとえば、ユーザーが電話番号を変更すると、顧客レコードが新しい番号で更新されます。 データ エンティティの現在の状態は常にわかりますが、各更新プログラムは以前の状態を上書きします。

ほとんどの場合、このモデルは正常に動作します。 ただし、大量のシステムでは、トランザクション ロックと頻繁な更新操作によるオーバーヘッドがデータベースのパフォーマンス、応答性、およびスケーラビリティの制限に影響を与える可能性があります。

イベント ソーシングでは、データのキャプチャに異なるアプローチを採用しています。 データに影響を与える各操作は、イベント ストアに永続化されます。 データ レコードの状態を更新する代わりに、経理担当の元帳と同様に、各変更を過去のイベントの連続したリストに追加します。 イベントストアは、データの記録のシステムになります。 マイクロサービスの境界コンテキスト内でさまざまなマテリアライズドビューを伝播するために使用されます。 図 5.8 にパターンを示します。

![イベント ソーシング](./media/event-sourcing.png)

**図 5-8**. イベント ソーシング

前の図では、ユーザーのショッピング カートの各エントリ (青色) が基になるイベント ストアに追加される方法に注意してください。 隣接するマテリアライズドビューでは、各ショッピングカートに関連付けられたすべてのイベントを再生することによって、現在の状態が投影されます。 このビュー(読み取りモデル)は、UI に公開されます。 イベントは、外部システムやアプリケーションと統合したり、エンティティの現在の状態を判断するためにクエリを実行したりすることもできます。 このアプローチでは、履歴を保持します。 エンティティの現在の状態だけでなく、この状態に到達した方法も知っています。

機械的に言えば、イベント ソーシングは書き込みモデルを単純化します。 更新または削除はありません。 各データ エントリを変更できないイベントとして追加すると、リレーショナル データベースに関連する競合、ロック、および同時実行の競合を最小限に抑えることができます。 マテリアライズドビューパターンを使用して読み取りモデルを構築すると、書き込みモデルからビューを切り離し、最適なデータ ストアを選択してアプリケーション UI のニーズを最適化できます。

このパターンでは、イベント ソーシングを直接サポートするデータ ストアを検討してください。 Azure コスモス DB、モンゴデータベース、カサンドラ、カウチDB、および RavenDB が適しています。

すべてのパターンとテクノロジと同様に、戦略的に、必要に応じて実装します。 イベント ソーシングはパフォーマンスとスケーラビリティを向上させますが、複雑さと学習曲線を犠牲にします。

>[!div class="step-by-step"]
>[前次](service-mesh-communication-infrastructure.md)
>[Next](relational-vs-nosql-data.md)
