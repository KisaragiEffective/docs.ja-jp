---
title: Entity Framework の用語
ms.date: 03/30/2017
ms.assetid: fa2a1bd1-6118-487b-8673-eebc66b92945
ms.openlocfilehash: e646b1d898ed1f0ed7b3dbb8f77c451c2cfcd1e0
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452423"
---
# <a name="entity-framework-terminology"></a>Entity Framework の用語
このトピックでは Entity Framework ドキュメントで頻繁に参照される用語を定義します。 追加情報を確認できる関連トピックへのリンクも示しています。  
  
|期間|Definition|  
|----------|----------------|  
|関連付け|エンティティ型間のリレーションシップの定義。<br /><br /> 詳細については、「 [Association 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#association-element-csdl) 」および「[関連付けの種類](../association-type.md)」を参照してください。|  
|関連付けセット|同じ型のアソシエーションのインスタンスの論理コンテナー。<br /><br /> 詳細については、「 [AssociationSet Element (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#associationset-element-csdl) 」および「 [association set](../association-set.md)」を参照してください。|  
|Code First|Entity Framework 4.1 以降では、Code First の開発を使用してモデルをプログラムで作成することもできます。 Code First の開発に対しては、2 つの異なるシナリオがあります。 どちらの場合でも、開発者は .NET Framework のクラス定義をコーディングしてモデルを定義し、データ注釈または Fluent API を使用してオプションで追加のマッピングまたは構成を指定します。<br /><br /> Code First の開発は Entity Framework 5.0 の一部です。 Entity Framework 5.0 は .NET Framework の一部ではありませんが .NET Framework 4.5 上に構築されています。 Entity Framework 5.0 は[Entity Framework](https://www.nuget.org/packages/EntityFramework) NuGet パッケージとして利用できます。 詳細については、「 [Entity Framework の過去のリリース](/ef/ef6/what-is-new/past-releases)」を参照してください。|  
|コマンド ツリー|1つ以上の式で構成されるすべての Entity Framework クエリの、プログラムによる一般的な表現。<br /><br /> 詳細については、「 [Entity Framework の概要](overview.md)」を参照してください。|  
|複合型|概念モデルに定義されている複合プロパティを表す .NET Framework クラス。 複合型により、スカラー プロパティをエンティティ内で整理することができます。 複合オブジェクトは、複合型のインスタンスです。 詳細については、「 [ComplexType 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#complextype-element-csdl) 」および「[複合型](../complex-type.md)」を参照してください。|  
|ComplexType 要素|キー プロパティを持たないエンティティ型の非スカラー プロパティを表すデータ型の仕様。<br /><br /> 詳細については、「 [ComplexType 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#complextype-element-csdl) 」および「[複合型](../complex-type.md)」を参照してください。|  
|概念モデル|Entity Framework 内のアプリケーションのドメインにおけるエンティティ型、複合型、アソシエーション、エンティティコンテナー、エンティティセット、およびアソシエーションセットの抽象的な仕様。 概念モデルは、CSDL で .csdl ファイルに定義されます。<br /><br /> 詳細については、「[モデリングとマッピング](modeling-and-mapping.md)」を参照してください。|  
|.csdl ファイル|CSDL で表現された概念モデルを含む XML ファイル。|  
|概念スキーマ定義言語 (CSDL)|概念モデルのエンティティ型、関連付け、エンティティ コンテナー、エンティティ セット、および関連付けセットを定義するための XML ベースの言語。<br /><br /> 詳細については、「 [CSDL Specification](./language-reference/csdl-specification.md)」を参照してください。|  
|コンテナー|エンティティ セットとアソシエーション セットの論理的なグループ。<br /><br /> 詳細については、「 [EntityContainer 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#entitycontainer-element-csdl) 」および「[エンティティコンテナー](../entity-container.md)」を参照してください。|  
|concurrency|複数のユーザーが共有データに同時にアクセスや変更を行うことができるようにする処理。 既定では、Entity Framework はオプティミスティック同時実行制御モデルを実装します。|  
|direction|一部のアソシエーションの非対称の性質を表します。 方向は、スキーマの `FromRole` 要素または `ToRole` 要素の `NavigationProperty` 属性と `ReferentialConstraint` 属性で指定されます。<br /><br /> 詳細については、「 [NavigationProperty 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#navigationproperty-element-csdl) 」および「[ナビゲーションプロパティ](../navigation-property.md)」を参照してください。|  
|一括読み込み (eager loading)|関連オブジェクトの特定のセットを、クエリで明示的に要求されたオブジェクトと共に読み込むプロセス。|  
|.edmx ファイル|概念モデル (CSDL)、ストレージ モデル (SSDL)、および概念モデルとストレージ モデルの間のマッピング (MSL) を含む XML ファイル。 .Edmx ファイルは、Entity Data Model ツールによって作成されます。 詳細については、「 [.Edmx ファイルの概要](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))」を参照してください。|  
|end|アソシエーションに参加しているエンティティ。<br /><br /> 詳細については、「 [End 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#end-element-csdl) 」および「[アソシエーション end](../association-end.md)」を参照してください。|  
|エンティティ|データ型を定義する際に基づくアプリケーションのドメインにおける概念。<br /><br /> 詳細については、「 [EntityType 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#entitytype-element-csdl) 」および「[エンティティ型](../entity-type.md)」を参照してください。|  
|EntityClient|`EntityConnection`、`EntityCommand`、`EntityDataReader`などのクラスを含むストレージに依存しない ADO.NET データプロバイダー。 [!INCLUDE[esql](../../../../../includes/esql-md.md)] と連携し、`SqlClient`などのストレージ固有の ADO.NET データプロバイダーに接続します。<br /><br /> 詳細については、「 [Entity Framework 用の EntityClient プロバイダー](entityclient-provider-for-the-entity-framework.md)」を参照してください。|  
|エンティティ コンテナー|指定された名前空間に実装されるエンティティ セットとアソシエーション セットを指定します。<br /><br /> 詳細については、「 [EntityContainer 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#entitycontainer-element-csdl) 」および「[エンティティコンテナー](../entity-container.md)」を参照してください。|  
|エンティティ データ モデル (EDM: Entity Data Model)|格納される形式に関係なく、エンティティおよびリレーションシップとしてデータ構造を記述する一連の概念。<br /><br /> 詳細については、「 [Entity Data Model](../entity-data-model.md)」を参照してください。|  
|Entity Framework|開発者がデータ ソースの論理スキーマにマップされた概念モデルを使用できるようにすることで、データ指向のソフトウェア アプリケーションの開発をサポートするテクノロジ セット。<br /><br /> 詳細については、「 [Entity Framework の概要](overview.md)」を参照してください。|  
|エンティティ セット|指定された型とそのサブタイプのエンティティの論理コンテナー。 エンティティ セットは、データベース内のテーブルにマップされます。<br /><br /> 詳細については、「 [EntitySet 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#entityset-element-csdl) 」および「[エンティティセット](../entity-set.md)」を参照してください。|  
|Entity SQL|エンティティの概念スキーマを直接操作し、継承やリレーションシップなどの概念モデルの概念をサポートする、ストレージの影響を受けない SQL の言語。<br /><br /> 詳細については、「 [Entity SQL 言語](./language-reference/entity-sql-language.md)」を参照してください。|  
|エンティティ型|概念モデルで定義されているエンティティを表す .NET Framework クラス。 エンティティ型には、スカラー プロパティ、複合プロパティ、およびナビゲーション プロパティが含まれる場合があります。 オブジェクトは、エンティティ型のインスタンスです。 詳細については、「[オブジェクトの操作](working-with-objects.md)」を参照してください。|  
|EntityType|キーやプロパティの名前付きセットを含み、概念モデルまたはストレージ モデルの最上位項目を表すデータ型の仕様。<br /><br /> 詳細については、「 [EntityType 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#entitytype-element-csdl) 」および「[エンティティ型](../entity-type.md)」を参照してください。|  
|明示的な読み込み (explicit loading)|クエリがオブジェクトを返す場合、関連オブジェクトは同時に読み込まれません。 既定では、関連オブジェクトは、ナビゲーション プロパティの `Load` メソッドを使用して明示的に要求されるまで読み込まれません。|  
|外部キーの関連付け (foreign key association)|外部キーのプロパティを介して管理される、エンティティ間の関連付け。|  
|依存リレーションシップ (identifying relationship)|プリンシパル エンティティの主キーが、依存エンティティの主キーの一部であるリレーションシップ。 この種のリレーションシップでは、依存エンティティはプリンシパル エンティティなしに存在できません。|  
|独立した関連付け (independent association)|独立のオブジェクトによって表され追跡される、エンティティ間の関連付け。|  
|key|エンティティ型の一意のインスタンスを識別するために使用されるプロパティまたはプロパティ セットを指定するエンティティ型の属性。 オブジェクト レイヤーでは、<xref:System.Data.EntityKey> クラスで表現されます。<br /><br /> 詳細については、「 [Key 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#key-element-csdl) 」および「[エンティティキー](../entity-key.md)」を参照してください。|  
|遅延読み込み (lazy loading)|クエリがオブジェクトを返す場合、関連オブジェクトは同時に読み込まれません。 代わりに、ナビゲーション プロパティへのアクセス時に自動的に読み込まれます。|  
|LINQ to Entities|走査、フィルター、および射影操作を視覚的C#および Visual Basic で直接、宣言的な方法で表現できるようにする一連のクエリ演算子を定義するクエリ構文。<br /><br /> 詳細については、「 [LINQ to Entities](./language-reference/linq-to-entities.md)」を参照してください。|  
|mapping|概念モデルの項目とストレージ モデルの項目の対応付けの指定。<br /><br /> 詳細については、「 [MSL 仕様](./language-reference/msl-specification.md)」を参照してください。|  
|.msl ファイル|MSL で表現された概念モデルとストレージ モデルの間のマッピングを含む XML ファイル。|  
|マッピング仕様言語 (MSL)|概念モデルで定義された項目をストレージ モデルの項目に対応付ける XML ベースの言語。<br /><br /> 詳細については、「 [MSL 仕様](./language-reference/msl-specification.md)」を参照してください。|  
|変更関数|データ ソースでデータを挿入、更新、および削除するために使用されるストアド プロシージャ。 これらの関数は Entity Framework 生成されたコマンドの代わりに使用されます。 変更関数は、ストレージ モデルの `Function` 要素で定義されます。 [ModificationFunctionMapping](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc716778(v=vs.100))要素は、概念モデルで定義されているエンティティに対する挿入、更新、および削除の各操作に、これらの変更関数をマップします。|  
|多重度|アソシエーションによって定義されているリレーションシップの両側に存在できるエンティティの数。 カーディナリティとも呼ばれます。<br /><br /> 詳細については、「 [End 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#end-element-csdl) 」および「[アソシエーション end](../association-end.md)」を参照してください。|  
|Multiple-Entity-Sets-per-Type|1 つのエンティティ型を複数のエンティティ セットで定義できる機能。<br /><br /> 詳細については、「 [EntitySet 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#entityset-element-csdl) 」および「[方法: 型ごとに複数のエンティティセットを持つモデルを定義する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738537(v=vs.100))」を参照してください。|  
|ナビゲーション プロパティ|アソシエーションによって定義されている別のエンティティ型とのリレーションシップを表すエンティティ型のプロパティ。 ナビゲーション プロパティは、アソシエーションのもう一方の End での複数要素の接続性に応じて、関連オブジェクトを <xref:System.Data.Objects.DataClasses.EntityCollection%601> または <xref:System.Data.Objects.DataClasses.EntityReference%601> として返すために使用されます。<br /><br /> 詳細については、「 [NavigationProperty 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#navigationproperty-element-csdl) 」および「[ナビゲーションプロパティ](../navigation-property.md)」を参照してください。|  
|クエリ パス|オブジェクト クエリの実行時に返す関連オブジェクトを指定するパスの文字列表記。 クエリ パスは、<xref:System.Data.Objects.ObjectQuery%601.Include%2A> に対して <xref:System.Data.Objects.ObjectQuery%601> メソッドを呼び出すことによって定義されます。<br /><br /> 詳細については、「[関連オブジェクトの読み込み](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896272(v=vs.100))」を参照してください。|  
|オブジェクト コンテキスト|概念モデルで定義したエンティティ コンテナーを表します。 基になるデータ ソースへの接続を含み、変更の追跡や ID 解決などのサービスを提供します。 オブジェクト コンテキストは、<xref:System.Data.Objects.ObjectContext> クラスまたは `DbContext` クラスのインスタンスで表されます。<br /><br /> `DbContext` は Entity Framework 5.0 の一部です。 Entity Framework 5.0 は .NET Framework の一部ではありませんが .NET Framework 4.5 上に構築されています。 Entity Framework 5.0 は[Entity Framework](https://www.nuget.org/packages/EntityFramework) NuGet パッケージとして利用できます。 詳細については、「 [Entity Framework の過去のリリース](/ef/ef6/what-is-new/past-releases)」を参照してください。|  
|オブジェクト レイヤー|Entity Framework によって使用されるエンティティ型およびオブジェクト コンテキストの定義。|  
|オブジェクト クエリ|データをオブジェクトとして返す概念モデルに対してオブジェクト コンテキスト内で実行されるクエリ。<br /><br /> 詳細については、「[オブジェクトクエリ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896241(v=vs.100))」を参照してください。|  
|オブジェクト リレーショナル マッピング|リレーショナル データベースのデータをオブジェクト指向のソフトウェア アプリケーションで使用できるデータ型に変換する手法。<br /><br /> Entity Framework は、ストレージモデルで定義されているリレーショナルデータを、概念モデルで定義されているデータ型にマップすることによって、オブジェクトリレーショナルマッピングサービスを提供します。<br /><br /> 詳細については、「[モデリングとマッピング](modeling-and-mapping.md)」を参照してください。|  
|オブジェクト サービス|アプリケーションコードが .NET Framework オブジェクトなどのエンティティを操作できるようにする、Entity Framework によって提供されるサービス。|  
|永続化非依存オブジェクト|データ ストレージに関連するロジックが含まれていないオブジェクト。 POCO エンティティとも呼ばれます。|  
|POCO|Plain Old CLR Object の略。 別のクラスから継承しないオブジェクト、またはインターフェイスを実装しないオブジェクト。|  
|POCO エンティティ|<xref:System.Data.Objects.DataClasses.EntityObject> または <xref:System.Data.Objects.DataClasses.ComplexObject> から継承せず、Entity Framework インターフェイスを実装していない Entity Framework 内のエンティティ。 多くの場合、POCO エンティティは Entity Framework アプリケーションで使用する既存のドメインオブジェクトです。 このようなエンティティは、永続化非依存性をサポートしています。 詳細については、「 [POCO エンティティの操作](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd456853(v=vs.100))」を参照してください。|  
|プロキシ オブジェクト|POCO クラスから派生し、変更の追跡と遅延読み込みをサポートするために Entity Framework によって生成されるオブジェクト。 詳細については、「 [POCO プロキシを作成するための要件](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd468057(v=vs.100))」を参照してください。|  
|参照制約|エンティティが他のエンティティと依存関係にあることを示す、概念モデルで定義された制約。 この制約は、依存エンティティのインスタンスが対応する主要エンティティのインスタンスなしでは存在できないことを意味します。<br /><br /> 詳細については[、「参照](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#referentialconstraint-element-csdl)[整合性制約](../referential-integrity-constraint.md)」を参照してください。|  
|リレーションシップ (relationship)|エンティティ間の論理的な関係。|  
|ロール (role)|リレーションシップのセマンティクスを明確にするためにアソシエーションの両方の `End` に付けられた名前。<br /><br /> 詳細については、「 [End 要素 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#end-element-csdl) 」および「[アソシエーション end](../association-end.md)」を参照してください。|  
|スカラー プロパティ|ストレージ モデルの単一のフィールドにマップされるエンティティのプロパティ。|  
|進捗管理エンティティ (self-tracking entity)|スカラー プロパティ、複合プロパティ、およびナビゲーション プロパティの変更を報告できる、テキスト テンプレート変換ツールキット (T4) から構築されるエンティティ。|  
|単純型|概念モデルでプロパティを定義するために使用されるプリミティブ型。<br /><br /> 詳細については、「[概念モデルの型 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#conceptual-model-types-csdl) 」および「 [Entity Data Model: プリミティブデータ型](../entity-data-model-primitive-data-types.md)」を参照してください。|  
|分割されたエンティティ|ストレージ モデルの 2 つの別個の型にマップされる 1 つのエンティティ型。<br /><br /> 詳細については、「[方法: 2 つのテーブルにマップされた単一のエンティティを持つモデルを定義](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896233(v=vs.100))する」を参照してください。|  
|ストレージ モデル|リレーショナル データベースなど、サポートされているデータ ソースのデータの論理モデルの定義。 ストレージ モデルは、SSDL で .ssdl ファイルに定義されます。<br /><br /> 詳細については、「[モデリングとマッピング](modeling-and-mapping.md)」と「 [SSDL Specification](/ef/ef6/modeling/designer/advanced/edmx/ssdl-spec)」を参照してください。|  
|.ssdl ファイル|SSDL で表現されたストレージ モデルを含む XML ファイル。|  
|ストア スキーマ定義言語 (SSDL)|一般的にデータベース スキーマに対応するストレージ モデルのエンティティ型、アソシエーション、エンティティ コンテナー、エンティティ セット、およびアソシエーション セットの定義に使用される XML ベースの言語。<br /><br /> 詳細については、「 [SSDL Specification](/ef/ef6/modeling/designer/advanced/edmx/ssdl-spec)」を参照してください。|  
|table-per-hierarchy|あらゆる型の属性を 1 つのテーブル内の階層構造に含める、データベースにおける型階層のモデリング手法。|  
|table-per-type|一対一リレーションシップを持った複数のテーブルを使用して各種の型をモデリングする、データベースにおける型階層のモデリング手法。|  
  
## <a name="see-also"></a>参照

- [ADO.NET Entity Framework](index.md)
- [Entity Framework の概要](overview.md)
- [作業の開始](getting-started.md)
- [Entity Framework のリソース](resources.md)
