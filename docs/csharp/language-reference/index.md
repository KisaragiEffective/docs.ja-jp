---
title: C# リファレンス
ms.date: 02/14/2017
helpviewer_keywords:
- Visual C#, language reference
- language reference [C#]
- Programmer's Reference for C#
- C# language, reference
- reference, C# language
ms.assetid: 06de3167-c16c-4e1a-b3c5-c27841d4569a
ms.openlocfilehash: 4875e53327e24c4b5983a4a3b79b5beced368725
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74428607"
---
# <a name="c-reference"></a>C# リファレンス

このセクションでは、C# のキーワード、演算子、特殊文字、プリプロセッサ ディレクティブ、コンパイラ オプション、およびコンパイラのエラーと警告に関する参考資料を紹介します。  
  
## <a name="in-this-section"></a>このセクションの内容

 [C# のキーワード](./keywords/index.md)  
 C# のキーワードと構文に関する情報へのリンクを示します。  
  
 [C# 演算子](./operators/index.md)  
 C# の演算子と構文に関する情報へのリンクを示します。  

 [C# の特殊文字](./tokens/index.md)  
 C# のコンテキスト特殊文字とその使用方法に関する情報へのリンクを提供します。  

 [C# プリプロセッサ ディレクティブ](./preprocessor-directives/index.md)  
 C# ソース コード内に埋め込むためのコンパイラ コマンドに関する情報へのリンクを提供します。  
  
 [C# コンパイラ オプション](./compiler-options/index.md)  
 コンパイラ オプションとその使用方法について取り上げます。  
  
 [C# コンパイラ エラー](./compiler-messages/index.md)  
 C# コンパイラのエラーや警告の原因と修正法を示すコード スニペットを示します。  
  
 [C# 言語仕様](../../../_csharplang/spec/introduction.md)  
 C# 6.0 の言語仕様です。 これは、C# 6.0 言語に関するドラフト提案です。 このドキュメントは、ECMA C# 標準委員会との共同作業により改良されています。 バージョン 5.0 は、[Standard ECMA-334 5th Edition](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf) ドキュメントとして 2017 年 12 月にリリースされています。

C# 6.0 より後のバージョンで実装された機能は、言語仕様の提案に示されています。 これらのドキュメントでは、言語仕様に加えた新機能の内容が説明されています。 これらはドラフト提案の形式です。 これらの仕様は、改良ののち、正式なレビューを行うために ECMA 標準委員会に提出され、C# 標準の今後のバージョンに組み込まれる予定です。

 [C# 7.0 仕様の提案](../../../_csharplang/proposals/csharp-7.0/pattern-matching.md)  
 C# 7.0 では多数の新機能が実装されます。 これには、パターン マッチング、ローカル関数、out 変数宣言、throw 式、バイナリ リテラル、および桁区切り文字が含まれます。 このフォルダーには、これらの各機能に関する仕様が含まれています。
  
 [C# 7.1 仕様の提案](../../../_csharplang/proposals/csharp-7.1/async-main.md)  
 C#7.1 には新しい機能が追加されています。 まず、`Main` または `Task` を返す `Task<int>` メソッドを記述できます。 これにより、`async` 修飾子を `Main` に追加することができます。 型を推論できる場所では型なしで `default` 式を使用することができます。 また、タプルのメンバー名を推論することができます。 最後に、ジェネリックにはパターン マッチングを使用できます。

 [C# 7.2 仕様の提案](../../../_csharplang/proposals/csharp-7.2/readonly-ref.md)  
 C#7.2 では小さな機能が複数追加されています。 `in` キーワードを使用して readonly 参照によって引数を渡すことができます。 `Span` および関連する型についてコンパイル時の安全性をサポートするために低レベルの変更が複数加えられました。 名前付き引数を使用できます。この場合、状況によって、後の引数は位置指定引数となります。 `private protected` アクセス修飾子を使用すると、呼び出し元が同じアセンブリ内で実装された派生型に限定されるように指定することができます。 `?:` 演算子は変数への参照に解決される可能性があります。 先頭の桁区切り記号を使用して 16 進数と 2 進数の書式を設定することもできます。

 [C# 7.3 仕様の提案](../../../_csharplang/proposals/csharp-7.3/blittable.md)  
 C#7.3 は、いくつかの小規模な更新プログラムを含む別個のポイント リリースです。 ジェネリック型パラメーターに対して新しい制約を使用できます。 その他の変更により、`fixed` フィールドの操作が簡単になります。これには、[`stackalloc`](./operators/stackalloc.md) 割り当ての使用も含まれます。 `ref` キーワードを使用して参照されるローカル変数は、新しいストレージを参照するように再割り当てされる場合があります。 コンパイラによって生成されたバッキング フィールドを対象とする自動実装プロパティに属性を配置することができます。 初期化子内で式の変数を使用できます。 タプルは、等値 (または非等値) を確認するために比較することができます。 オーバーロードの解決法に対してもいくつかの改良が行われました。
  
 [C# 8.0 仕様の提案](../../../_csharplang/proposals/csharp-8.0/nullable-reference-types.md)  
 C# 8.0 は .NET Core 3.0 で使用できます。 null 許容参照型、再帰的なパターン マッチング、既定のインターフェイス メソッド、非同期ストリーム、範囲とインデックス、using および using 宣言に基づくパターン、null 合体演算子割り当て、読み取り専用インスタンス メンバーなどの機能が含まれています。
  
## <a name="related-sections"></a>関連項目  

 [Visual C# 開発環境の使用](/visualstudio/get-started/csharp)  
 IDE およびエディターについて説明する概念トピックおよびタスク トピックへのリンクを提供します。  
  
 [C# プログラミングガイド](../programming-guide/index.md)  
 C# プログラミング言語の使用方法について説明します。
