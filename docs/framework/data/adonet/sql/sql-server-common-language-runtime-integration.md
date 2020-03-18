---
title: SQL Server の共通言語ランタイム統合
ms.date: 03/30/2017
ms.assetid: c7a324c4-160d-44c2-b593-641af06eca61
ms.openlocfilehash: 12ae15d72644e314aa694f8d169bc8f45fa284a2
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452345"
---
# <a name="sql-server-common-language-runtime-integration"></a>SQL Server の共通言語ランタイム統合
SQL Server 2005 で、Microsoft Windows 用の .NET Framework の共通言語ランタイム (CLR) コンポーネントの統合が導入されました。 つまり、Microsoft Visual Basic .NET や Microsoft Visual C# を含む任意の .NET Framework 言語を使用して、ストアド プロシージャ、トリガー、ユーザー定義型、ユーザー定義関数、ユーザー定義集計、およびストリーミング テーブル値関数を作成できるようになりました。 マネージド コードが Microsoft SQL Server 環境とデータをやり取りできるように、<xref:Microsoft.SqlServer.Server> 名前空間には新しいアプリケーション プログラミング インターフェイス (API) のセットが含まれています。  
  
 このセクションでは、SQL Server の共通言語ランタイム (CLR) 統合および SQL Server のインプロセス固有の ADO.NET の拡張に固有の機能や動作ついて説明します。  
  
 このセクションは、SQL Server の CLR 統合を利用したプログラミングを始めるのに十分な情報を提供することを目的としており、包括的な情報の提供は目的としていません。 詳細については、ご使用中の SQL Server のバージョンに対応するバージョンの SQL Server オンライン ブックを参照してください。  
  
 **SQL Server のドキュメント**  
  
1. [CLR (共通言語ランタイム) 統合のプログラミング概念](/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts)  
  
## <a name="in-this-section"></a>このセクションの内容  
 [SQL Server の CLR 統合の概要](introduction-to-sql-server-clr-integration.md)  
 SQL Server の CLR 統合の概要について説明します。 追加情報へのリンクもあります。  
  
 [CLR ユーザー定義関数](clr-user-defined-functions.md)  
 テーブル値関数、スカラー関数、ユーザー定義集計関数など、さまざまな種類の CLR 関数の実装方法と使用方法について説明します。  
  
 [CLR ユーザー定義型](clr-user-defined-types.md)  
 CLR ユーザー定義型の実装方法と使用方法について説明します。 追加情報へのリンクもあります。  
  
 [CLR ストアド プロシージャ](clr-stored-procedures.md)  
 CLR ストアド プロシージャの実装方法と使用方法について説明します。 追加情報へのリンクもあります。  
  
 [CLR トリガー](clr-triggers.md)  
 CLR トリガーの実装方法と使用方法について説明します。 追加情報へのリンクもあります。  
  
 [コンテキスト接続](the-context-connection.md)  
 コンテキスト接続について説明します。  
  
 [SQL Server のインプロセス固有の ADO.NET の動作](sql-server-in-process-specific-behavior-of-adonet.md)  
 SQL Server のインプロセス固有の ADO.NET の拡張およびコンテキスト接続について説明します。 追加情報へのリンクもあります。  
  
## <a name="see-also"></a>参照

- [SQL Server と ADO.NET](index.md)
- [ADO.NET の概要](../ado-net-overview.md)
