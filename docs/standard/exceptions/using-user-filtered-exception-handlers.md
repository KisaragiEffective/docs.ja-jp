---
title: ユーザー フィルター例外ハンドラーの使用
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- user-filtered exceptions
- exceptions, user-filtered
ms.assetid: aa80d155-060d-41b4-a636-1ceb424afee8
ms.openlocfilehash: 5537404178b746310f720c5b0c075c77287dda4c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708454"
---
# <a name="using-user-filtered-exception-handlers"></a>ユーザー フィルター例外ハンドラーの使用

現在 Visual Basic では、ユーザー フィルター例外をサポートしています。 ユーザー フィルター例外ハンドラーは、ユーザーが例外に対して定義した要件に基づいて、例外をキャッチして処理します。 これらのハンドラーでは、**Catch** ステートメントを **When** キーワードと一緒に使用します。  
  
 この手法は、特定の例外オブジェクトが複数のエラーに対応する場合に便利です。 その場合、オブジェクトには通常、エラーに関連付けられた特定のエラー コードが格納されているプロパティがあります。 エラー コード プロパティを式で使用すると、その **Catch** 句で処理する特定のエラーだけを選択することができます。  
  
 **Catch/When** ステートメントを使用した Visual Basic コードの例を次に示します。  
  
```vb
Try  
    'Try statements.  
    Catch When Err = VBErr_ClassLoadException
    'Catch statements.
End Try  
```  
  
 ユーザー フィルター句の式が制限されることはありません。 ユーザー フィルター式の実行中に例外が発生した場合、その例外は破棄され、フィルター式は false と評価されたものと見なされます。 その場合、共通言語ランタイムは、現在の例外に対応するハンドラーの検索を継続します。  
  
## <a name="combining-the-specific-exception-and-the-user-filtered-clauses"></a>特定の例外とユーザー フィルター句の組み合わせ  
 catch ステートメントには、特定の例外とユーザー フィルター句の両方を記述できます。 ランタイムは、最初に特定の例外をテストします。 特定の例外がテストを通過すると、ランタイムはユーザー フィルターを実行します。 汎用フィルターには、クラス フィルターで宣言されている変数への参照を含めることができます。 なお、2 つのフィルター句の順序をが逆にすることはできません。  
  
 次に示すのは、`ClassLoadException` という例外が指定された **Catch** ステートメントと、**When** キーワードを使用したユーザー フィルター句がある Visual Basic コードの例です。  
  
```vb
Try  
    'Try statements.
    Catch cle As ClassLoadException When cle.IsRecoverable()  
    'Catch statements.
End Try  
```  

## <a name="see-also"></a>参照

- [例外](index.md)
