---
title: フィルター処理
ms.date: 03/30/2017
ms.assetid: 4002946c-e34a-4356-8cfb-e25912a4be63
ms.openlocfilehash: efbedc16fe48d83cdc4223862bc691e9cbe15c10
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964287"
---
# <a name="filtering"></a>フィルター処理
Windows Communication Foundation (WCF) フィルター処理システムは、宣言型フィルターを使用してメッセージを照合し、操作上の決定を行うことができます。 フィルターを使用してメッセージの一部を調べることで、そのメッセージで必要な操作を決定できます。 たとえば、キュー プロセスでは、XPath 1.0 クエリを使用して既知のヘッダー優先度要素をチェックし、メッセージをキューの先頭に移動するべきかどうかを決定します。  
  
 フィルター処理システムは、特定の WCF メッセージに対してどのフィルターセットが `true` されているかを効率的に判断できる一連のクラスで構成されています。  
  
 フィルター処理システムは、WCF メッセージングのコアコンポーネントです。非常に高速になるように設計されています。 各フィルターの実装は、WCF メッセージに対する特定の種類の照合に対して最適化されています。  
  
 フィルター処理システムは、スレッド セーフではありません。 アプリケーションは、すべてのロック セマンティクスを処理する必要があります。 ただし、(スレッドに対する) マルチ リーダー/シングル ライターはサポートされています。  
  
## <a name="where-filtering-fits"></a>フィルター処理が適する場合  
 フィルター処理は、メッセージを適切なアプリケーション コンポーネントにディスパッチする処理の一部であり、メッセージの受信後に行われます。 フィルター処理システムの設計では、メッセージング、ルーティング、セキュリティ、イベント処理、システム管理など、いくつかの WCF サブシステムの要件に対応しています。  
  
## <a name="filters"></a>フィルター  
 フィルター エンジンには、フィルターとフィルター テーブルという 2 つの主要コンポーネントが含まれます。 フィルターは、ユーザーが指定した論理条件に基づいてメッセージに関する論理判定を行います。 フィルターは <xref:System.ServiceModel.Dispatcher.MessageFilter> クラスを実装します。  
  
 メッセージがフィルターの一致条件を満たしているかどうかを判定するには、<xref:System.ServiceModel.Dispatcher.MessageFilter.Match%2A> メソッドを使用します。 これらのメソッドの中の 1 つは、メッセージのヘッダーをテストしますが、メッセージ本文は検査できません。 もう1つのメソッドは、*メッセージバッファー*を入力パラメーターとして受け取り、メッセージ本文を検査できます。  
  
 通常、フィルターは個別にテストされるのではなく、フィルター テーブルの一部としてテストされます。フィルター テーブルは、<xref:System.ServiceModel.Dispatcher.MessageFilterTable%601.CreateFilterTable%2A> メソッドによって作成されるジェネリック クラスです。  
  
 数種類のフィルターがあり、それぞれが特定の種類のブール型の条件での照合処理に特化されています。 フィルターを作成した後に、フィルターが使用する条件を変更することはできません。フィルターの条件を変更するには、新しいフィルターを作成し、既存のフィルターを削除します。  
  
### <a name="action-filters"></a>アクション フィルター  
 <xref:System.ServiceModel.Dispatcher.ActionMessageFilter> には、アクション文字列の一覧が含まれます。 フィルターの一覧にあるアクションのいずれかが、メッセージまたはメッセージ バッファーにある Action ヘッダーに一致した場合、`Match` メソッドは `true` を返します。 この一覧が空である場合、フィルターはすべてに一致するフィルターと見なされるため、メッセージまたはメッセージ バッファーはすべて一致することになり、`Match` メソッドは `true` を返します。 フィルターの一覧にあるすべてのアクションが、メッセージまたはメッセージ バッファーにある Action ヘッダーに一致しない場合、`Match` メソッドは `false` を返します。 メッセージにアクションがなく、フィルターの一覧が空でない場合、`Match` メソッドは `false` を返します。  
  
### <a name="endpoint-address-filters"></a>エンドポイント アドレス フィルター  
 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> は、メッセージのヘッダー コレクションに示されるエンドポイント アドレスに基づいて、メッセージおよびメッセージ バッファーのフィルター処理を行います。 メッセージがこのようなフィルターを通過するには、次の条件を満たす必要があります。  
  
- フィルターのアドレス URI (Uniform Resource Identifier) がメッセージの To ヘッダーのアドレスと同じであること。  
  
- フィルターのアドレス (`address.Headers` コレクション) にある各エンドポイント パラメーターが、マッピング対象のヘッダーをメッセージ内で見つけることができること。 メッセージまたはメッセージ バッファーの追加のヘッダーは、一致を `true` の状態にしておくためであれば、許容されます。  
  
### <a name="prefix-endpoint-address-filters"></a>プレフィックス エンドポイント アドレス フィルター  
  
1. <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> は、メッセージ URI のプレフィックスとも一致できるという点を除けば、<xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> フィルターと同じように動作します。 たとえば、アドレス `http://www.adatum.com` を指定するフィルターは、`http://www.adatum.com/userA`宛てのメッセージと一致します。  
  
### <a name="xpath-message-filters"></a>XPath メッセージ フィルター  
 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> は、XPath 式を使用して、XML ドキュメントに特定の要素、属性、テキスト、その他の XML 構文が含まれているかどうかを判定します。 このフィルターは、XPath の厳密なサブセットに対して非常に効率的に処理できるように最適化されています。 XML パス言語については、 [W3C 勧告『 Xml Path language 1.0](https://www.w3.org/TR/xpath/all/)』を参照してください。  
  
 アプリケーションは通常、エンドポイントで <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> を使用して SOAP メッセージの内容を問い合わせ、その結果に基づいて適切なアクションを実行します。 たとえば、キューの処理では、XPath クエリを使用して既知のヘッダーの優先度要素を検査し、メッセージをキューの先頭に移動するべきかどうかを決定します。  
  
## <a name="filter-tables"></a>フィルター テーブル  
 フィルター テーブルは、キーと値のペアを保存するために使用されます。ここで、フィルターがキーであり、これに関連するデータが値になります。 このフィルター データは、メッセージがフィルターに一致した場合に起きるアクションを示すために使用することができ、その型はフィルター テーブル クラスのジェネリック パラメーターになります。 フィルター データは、ルーティングのルール、セッション セキュリティの状態、チャネル上のリスナーなどで構成できます。 データは、データ フロー制御が必要な場所で使用できます。  
  
 フィルター テーブルは、<xref:System.ServiceModel.Dispatcher.IMessageFilterTable%601> ジェネリック インターフェイスを実装します。  
  
 フィルター テーブルには、テーブル内のすべてのフィルターに対してメッセージとの一致を調べて、一致したフィルターまたはデータの順序付けられていないコレクションを返すメソッドがいくつかあります。 メソッドの中には複数回の一致を調べて、一致したすべての項目を返すものもあります。 それ以外のメソッドは 1 回だけの一致となり、1 つの項目のみを返し、フィルターが複数回一致する場合には、<xref:System.ServiceModel.Dispatcher.MultipleFilterMatchesException> をスローします。  
  
### <a name="message-filter-table"></a>メッセージ フィルター テーブル  
 <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601> は、<xref:System.ServiceModel.Dispatcher.IMessageFilterTable%601> の最も一般的な実装です。 このテーブルには、すべての種類のフィルターを格納できます。  
  
 フィルターには数字による優先順位を割り当てることができます。一番大きな数が最も高い優先順位となります。 複数の種類のフィルターが、同じ優先度を持つことができます。 また特定の種類のフィルターが、複数の優先順位レベルに現れることもできます。  
  
 照合処理は優先順位が最も高いフィルターから開始され、ある優先順位で一致するフィルターが見つかると、それよりも優先順位の低いフィルターは検査されません。 そのため、フィルターが 1 回だけ一致するメソッドを使用しているときに、あるメッセージに対してフィルターの一致が複数回発生した場合、一致フィルターの優先順位がそれぞれ異なっていれば、例外はスローされず、最も優先順位の高いフィルターが返されます。 同様に、複数回フィルターが一致するメソッドでも、優先順位が最も高い一致フィルターのみが返されます。  
  
### <a name="xpath-message-filter-table"></a>XPath メッセージ フィルター テーブル  
 <xref:System.ServiceModel.Dispatcher.XPathMessageFilterTable%601> は宣言的な XPath フィルターに最適化されているため、テーブル キーは <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> になります。  
  
 <xref:System.ServiceModel.Dispatcher.XPathMessageFilterTable%601> クラスは、ほとんどのメッセージ シナリオをカバーし、XPath 1.0 の文法を完全にサポートする XPath のサブセットに合わせてマッチングを最適化します。 また、効率的な並列マッチング用のアルゴリズムも最適化します。  
  
 このテーブルには、`Match` および <xref:System.Xml.XPath.XPathNavigator> 上で動作する特殊な <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> メソッドがいくつかあります。 <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> は、<xref:System.Xml.XPath.XPathNavigator> プロパティを追加することで、<xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator.CurrentPosition%2A> クラスを拡張します。 このプロパティを使用すると、ナビゲーターを複製せずに XML ドキュメント内の位置を迅速に保存し、読み込むことができます。<xref:System.Xml.XPath.XPathNavigator> を使用してそうした操作を行うには、大量のメモリ領域を割り当てる必要があります。 WCF XPath エンジンでは、XML ドキュメントに対してクエリを実行する過程でカーソルの位置を頻繁に記録する必要があるため、<xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> はメッセージ処理のための重要な最適化を提供します。  
  
## <a name="customer-scenarios"></a>顧客シナリオ  
 フィルター処理は、メッセージに含まれるデータに応じて、異なる処理モジュールにメッセージを送信する必要がある場合に、いつでも使用できます。 一般的なのは、アクション コードに基づいてメッセージをルーティングするシナリオと、メッセージのエンドポイント アドレスに基づいてメッセージのストリームを分離化するシナリオの 2 つです。  
  
### <a name="routing"></a>ルーティング  
 エンドポイントのリスナーは、メッセージの SOAP ヘッダーに 1 つ以上のアクション コードが含まれるメッセージをリッスンします。 これは、アクション コードを含む配列をコンストラクターに渡して <xref:System.ServiceModel.Dispatcher.ActionMessageFilter> を作成することで実装します。 このフィルターは、`ListenerFactory` に登録するために使用されるため、アクションがフィルター内のアクション コードのいずれかと一致するメッセージだけが特定のエンドポイントに到達します。  
  
### <a name="de-multiplexing"></a>分離化  
 複数のエンドポイントがネットワーク上の同じ `ServiceListener` から分散している場合、メッセージを分離化し、メッセージが特定のエンドポイント アドレスに属しているかどうかを確認する唯一の方法は、<xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> を使用することです。このフィルターは、ヘッダーに格納されている情報に対して検索を実行して、登録されているエンドポイントに向けられたメッセージを選択します。 このようなフィルターを通過するのは、次の両方に対応する必要なヘッダーをすべて持っているメッセージだけです。  
  
- `EndpointAddress` にある URI  
  
- `EndpointAddress` で指定された <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> にある残りのエンドポイント パラメーター  
  
## <a name="see-also"></a>関連項目

- [データ転送とシリアル化](../../../../docs/framework/wcf/feature-details/data-transfer-and-serialization.md)
