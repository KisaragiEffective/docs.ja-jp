---
description: '詳細情報: コンテキスト接続'
title: コンテキスト接続
ms.date: 03/30/2017
ms.assetid: e443ca86-9243-4234-a822-ed10a53a9de0
ms.openlocfilehash: caea829464ae19a8944f02c4edb38ec41b9bde48
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99767012"
---
# <a name="the-context-connection"></a>コンテキスト接続

内部データ アクセスの問題は、非常に一般的なシナリオです。 つまり、共通言語ランタイム (CLR) ストアド プロシージャまたは関数を実行しているサーバーにアクセスする場合の問題です。 1 つの解決方法は、<xref:System.Data.SqlClient.SqlConnection> を使用して接続文字列を作成し、ローカル サーバーをポイントするように接続文字列で指定して、接続を開くことです。 これを行うには、ログインするための資格情報を指定する必要があります。 接続がストアド プロシージャまたは関数とは別のデータベース セッションにある場合や、異なる `SET` オプションが指定されている場合があります。また、別のトランザクション内にある場合や、一時テーブルを参照していない場合もあります。 マネージド ストアド プロシージャまたは関数コードが SQL Server プロセス内で実行されている場合、別のユーザーがサーバーに接続して、そのマネージド ストアド プロシージャまたは関数コードを起動する SQL ステートメントを実行していることを意味します。 接続のコンテキスト内で、ストアド プロシージャまたは関数をトランザクションや `SET` オプションなどと共に実行する必要がある場合もあります。 これは、コンテキスト接続と呼ばれます。  
  
 コンテキスト接続を使用すると、コードが初めに起動されたコンテキスト内で Transact-SQL ステートメントを実行することができます。 詳細については、ご使用中の SQL Server のバージョンに対応するバージョンの SQL Server オンライン ブックを参照してください。  
  
 **SQL Server のドキュメント**  
  
1. [コンテキスト接続](/sql/relational-databases/clr-integration/data-access/context-connection)  
  
## <a name="see-also"></a>関連項目

- [ADO.NET の概要](../ado-net-overview.md)
