---
title: extern 修飾子 - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- extern_CSharpKeyword
- extern
helpviewer_keywords:
- DllImport attribute
- extern keyword [C#]
ms.assetid: 9c3f02c4-51b8-4d80-9cb2-f2b6e1ae15c7
ms.openlocfilehash: c121d810e64b5fa27f105f814253c0752e028a95
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713538"
---
# <a name="extern-c-reference"></a>extern (C# リファレンス)

`extern` 修飾子は、外部で実装されるメソッドを宣言するために使用します。 `extern` 修飾子は一般に、相互運用サービスを使用してアンマネージ コードを呼び出すときに、`DllImport` 属性と共に使用します。 この場合、次の例に示すように、メソッドを `static` として宣言する必要もあります。

```csharp
[DllImport("avifil32.dll")]
private static extern void AVIFileInit();
```

`extern` キーワードでは、外部アセンブリのエイリアスも定義できます。これにより、単一アセンブリ内から 1 つのコンポーネントの複数バージョンを参照できます。 詳細については、「[extern エイリアス](extern-alias.md)」を参照してください。

[abstract](abstract.md) 修飾子および `extern` 修飾子を一緒に使用して同一のメンバーを修飾すると、エラーになります。 `extern` 修飾子を使用すると、メソッドが C# コードの外部で実装されていることを意味します。一方、`abstract` 修飾子を使用すると、メソッドの実装がクラスには用意されていないことを意味します。

extern キーワードの C# での用法は、C++ の場合よりも制限されています。 C# のキーワードと C++ のキーワードを比較するには、C++ 言語リファレンスの「extern を使用したリンケージの指定」を参照してください。

## <a name="example-1"></a>例 1

この例では、ユーザーの入力したメッセージをプログラムが受け取って、メッセージ ボックスに表示します。 このプログラムは、User32.dll ライブラリからインポートされた `MessageBox` メソッドを使用します。

[!code-csharp[csrefKeywordsModifiers#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#8)]

## <a name="example-2"></a>例 2

この例は、C ライブラリ (ネイティブ DLL) を呼び出す C# プログラムを示しています。

1. 次の C ファイルを作成し、`cmdll.c` という名前を付けます。

    ```c
    // cmdll.c
    // Compile with: -LD
    int __declspec(dllexport) SampleMethod(int i)
    {
      return i*10;
    }
    ```

2. Visual Studio のインストール ディレクトリから Visual Studio x64 (または x32) Native Tools コマンド プロンプト ウィンドウを開き、コマンド プロンプトで「`cmdll.c`cl -LD cmdll.c **」と入力して、** ファイルをコンパイルします。

3. 同じディレクトリに、次の C# ファイルを作成し、`cm.cs` という名前を付けます。

    ```csharp
    // cm.cs
    using System;
    using System.Runtime.InteropServices;
    public class MainClass
    {
        [DllImport("Cmdll.dll")]
          public static extern int SampleMethod(int x);

        static void Main()
        {
            Console.WriteLine("SampleMethod() returns {0}.", SampleMethod(5));
        }
    }
    ```

4. Visual Studio のインストール ディレクトリから Visual Studio x64 (または x32) Native Tools コマンド プロンプト ウィンドウを開き、次のように入力して `cm.cs` ファイルをコンパイルします。

    > **csc cm.cs** (x64 コマンド プロンプトの場合) または **csc -platform:x86 cm.cs** (x32 コマンド プロンプトの場合)

    これで、実行可能ファイル `cm.exe` が作成されます。

5. `cm.exe` を実行します。 `SampleMethod` メソッドは、DLL ファイルに値 5 を渡します。DLL は 10 で乗算した値を返します。  このプログラムの出力は、次のようになります。

    ```output
    SampleMethod() returns 50.
    ```

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.DllImportAttribute?displayProperty=nameWithType>
- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [修飾子](index.md)
