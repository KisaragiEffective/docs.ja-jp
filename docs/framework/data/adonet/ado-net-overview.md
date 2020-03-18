---
title: の概要
ms.date: 03/30/2017
ms.assetid: ee3bc1d8-11db-4be4-89eb-c708cf04117d
ms.openlocfilehash: d5dc9cf7081c6876118914a0b95853a5a7ca5e57
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980341"
---
# <a name="adonet-overview"></a>ADO.NET の概要
ADO.NET は、SQL Server や XML などのデータ ソースや、OLE DB や ODBC 経由で公開されるデータ ソースに対する一貫性を持ったアクセス機能を実現します。 データを共有する消費者向けアプリケーションで ADO.NET を使用することで、そのようなデータ ソースへの接続や、データ ソースに格納されているデータの取得、操作、更新を実行できます。  
  
 ADO.NET は、データ操作の中からデータ アクセス機能を分離し、個別にまたは組み合わせて使用できる、独立したコンポーネントへと分解します。 ADO.NET には、データベースとの接続、コマンドの実行、および結果の取得を行うための .NET Framework データ プロバイダーが含まれます。 この結果は直接処理され、ADO.NET <xref:System.Data.DataSet> オブジェクトに格納されます。この結果は、ユーザーに暫定的に公開したり、複数のソースからのデータと組み合わせたり、層間で受け渡したりするために使用されます。 `DataSet` オブジェクトを .NET Framework データ プロバイダーに関係なく使用した場合でも、アプリケーションにとってローカルなデータや XML から提供されたデータを管理できます。  
  
 ADO.NET クラスは System.Data.dll にあり、System.Xml.dll に含まれている XML クラスに統合されています。 データベースに接続してデータを取得し、そのデータをコンソールウィンドウに表示するサンプルコードについては、「 [ADO.NET コード例](ado-net-code-examples.md)」を参照してください。  
  
 マネージド コードを作成する開発者は、ADO.NET により、ネイティブのコンポーネント オブジェクト モデル (COM) 開発者が ActiveX Data Object (ADO) によって利用できる機能と同等の機能を利用できます。 .NET アプリケーションでのデータのアクセスには、ADO ではなく ADO.NET を使用することをお勧めします。  
  
 ADO.NET は、.NET Framework におけるデータ アクセスの最も直接的な方法を提供します。 基になるストレージモデルではなく、概念モデルに対するアプリケーションの動作を可能にする高レベルの抽象化については、 [ADO.NET Entity Framework](./ef/index.md)を参照してください。  
  
 **プライバシー**に関する声明: System.data.oracleclient、system.data.sqlserverce、System.string、、および system.string の各アセンブリは、ユーザーのプライベートデータと非プライベートデータを区別しません。Dll、、および system.string の各アセンブリは、ユーザーのプライベートデータと非プライベートデータを区別しません。  これらのアセンブリによって、ユーザーの個人データの収集、格納、送信が実行されることはありません。 ただし、サードパーティ製のアプリケーションでこれらのアセンブリが使用され、ユーザーの個人データの収集、格納、送信が行われる可能性はあります。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [ADO.NET のアーキテクチャ](ado-net-architecture.md)  
 ADO.NET のアーキテクチャとコンポーネントの概要を説明します。  
  
 [ADO.NET テクノロジのオプションとガイドライン](ado-net-technology-options-and-guidelines.md)  
 エンティティ データ プラットフォームに含まれている製品とテクノロジを説明します。  
  
 [LINQ と ADO.NET](linq-and-ado-net.md)  
 ADO.NET での統合言語クエリ (LINQ) の実装方法を説明し、関連項目へのリンクを示します。  
  
 [.NET Framework データ プロバイダー](data-providers.md)  
 .NET Framework データ プロバイダーと、ADO.NET に同梱される .NET Framework データ プロバイダーのデザインの概要を説明します。  
  
 [ADO.NET データセット](ado-net-datasets.md)  
 `DataSet` のデザインとコンポーネントの概要を説明します。  
  
 [ADO.NET での side-by-side 実行](side-by-side-execution.md)  
 ADO.NET のバージョン間の相違と、その相違が side-by-side 実行とアプリケーションの互換性に及ぼす影響について説明します。  
  
 [ADO.NET のコード例](ado-net-code-examples.md)  
 ADO.NET データ プロバイダーを使用してデータを取得するコード サンプルです。  
  
## <a name="related-sections"></a>関連セクション  
 [ADO.NET の新機能](whats-new.md)  
 ADO.NET の新機能について説明します。  
  
 [ADO.NET アプリケーションのセキュリティ保護](securing-ado-net-applications.md)  
 ADO.NET を使用する場合の安全なコーディング方法について説明します。  
  
 [ADO.NET でのデータ型のマッピング](data-type-mappings-in-ado-net.md)  
 .NET Framework データ型と .NET Framework データ プロバイダーとの間のデータ型のマッピングについて説明します。  
  
 [ADO.NET でのデータの取得および変更](retrieving-and-modifying-data.md)  
 データ ソースへの接続、データの取得、データの変更の方法について説明します。 これには、`DataReaders` と `DataAdapters` が含まれます。  
  
## <a name="see-also"></a>関連項目

- [ADO.NET](index.md)
- [Visual Studio でのデータへのアクセス](/visualstudio/data-tools/accessing-data-in-visual-studio)
