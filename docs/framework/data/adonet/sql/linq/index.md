---
title: LINQ to SQL
ms.date: 03/30/2017
ms.assetid: 73d13345-eece-471a-af40-4cc7a2f11655
ms.openlocfilehash: 4553be9eeab8792197503d0b1de872f4494277b6
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634626"
---
# <a name="linq-to-sql"></a>LINQ to SQL
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は .NET Framework バージョン3.5 のコンポーネントであり、リレーショナルデータをオブジェクトとして管理するためのランタイムインフラストラクチャを提供します。  
  
> [!NOTE]
> リレーショナル データは 2 次元テーブル (*リレーション*または*フラット ファイル*) のコレクションで表され、共通の列がテーブルを相互に関連付けます。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] を効果的に使用するには、リレーショナル データベースの基本原則を理解している必要があります。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] では、リレーショナル データベースのデータ モデルが、開発者のプログラミング言語で表されるオブジェクト モデルに対応付けられています。 アプリケーションが実行されると、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、オブジェクト モデルの統合言語クエリを SQL に変換し、それをデータベースに送信して実行します。 データベースから結果が返されると、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] はそれをプログラミング言語で操作できるオブジェクトに変換し直します。  
  
 通常、Visual Studio を使用する開発者は、オブジェクトリレーショナルデザイナーを使用します。これは、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]の多くの機能を実装するためのユーザーインターフェイスを提供します。  
  
 このリリースの [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] に含まれているドキュメントでは、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] アプリケーションを構築するのに必要な基本的なビルド ブロック、プロセス、および手法について説明します。 また、特定の問題について Microsoft Docs を検索したり、 [LINQ フォーラム](https://go.microsoft.com/fwlink/?LinkId=76488)に参加したりすることもできます。このトピックでは、より複雑なトピックについて、専門家に詳しく説明します。 最後に、 [LINQ to SQL: リレーショナルデータのための .Net 統合言語クエリに](https://go.microsoft.com/fwlink/?LinkId=93205)関するホワイトペーパーでは、Visual Basic とC#コード例を使用して、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] テクノロジについて詳しく説明しています。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [はじめに](getting-started.md)  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] の簡単な概要と、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] を使用して作業を始める方法を示します。  
  
 [プログラミング ガイド](programming-guide.md)  
 マップ、クエリ、更新、デバッグ、および同様のタスクを行う手順を示します。  
  
 [参照](reference.md)  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のさまざまな側面に関するリファレンス情報を示します。 SQL-CLR 型マッピング、標準クエリ演算子変換などのトピックが含まれます。  
  
 [サンプル](samples.md)  
 Visual Basic とC#サンプルへのリンクを示します。  
  
## <a name="related-sections"></a>関連セクション  
 [統合言語クエリ (LINQ)- C# ](../../../../../csharp/programming-guide/concepts/linq/index.md)\
 の LINQ テクノロジの概要にC#ついて説明します。
 
 [統合言語クエリ (LINQ) - Visual Basic](../../../../../visual-basic/programming-guide/concepts/linq/index.md)  
 Visual Basic の LINQ テクノロジの概要について説明します。
  
 [LINQ](../../../../../visual-basic/programming-guide/language-features/linq/index.md)  
 Visual Basic ユーザー向けの LINQ テクノロジについて説明します。  
  
 [LINQ と ADO.NET](../../linq-and-ado-net.md)  
 ADO.NET ポータルにリンクします。  
  
 [LINQ to SQL チュートリアル](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/bb386295(v=vs.90))  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のチュートリアルを一覧します。  
  
 [サンプル データベースのダウンロード](downloading-sample-databases.md)  
 ドキュメントで使用されるサンプル データベースをダウンロードする方法について説明します。  
  
 [LinqDataSource Web サーバーコントロールの概要](https://docs.microsoft.com/previous-versions/aspnet/bb547113(v=vs.100))  
 <xref:System.Web.UI.WebControls.LinqDataSource> コントロールが、ASP.NET データソースコントロールアーキテクチャを使用して、統合言語クエリ (LINQ) を Web 開発者に公開する方法について説明します。
