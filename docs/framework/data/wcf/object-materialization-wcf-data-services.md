---
title: オブジェクトの具体化 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, querying
ms.assetid: f0dbf7b0-0292-4e31-9ae4-b98288336dc1
ms.openlocfilehash: 7a2b201e5690c4304e663e9429c54f377e05f556
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568904"
---
# <a name="object-materialization-wcf-data-services"></a>オブジェクトの具体化 (WCF Data Services)

.NET Framework ベースのクライアントアプリケーションで **[サービス参照の追加]** ダイアログボックスを使用して Open Data Protocol (OData) フィードを使用すると、フィードによって公開されるデータモデル内の各エンティティ型に対して同等のデータクラスが生成されます。 詳細については、「[データサービスクライアントライブラリの生成](generating-the-data-service-client-library-wcf-data-services.md)」を参照してください。 クエリによって返されるエンティティ データは、これらの生成されたクライアント データ サービス クラスのいずれかのインスタンスに具体化されます。 追跡オブジェクトのマージオプションと id 解決の詳細については、「[データサービスコンテキストの管理](managing-the-data-service-context-wcf-data-services.md)」を参照してください。

WCF Data Services では、ツールで生成されたデータクラスを使用するのではなく、独自のクライアントデータサービスクラスを定義することもできます。 これにより、"plain-old CLR object" (POCO) データ クラスとして知られる独自のデータ クラスを使用できます。 これらの種類のカスタムデータクラスを使用する場合は、<xref:System.Data.Services.Common.DataServiceKeyAttribute> または <xref:System.Data.Services.Common.DataServiceEntityAttribute> のいずれかを使用してデータクラスの属性を設定し、クライアントの型名がデータサービスのデータモデルの型名と一致することを確認する必要があります。

ライブラリは、クエリ応答メッセージを受信すると、OData フィードから返されたデータを、クエリの種類のクライアントデータサービスクラスのインスタンスに具体化します。 これらのオブジェクトを具体化する一般的なプロセスは次のとおりです。

1. クライアント ライブラリは、応答メッセージ フィード内の `entry` 要素からシリアル化された型を読み取り、次のいずれかの方法で正しい型の新しいインスタンスの作成を試みます。

    - フィードで宣言された型の名前が <xref:System.Data.Services.Client.DataServiceQuery%601> の型と同じ場合は、空のコンストラクターを使用してこの型の新しいインスタンスが作成されます。

    - フィードで宣言された型の名前が <xref:System.Data.Services.Client.DataServiceQuery%601> の型から派生した型と同じ場合は、空のコンストラクターを使用して派生したこの型の新しいインスタンスが作成されます。

    - フィードで宣言された型が <xref:System.Data.Services.Client.DataServiceQuery%601> の型、または派生した型と一致しない場合は、空のコンストラクターを使用してクエリされた型の新しいインスタンスが作成されます。

    - <xref:System.Data.Services.Client.DataServiceContext.ResolveType%2A> プロパティが設定されている場合、指定されたデリゲートが呼び出され、既定の名前ベースの型のマッピングをオーバーライドし、<xref:System.Func%602> によって返された型の新しいインスタンスが代わりに作成されます。 このデリゲートが NULL 値を返した場合、クエリされた型の新しいインスタンスが代わりに作成されます。 継承のシナリオをサポートするために、既定の名前ベースの型名のマッピングをオーバーライドすることが必要な場合があります。

2. クライアント ライブラリは `id` の `entry` 要素から URI 値を読み取ります。これがエンティティの ID 値です。 <xref:System.Data.Services.Client.DataServiceContext.MergeOption%2A> の  <xref:System.Data.Services.Client.MergeOption.NoTracking> 値が使用されない限り、<xref:System.Data.Services.Client.DataServiceContext> のオブジェクトの追跡には、ID 値が使用されます。 さらに、クエリ応答でエンティティが複数回返されたときでも、単一のエンティティ インスタンスのみが作成されることを保証するためにも ID 値を使用します。

3. クライアント ライブラリは、フィード エントリからプロパティを読み取り、新しく作成されたオブジェクトに対応するプロパティを設定します。 <xref:System.Data.Services.Client.DataServiceContext> に同じ ID 値のオブジェクトが既に存在する場合、プロパティは、<xref:System.Data.Services.Client.MergeOption> の <xref:System.Data.Services.Client.DataServiceContext> 設定に基づき設定されます。 応答には、クライアント型で対応するプロパティがないプロパティ値が含まれる場合があります。 そのような場合、アクションは、<xref:System.Data.Services.Client.DataServiceContext.IgnoreMissingProperties%2A> の <xref:System.Data.Services.Client.DataServiceContext> プロパティの値によって異なります。 このプロパティが `true` に設定されている場合は、欠落しているプロパティは無視されます。 それ以外の場合は、エラーが発生します。 プロパティは次のように設定されます。

    - スカラー プロパティは、応答メッセージのエントリ内の対応する値に設定されます。

    - 複合プロパティは、新しい複合型インスタンスに設定されます。このインスタンスは、応答からの複合型のプロパティとともに設定されます。

    - 関連するエンティティのコレクションを返すナビゲーション プロパティは、<xref:System.Collections.Generic.ICollection%601> の新しいまたは既存のインスタンスに設定されます。ここで、`T` は関連エンティティの型です。 このコレクションは、関連オブジェクトが <xref:System.Data.Services.Client.DataServiceContext> に読み込まれていない限り空になります。 詳細については、「[遅延コンテンツの読み込み](loading-deferred-content-wcf-data-services.md)」を参照してください。

      > [!NOTE]
      > 生成されたクライアント データ クラスでデータ バインディングがサポートされる場合、ナビゲーション プロパティは代わりに <xref:System.Data.Services.Client.DataServiceCollection%601> クラスのインスタンスを返します。 詳細については、「[コントロールへのデータのバインド](binding-data-to-controls-wcf-data-services.md)」を参照してください。

4. <xref:System.Data.Services.Client.DataServiceContext.ReadingEntity> イベントが発生します。

5. クライアント ライブラリは <xref:System.Data.Services.Client.DataServiceContext> にオブジェクトをアタッチします。 <xref:System.Data.Services.Client.MergeOption> が <xref:System.Data.Services.Client.MergeOption.NoTracking> の場合は、オブジェクトはアタッチされません。

## <a name="see-also"></a>参照

- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)
- [クエリ射影](query-projections-wcf-data-services.md)
