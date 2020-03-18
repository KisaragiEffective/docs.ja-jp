---
title: -netcf
ms.date: 03/13/2018
f1_keywords:
- /netcf
- -netcf
helpviewer_keywords:
- -netcf compiler option [Visual Basic]
- netcf compiler option [Visual Basic]
- /netcf compiler option [Visual Basic]
ms.assetid: db7cfa59-c315-401c-a59b-0daf355343d6
ms.openlocfilehash: 7f14ce07a2928f4dbffd3aa57f8cdd514b75694c
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716717"
---
# <a name="-netcf"></a>-netcf

.NET Compact Framework を対象とするようにコンパイラを設定します。

## <a name="syntax"></a>構文

```console
-netcf
```

## <a name="remarks"></a>Remarks

`-netcf` オプションを指定すると、Visual Basic コンパイラは完全な .NET Framework ではなく .NET Compact Framework を対象とします。 完全な .NET Framework にのみ存在する言語機能は無効になっています。

`-netcf` オプションは、 [-sdkpath](../../../visual-basic/reference/command-line-compiler/sdkpath.md)と共に使用するように設計されています。 `-netcf` によって無効にされた言語機能は、`-sdkpath`の対象となるファイルには存在しないものと同じ言語機能です。

> [!NOTE]
> `-netcf` オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。 `-netcf` オプションは、Visual Basic デバイスプロジェクトが読み込まれるときに設定されます。

`-netcf` オプションは、次の言語機能を変更します。

- [End \<keyword > ステートメント](../../../visual-basic/language-reference/statements/end-keyword-statement.md)キーワードは、プログラムの実行を終了しますが、無効になっています。 次のプログラムは `-netcf` せずにコンパイルして実行しますが、コンパイル時に `-netcf`で失敗します。

  [!code-vb[VbVbalrCompiler#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/netcf.vb#34)]

- すべての形式の遅延バインディングは無効になっています。 コンパイル時のエラーは、認識された遅延バインディングのシナリオが検出された場合に生成されます。 次のプログラムは `-netcf` せずにコンパイルして実行しますが、コンパイル時に `-netcf`で失敗します。

  [!code-vb[VbVbalrCompiler#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#35)]

- [Auto](../../../visual-basic/language-reference/modifiers/auto.md)、 [Ansi](../../../visual-basic/language-reference/modifiers/ansi.md)、および[Unicode](../../../visual-basic/language-reference/modifiers/unicode.md)の各修飾子は無効になっています。 [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)の構文も `Declare Sub|Function name Lib "library" [Alias "alias"] [([arglist])]`に変更されています。 次のコードは、コンパイル時の `-netcf` の効果を示しています。

  [!code-vb[VbVbalrCompiler#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#36)]

- Visual Basic から削除された Visual Basic 6.0 キーワードを使用すると、`-netcf` の使用時に別のエラーが生成されます。 これは、次のキーワードのエラーメッセージに影響します。

  - `Open`

  - `Close`

  - `Put`

  - `Print`

  - `Write`

  - `Input`

  - `Lock`

  - `Unlock`

  - `Seek`

  - `Width`

  - `Name`

  - `FreeFile`

  - `EOF`

  - `Loc`

  - `LOF`

  - `Line`

## <a name="example"></a>使用例

次のコードでは、C ドライブの .NET Compact Framework の既定のインストールディレクトリにある mscorlib.dll と Microsoft. .dll のバージョンを使用して、.NET Compact Framework で `Myfile.vb` をコンパイルします。 通常は、最新バージョンの .NET Compact Framework を使用します。

```console
vbc -netcf -sdkpath:"c:\Program Files\Microsoft Visual Studio .NET 2003\CompactFrameworkSDK\v1.0.5000\Windows CE " myfile.vb
```

## <a name="see-also"></a>関連項目

- [Visual Basic のコマンド ライン コンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [-sdkpath](../../../visual-basic/reference/command-line-compiler/sdkpath.md)
