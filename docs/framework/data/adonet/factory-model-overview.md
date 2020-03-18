---
title: ファクトリ モデルの概要
ms.date: 03/30/2017
ms.assetid: b5dc81c4-7554-44b9-b513-769bd61e2e7b
ms.openlocfilehash: f344502453acbbb5c08e49b7c21a0f4e84a29ffa
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348394"
---
# <a name="factory-model-overview"></a>ファクトリ モデルの概要
ADO.NET 2.0 では、<xref:System.Data.Common> 名前空間に新しい基本クラスが導入されました。 基本クラスは抽象的な存在です。つまり、直接インスタンス化することはできません。 新しい基本クラスとしては、<xref:System.Data.Common.DbConnection>、<xref:System.Data.Common.DbCommand>、<xref:System.Data.Common.DbDataAdapter> があり、<xref:System.Data.SqlClient> や <xref:System.Data.OleDb> などの .NET Framework データ プロバイダーによって共有されます。 基本クラスが追加されたことで、新しいインターフェイスを作成しなくても、.NET Framework データ プロバイダーに対して容易に機能を追加できるようになりました。  
  
 また、ADO.NET 2.0 で抽象基本クラスが導入されたことにより、開発者は特定のデータ プロバイダーに依存しない汎用的なデータ アクセス コードを記述できるようになりました。  
  
## <a name="the-factory-design-pattern"></a>ファクトリ デザイン パターン  
 単一の API で複数プロバイダーのデータベースにアクセスできる "ファクトリ" デザイン パターンの使用は、プロバイダーに依存しないコードを作成するプログラミング モデルの基礎となるものです。 このパターンでは、現実世界の工場のように、他のオブジェクトを作成するためだけに特殊なオブジェクトを使用するため、適切なネーミングがなされていると言えます。 ファクトリデザインパターンの詳細については、「 [ASP.NET 2.0 および ADO.NET 2.0 での汎用データアクセスコードの記述](https://docs.microsoft.com/previous-versions/dotnet/articles/ms971499(v=msdn.10))」を参照してください。
  
 ADO.NET 2.0 以降では、<xref:System.Data.Common.DbProviderFactories> のインスタンスを作成するための `static` (Visual Basic の場合は `Shared`) メソッドが <xref:System.Data.Common.DbProviderFactory> クラスに用意されています。 このインスタンスにより、実行時に指定したプロバイダー情報と接続文字列に基づいて、厳密に型指定された適切なオブジェクトが返されます。  
  
## <a name="see-also"></a>関連項目

- [DbProviderFactory の取得](obtaining-a-dbproviderfactory.md)
- [DbConnection、DbCommand、および DbException](dbconnection-dbcommand-and-dbexception.md)
- [DbDataAdapter を使用したデータの変更](modifying-data-with-a-dbdataadapter.md)
- [ADO.NET の概要](ado-net-overview.md)
