---
title: 移行に関する注意事項 (Entity Framework)
ms.date: 03/30/2017
ms.assetid: c85b6fe8-cc32-4642-8f0a-dc0e5a695936
ms.openlocfilehash: 168aec6ef369f446cfac22ee5c4361fa06aaf16d
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569439"
---
# <a name="migration-considerations-entity-framework"></a>移行に関する注意事項 (Entity Framework)
ADO.NET Entity Framework には、既存のアプリケーションにいくつかの利点があります。 特に重要な利点の 1 つが、概念モデルを使用して、アプリケーションで使用するデータ構造をデータ ソースのスキーマから分離できることです。 これにより、ストレージ モデルやデータ ソース自体の将来の変更が容易になり、その変更を補うための変更をアプリケーションに加える必要がなくなります。 Entity Framework を使用する利点の詳細については、「 [Entity Framework の概要](overview.md)と[Entity Data Model](../entity-data-model.md)」を参照してください。  
  
 Entity Framework の利点を活用するために、既存のアプリケーションを Entity Framework に移行することができます。 移行作業の一部はすべてのアプリケーションに共通です。 これらの一般的なタスクには、バージョン 3.5 Service Pack 1 (SP1) 以降の .NET Framework を使用したアプリケーションのアップグレード、モデルとマッピングの定義、Entity Framework の構成などがあります。 アプリケーションを Entity Framework に移行する場合、追加の考慮事項が適用されます。 移行するアプリケーションの種類やアプリケーションの特定の機能に依存する注意点もあります。 このトピックでは、既存のアプリケーションをアップグレードする際に最適な方法を選択するために役立つ情報を紹介します。  
  
## <a name="general-migration-considerations"></a>全般的な移行の注意点  
 アプリケーションを Entity Framework に移行する場合は、次の点に注意してください。  
  
- バージョン 3.5 SP1 以降の .NET Framework を使用するアプリケーションは、アプリケーションで使用されるデータソースのデータプロバイダーが Entity Framework をサポートしている限り、Entity Framework に移行できます。  
  
- データ ソース プロバイダーが Entity Framework をサポートしていても、そのプロバイダーのすべての機能が Entity Framework でサポートされるとは限りません。  
  
- 大規模なアプリケーションや複雑なアプリケーションの場合、アプリケーション全体を一度に Entity Framework に移行する必要はありません。 ただし、Entity Framework を使用しない部分がアプリケーションにある場合、それらの部分については、データ ソースが変更された場合に変更が必要になります。  
  
- Entity Framework によって使用されるデータプロバイダー接続は、アプリケーションの他の部分と共有できます。これは、Entity Framework が ADO.NET データプロバイダーを使用してデータソースにアクセスするためです。 (たとえば、Entity Framework は SqlClient プロバイダーを使用して SQL Server データベースにアクセスします)。 詳細については、「 [Entity Framework 用の EntityClient プロバイダー](entityclient-provider-for-the-entity-framework.md)」を参照してください。  
  
## <a name="common-migration-tasks"></a>共通の移行タスク  
 既存のアプリケーションを Entity Framework に移行するためのパスは、アプリケーションの種類と既存のデータアクセス戦略の両方に依存します。 ただし、既存のアプリケーションを Entity Framework に移行する場合は、常に次のタスクを実行する必要があります。  
  
> [!NOTE]
> これらのタスクはすべて、Visual Studio 2008 以降の Entity Data Model ツールを使用すると自動的に実行されます。 詳細については、「[方法: Entity Data Model ウィザードを使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))する」を参照してください。  
  
1. アプリケーションをアップグレードします。  
  
     以前のバージョンの Visual Studio と .NET Framework を使用して作成されたプロジェクトは、Visual Studio 2008 SP1 を使用するようにアップグレードし、バージョン 3.5 SP1 以降の .NET Framework にする必要があります。  
  
2. モデルおよびマッピングを定義します。  
  
     モデル ファイルとマッピング ファイルでは、概念モデルのエンティティ、データ ソースの構造 (テーブル、ストアド プロシージャ、ビューなど)、およびエンティティとデータ ソース構造との間のマッピングを定義します。 詳細については、「[方法: モデルファイルとマッピングファイルを手動で定義](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399785(v=vs.100))する」を参照してください。  
  
     ストレージ モデルに定義されている型は、データ ソースのオブジェクトと名前が一致している必要があります。 また、既存のアプリケーションでデータがオブジェクトとして公開されている場合は、概念モデルに定義するエンティティおよびプロパティの名前を既存のデータ クラスおよびプロパティの名前に一致させる必要があります。 詳細については、「[方法: カスタムオブジェクトと連携するようにモデリングファイルとマッピングファイルをカスタマイズ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738625(v=vs.100))する」を参照してください。  
  
    > [!NOTE]
    > Entity Data Model デザイナーを使用すると、概念モデルのエンティティの名前を既存のオブジェクトに合わせて変更できます。 詳細については、「 [Entity Data Model デザイナー](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc716685(v=vs.100))」を参照してください。  
  
3. 接続文字列を定義します。  
  
     Entity Framework は、概念モデルに対してクエリを実行するときに、特殊な形式の接続文字列を使用します。 この接続文字列は、モデル ファイルとマッピング ファイルに関する情報とデータ ソースへの接続に関する情報をカプセル化したものです。 詳細については、「[方法: 接続文字列を定義](how-to-define-the-connection-string.md)する」を参照してください。  
  
4. Visual Studio プロジェクトを構成します。  
  
     Entity Framework アセンブリおよびモデルファイルとマッピングファイルへの参照は、Visual Studio プロジェクトに追加する必要があります。 これらのマッピング ファイルをプロジェクトに追加することで、接続文字列で指定された場所にアプリケーションと共に配置されるようにすることができます。 詳細については、「[方法: Entity Framework プロジェクトを手動で構成する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738546(v=vs.100))」を参照してください。  
  
## <a name="considerations-for-applications-with-existing-objects"></a>既存のオブジェクトを含むアプリケーションの注意点  
 .NET Framework 4 以降では、Entity Framework は "plain old" CLR オブジェクト (POCO) をサポートしています。これは、永続化非依存オブジェクトとも呼ばれます。 ほとんどの場合、既存のオブジェクトは軽微な変更を加えることによって Entity Framework を操作できます。 詳細については、「 [POCO エンティティの操作](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd456853(v=vs.100))」を参照してください。 また、アプリケーションを Entity Framework に移行し、Entity Framework ツールによって生成されるデータクラスを使用することもできます。 詳細については、「[方法: Entity Data Model ウィザードを使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738677(v=vs.100))する」を参照してください。  
  
## <a name="considerations-for-applications-that-use-adonet-providers"></a>ADO.NET プロバイダーを使用するアプリケーションの注意点  
 SqlClient などの ADO.NET プロバイダーを使用すると、データソースに対してクエリを実行し、表形式のデータを返すことができます。 データを ADO.NET データセットに読み込むこともできます。 次の一覧では、既存の ADO.NET プロバイダーを使用するアプリケーションのアップグレードに関する考慮事項について説明します。  
  
- データ リーダーを使用して表形式のデータを表示している場合  

  EntityClient プロバイダーを使用して [!INCLUDE[esql](../../../../../includes/esql-md.md)] クエリを実行し、返された <xref:System.Data.EntityClient.EntityDataReader> オブジェクトを列挙することを検討してください。 これは、アプリケーションがデータリーダーを使用して表形式のデータを表示し、データをオブジェクトに具体化したり、変更を追跡したり、更新を行ったりするために Entity Framework によって提供される機能を必要としない場合にのみ実行します。 データ ソースを更新する既存のデータ アクセス コードも引き続き使用できますが、<xref:System.Data.EntityClient.EntityConnection.StoreConnection%2A> の <xref:System.Data.EntityClient.EntityConnection> プロパティから既存の接続にアクセスできます。 詳細については、「 [Entity Framework 用の EntityClient プロバイダー](entityclient-provider-for-the-entity-framework.md)」を参照してください。  
  
- DataSet を使用している場合  

  Entity Framework には、メモリ内の永続性、変更の追跡、データバインディング、オブジェクトの XML データとしてのシリアル化など、データセットによって提供されるのと同じ機能が多数用意されています。 詳細については、「[オブジェクトの操作](working-with-objects.md)」を参照してください。  
  
  Entity Framework がアプリケーションで必要なデータセットの機能を提供していない場合でも、LINQ to DataSet を使用して LINQ クエリの利点を活用できます。 詳細については、「[LINQ to DataSet](../linq-to-dataset.md)」を参照してください。  
  
## <a name="considerations-for-applications-that-bind-data-to-controls"></a>データをコントロールにバインドするアプリケーションの注意点  
 .NET Framework を使用すると、データセットや ASP.NET データソースコントロールなどのデータソースにデータをカプセル化し、それらのデータコントロールにユーザーインターフェイス要素をバインドできます。 以下は、コントロールを Entity Framework データにバインドする場合の注意点です。  
  
- コントロールへのデータ バインディング  

  概念モデルに対してクエリを実行すると、Entity Framework は、エンティティ型のインスタンスであるオブジェクトとしてデータを返します。 これらのオブジェクトは、コントロールに直接バインドすることができ、このバインディングは更新をサポートします。 つまり、<xref:System.Windows.Forms.DataGridView>内の行など、コントロールのデータに対する変更は、<xref:System.Data.Objects.ObjectContext.SaveChanges%2A> メソッドが呼び出されたときにデータベースに自動的に保存されます。  
  
  クエリの結果を列挙して、データ バインドをサポートする <xref:System.Windows.Forms.DataGridView> などのコントロールにデータを表示するアプリケーションは、そのコントロールを <xref:System.Data.Objects.ObjectQuery%601> の結果にバインドするように変更できます。  
  
  詳細については、「[コントロールへのオブジェクトのバインド](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738469(v=vs.100))」を参照してください。  
  
- ASP.NET データソースコントロール。  

  Entity Framework には、ASP.NET Web アプリケーションでのデータバインディングを簡略化するように設計されたデータソースコントロールが含まれています。 詳細については、「 [EntityDataSource Web サーバーコントロールの概要](https://docs.microsoft.com/previous-versions/aspnet/cc488502(v=vs.100))」を参照してください。  
  
## <a name="other-considerations"></a>その他の注意事項  
 以下は、特定の種類のアプリケーションを Entity Framework に移行する場合の注意点です。  
  
- データ サービスを公開するアプリケーション  

  Windows Communication Foundation (WCF) をベースとする Web サービスやアプリケーションは、XML 要求/応答メッセージ形式を使用して基になるデータ ソースのデータを公開します。 Entity Framework は、バイナリ、XML、または WCF データコントラクトのシリアル化を使用して、エンティティオブジェクトのシリアル化をサポートします。 バイナリ シリアル化と WCF シリアル化ではオブジェクト グラフの完全なシリアル化がサポートされています。 詳細については、「 [N 層アプリケーションの構築](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896304(v=vs.100))」を参照してください。  
  
- XML データを使用するアプリケーション  

  オブジェクトのシリアル化を使用すると、Entity Framework データサービスを作成できます。 XML データを使用するアプリケーション (AJAX ベースのインターネット アプリケーションなど) にデータを提供する  データ サービスを作成できます。 このような場合は、WCF Data Services の使用を検討してください。 これらのデータサービスは、Entity Data Model に基づいており、GET、PUT、POST などの標準の表現形式 (REST) HTTP アクションを使用して、エンティティデータへの動的アクセスを提供します。 詳細については、「[WCF Data Services 4.5](../../wcf/index.md)」を参照してください。  
  
  Entity Framework は、ネイティブ XML データ型をサポートしていません。 そのため、XML 列を持つテーブルにエンティティをマップすると、その XML 列に対応するエンティティ プロパティは文字列になります。 このような場合は、オブジェクトを切断して XML としてシリアル化することができます。 詳細については、「[オブジェクトのシリアル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738446(v=vs.100))化」を参照してください。  
  
  アプリケーションで XML データのクエリ機能が必要な場合も、LINQ to XML を使用して LINQ クエリを活用することができます。 詳細については、「 [LINQ to XMLC#()](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-overview.md) 」または「 [LINQ to XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md)」を参照してください。  
  
- 状態を保持するアプリケーション  

  ASP.NET Web アプリケーションは、多くの場合、Web ページまたはユーザーセッションの状態を維持する必要があります。 <xref:System.Data.Objects.ObjectContext> インスタンス内のオブジェクトは、クライアントのビューステートまたはサーバーのセッション状態に格納でき、後で取得して新しいオブジェクトコンテキストに再アタッチできます。 詳細については、「[オブジェクトのアタッチとデタッチ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896271(v=vs.100))」を参照してください。  
  
## <a name="see-also"></a>参照

- [配置に関する注意事項](deployment-considerations.md)
- [Entity Framework の用語](terminology.md)
