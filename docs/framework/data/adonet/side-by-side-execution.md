---
title: side-by-side 実行
ms.date: 03/30/2017
ms.assetid: 9f9ba96d-9f89-4f65-bb2f-6860879f4393
ms.openlocfilehash: a624aac2ed1f3ab124973c84bc74e39297600c8b
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980016"
---
# <a name="side-by-side-execution-in-adonet"></a>ADO.NET での side-by-side 実行
.NET Framework での side-by-side 実行は、アプリケーションがコンパイルされたバージョンを使用して、.NET Framework の複数のバージョンがインストールされているコンピューターでアプリケーションを実行する機能です。 サイドバイサイド実行の構成の詳細については、「side-by-side[実行](../../deployment/side-by-side-execution.md)」を参照してください。  
  
 .NET Framework の1つのバージョンを使用してコンパイルされたアプリケーションは、.NET Framework の異なるバージョンで実行できます。 ただし、インストールされている .NET Framework のバージョンごとにアプリケーションのバージョンをコンパイルし、個別に実行することをお勧めします。 どちらのシナリオでも、アプリケーションの上位互換性または旧バージョンとの互換性に影響する可能性があるリリース間の ADO.NET の変更に注意する必要があります。  
  
## <a name="forward-compatibility-and-backward-compatibility"></a>上位互換性と下位互換性  
 上位互換性とは、アプリケーションを以前のバージョンの .NET Framework でコンパイルできるが、.NET Framework の新しいバージョンでは引き続き正常に動作することを意味します。 .NET Framework バージョン1.1 用に記述された ADO.NET コードは、以降のバージョンとの上位互換性があります。  
  
 旧バージョンとの互換性とは、アプリケーションが新しいバージョンの .NET Framework 用にコンパイルされても、機能を損なうことなく、.NET Framework の以前のバージョンで引き続き実行されることを意味します。 もちろん、これは新しいバージョンの .NET Framework で導入された機能には当てはまりません。  
  
## <a name="the-net-framework-data-provider-for-odbc"></a>.NET Framework Data Provider for ODBC  
 バージョン1.1 以降では、ODBC の .NET Framework Data Provider (<xref:System.Data.Odbc>) が .NET Framework の一部として含まれています。
  
 ODBC データプロバイダーを使用してデータソースに接続する .NET Framework バージョン1.0 用に開発されたアプリケーションがあり、そのアプリケーションを .NET Framework バージョン1.1 以降のバージョンで実行する場合は、ODBC データプロバイダーの名前**空間を system.string に更新**する必要があります。 次に、新しいバージョンの .NET Framework 用に再コンパイルする必要があります。  
  
 ODBC データプロバイダーを使用してデータソースに接続する .NET Framework バージョン2.0 以降用に開発されたアプリケーションがあり、そのアプリケーションを .NET Framework バージョン1.0 で実行する場合は、ODBC データプロバイダーをダウンロードしてインストールする必要があります.NET Framework バージョン1.0 システムの場合。 次に、ODBC データプロバイダーの名前**空間を変更**して、.NET Framework バージョン1.0 のアプリケーションを再コンパイルする必要があります。  
  
## <a name="the-net-framework-data-provider-for-oracle"></a>.NET Framework Data Provider for Oracle  
 バージョン1.1 以降では、Oracle の .NET Framework Data Provider (<xref:System.Data.OracleClient>) が .NET Framework の一部として含まれています。
  
 データプロバイダーを使用してデータソースに接続する .NET Framework バージョン2.0 以降用に開発されたアプリケーションがあり、そのアプリケーションを .NET Framework バージョン1.0 で実行する場合は、データプロバイダーをダウンロードして、. NE にインストールする必要があります。T Framework バージョン1.0 システム。  
  
## <a name="code-access-security"></a>コード アクセス セキュリティ  
 .NET Framework バージョン 1.0 (<xref:System.Data.SqlClient>、<xref:System.Data.OleDb>) の .NET Framework データプロバイダーは、FullTrust アクセス許可で実行する必要があります。 .NET Framework バージョン1.0 の .NET Framework k データプロバイダーを FullTrust アクセス許可より低いゾーンで使用しようとすると、<xref:System.Security.SecurityException>が発生します。  
  
 ただし、.NET Framework バージョン2.0 以降では、一部の .NET Framework データプロバイダーを部分信頼ゾーンで使用できます。 さらに、.NET Framework バージョン1.1 の .NET Framework データプロバイダーに新しいセキュリティ機能が追加されました。 この機能により、特定のセキュリティ ゾーンで使用できる接続文字列を制限することができます。 特定のセキュリティ ゾーンに対して空白のパスワードの使用を禁止することもできます。 詳細については、「 [Code Access Security and ADO.NET](code-access-security.md)」を参照してください。  
  
 .NET Framework の各インストールには個別のセキュリティ .config ファイルがあるため、セキュリティ設定には互換性の問題はありません。 ただし、アプリケーションが .NET Framework バージョン1.1 以降に含まれる ADO.NET の追加のセキュリティ機能に依存している場合は、バージョン1.0 システムに配布することはできません。  
  
## <a name="sqlcommand-execution"></a>SqlCommand の実行  
 .NET Framework バージョン1.1 以降では、データソースで <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> を実行する方法が変更されました。  
  
 .NET Framework バージョン1.0 では、<xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> **sp_executesql**ストアドプロシージャのコンテキストですべてのコマンドを実行しました。 その結果、接続の状態に影響を与えるコマンド (たとえば、SET NOCOUNT ON) は、現在のコマンドの実行だけに適用されます。 接続が開かれている間に実行される後続のコマンドについては、接続の状態は変更されません。  
  
 .NET Framework バージョン1.1 以降では、コマンドにパラメーターが含まれている場合、<xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> は**sp_executesql**ストアドプロシージャのコンテキストでのみコマンドを実行します。これにより、パフォーマンスが向上します。 その結果、接続の状態に影響を与えるコマンドが、非パラメーター化コマンドに含まれている場合、接続が開いている間に実行される後続のすべてのコマンドに対して、接続の状態を変更します。  
  
 たとえば、<xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> への呼び出しで次のバッチ コマンドが実行されるとします。  
  
```sql
SET NOCOUNT ON;  
SELECT * FROM dbo.Customers;  
```  
  
 .NET Framework バージョン1.1 以降では、接続が開いている間に後続のコマンドが実行されると、NOCOUNT はオンのままになります。 .NET Framework バージョン1.0 では、NOCOUNT は現在のコマンドの実行に対してのみ ON になります。  
  
 この変更は、いずれかのバージョンの .NET Framework の <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> の動作に依存する場合に、アプリケーションの上位互換性と下位互換性の両方に影響を与える可能性があります。  
  
 .NET Framework の以前のバージョンとそれ以降のバージョンの両方で実行されるアプリケーションでは、実行しているバージョンに関係なく、動作が同じであることを確認するコードを記述できます。 変更された接続状態が後続のすべてのコマンドでも有効になるようにする場合は、<xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> を使用してコマンドを実行することをお勧めします。 変更された接続状態が後続のコマンドでは無効になるようにする場合は、接続状態をリセットするコマンドを含めるようにしてください。 例:  
  
```sql
SET NOCOUNT ON;  
SELECT * FROM dbo.Customers;  
SET NOCOUNT OFF;  
```  
  
## <a name="see-also"></a>関連項目

- [ADO.NET の概要](ado-net-overview.md)
- [ADO.NET でのデータの取得および変更](retrieving-and-modifying-data.md)
