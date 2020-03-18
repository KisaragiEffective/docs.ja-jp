---
title: '#line - C# リファレンス'
ms.date: 07/20/2015
f1_keywords:
- '#line'
helpviewer_keywords:
- '#line directive [C#]'
ms.assetid: 6439e525-5dd5-4acb-b8ea-efabb32ff95b
ms.openlocfilehash: 79033fa652af62c76d54737fbf0a0b47cf3aae99
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712495"
---
# <a name="line-c-reference"></a>#line (C# リファレンス)

`#line` を使用すると、コンパイラの行番号および (必要に応じて) エラーと警告に出力されるファイル名を変更することができます。

次の例では、行番号に関連付けられている 2 つの警告を報告する方法を示します。 `#line 200` ディレクティブは次の行番号が 200 (既定では #6) になるように強制し、次の `#line` ディレクティブまでファイル名を "Special" として報告します。 `#line default` ディレクティブは、行の番号付けをその既定の番号付けに戻します。つまり、前のディレクティブで番号が付け直された行をカウントします。

```csharp
class MainClass
{
    static void Main()
    {
#line 200 "Special"
        int i;
        int j;
#line default
        char c;
        float f;
#line hidden // numbering not affected
        string s;
        double d;
    }
}
```

コンパイルで生成される出力は次のとおりです。

```console
Special(200,13): warning CS0168: The variable 'i' is declared but never used
Special(201,13): warning CS0168: The variable 'j' is declared but never used
MainClass.cs(9,14): warning CS0168: The variable 'c' is declared but never used
MainClass.cs(10,15): warning CS0168: The variable 'f' is declared but never used
MainClass.cs(12,16): warning CS0168: The variable 's' is declared but never used
MainClass.cs(13,16): warning CS0168: The variable 'd' is declared but never used
```

## <a name="remarks"></a>解説

`#line` ディレクティブは、ビルド プロセスで自動化された中間ステップで使用される場合があります。 たとえば、行が元のソース コード ファイルから削除されても、ファイル内の元の行番号付けに基づいてコンパイラに引き続き出力を生成させる場合は、行を削除してから `#line` を使用して元の行番号付けをシミュレートできます。

開発者がコードをステップ実行すると、`#line hidden` と次の `#line hidden` ディレクティブ (別の `#line` ディレクティブではないと仮定) の間のすべての行がステップ オーバーされるように、`#line hidden` ディレクティブは、連続する行をデバッガーから隠します。 このオプションは、ユーザー定義のコードとコンピューターによって生成されたコードを ASP.NET が区別できるようするために使用することもできます。 この機能を主に使用しているのは ASP.NET ですが、より多くのソース ジェネレーターが利用できる可能性はあります。

`#line hidden` ディレクティブはエラーの報告でのファイル名や行番号には影響しません。 つまり、非表示のブロックでエラーが発生した場合、コンパイラはエラーの現在のファイル名と行番号を報告します。

`#line filename` ディレクティブは、コンパイラ出力に表示するファイル名を指定します。 既定では、ソース コード ファイルの実際の名前が使用されます。 ファイル名は、二重引用符 ("") で囲み、前に行番号を付ける必要があります。

ソース コード ファイルは、任意の数の `#line` ディレクティブを持つことができます。

## <a name="example-1"></a>例 1

次の例では、デバッガーがコード内の非表示の行を無視する方法を示します。 例を実行すると、次の 3 行のテキストが表示されます。 しかし、例に示されているようにブレークポイントを設定し、F10 キーを押してコードをステップ実行すると、デバッガーが非表示の行を無視することがわかります。 非表示の行にブレークポイントを設定した場合でも、デバッガーは無視することに注目してください。

```csharp
// preprocessor_linehidden.cs
using System;
class MainClass
{
    static void Main()
    {
        Console.WriteLine("Normal line #1."); // Set break point here.
#line hidden
        Console.WriteLine("Hidden line.");
#line default
        Console.WriteLine("Normal line #2.");
    }
}
```

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# プリプロセッサ ディレクティブ](./index.md)
