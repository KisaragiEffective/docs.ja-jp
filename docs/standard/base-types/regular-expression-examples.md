---
title: 正規表現の例
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- regular expressions [.NET Framework], examples
- regular expressions [.NET Framework]
- strings [.NET Framework], regular expressions
ms.assetid: e9fd53f2-ed56-4b09-b2ea-e9bc9d65e6d6
ms.openlocfilehash: 788fa2a6793e14189def4c30a0baf0d4a5cf6b0a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73128091"
---
# <a name="regular-expression-examples"></a>正規表現の例
このセクションでは、一般的なアプリケーションで正規表現を使用するときのコード例を示します。  
  
> [!NOTE]
> <xref:System.Web.RegularExpressions> 名前空間には、正規表現オブジェクトがたくさん含まれています。このオブジェクトは、HTML、XML、ASP.NET 文書の文字列を解析する事前定義済み正規表現パターンを実装します。 たとえば、<xref:System.Web.RegularExpressions.TagRegex> クラスは文字列の開始タグを識別します。<xref:System.Web.RegularExpressions.CommentRegex> クラスは文字列の ASP.NET コメントを識別します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [例: HREFS のスキャン](../../../docs/standard/base-types/regular-expression-example-scanning-for-hrefs.md)  
 入力文字列を検索して、文字列内のすべての href="..." の値と位置を出力する例を示します。  
  
 [例: 日付形式の変更](../../../docs/standard/base-types/regular-expression-example-changing-date-formats.md)  
 mm/dd/yy 形式の日付を dd-mm-yy 形式の日付に置換する例を示します。  
  
 [方法: URL からプロトコルとポート番号を抽出する](../../../docs/standard/base-types/how-to-extract-a-protocol-and-port-number-from-a-url.md)  
 URL を含む文字列からプロトコルとポート番号を抽出する例を示します。 たとえば、"http://www.contoso.com:8080/letters/readme.html" の場合は "http:8080" が返されます。  
  
 [方法: 文字列から無効な文字を取り除く](../../../docs/standard/base-types/how-to-strip-invalid-characters-from-a-string.md)  
 文字列から無効な非英数文字を取り除く例を示します。  
  
 [方法: 文字列が有効な電子メール形式であるかどうかを検証する](../../../docs/standard/base-types/how-to-verify-that-strings-are-in-valid-email-format.md)  
 文字列が有効な電子メール形式であることを確認する例を次に示します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Text.RegularExpressions>  
 .NET **System.Text.RegularExpressions** 名前空間のクラス ライブラリのリファレンス情報を示します。  
  
## <a name="related-sections"></a>関連項目  
 [.NET の正規表現](../../../docs/standard/base-types/regular-expressions.md)  
 正規表現のプログラミング言語的な面の概要について説明します。  
  
 [正規表現のオブジェクト モデル](../../../docs/standard/base-types/the-regular-expression-object-model.md)  
 `System.Text.RegularExpression` 名前空間に含まれている正規表現クラスについて説明し、その使用例を示します。  
  
 [正規表現の動作の詳細](../../../docs/standard/base-types/details-of-regular-expression-behavior.md)  
 .NET の正規表現の機能と動作について説明します。  
  
 [正規表現言語 - クイック リファレンス](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)  
 正規表現を定義するために使う一連の文字、演算子、および構成体について説明します。
