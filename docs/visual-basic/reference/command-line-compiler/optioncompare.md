---
title: -optioncompare
ms.date: 07/20/2015
f1_keywords:
- /optioncompare
- -optioncompare
helpviewer_keywords:
- optioncompare compiler option [Visual Basic]
- -optioncompare compiler option [Visual Basic]
- /optioncompare compiler option [Visual Basic]
ms.assetid: 7237b766-b44d-4cc5-9a3c-885348a7d9e4
ms.openlocfilehash: ac385880f8c13c23dffff67fc2a1ecc5609fd189
ms.sourcegitcommit: 1f12db2d852d05bed8c53845f0b5a57a762979c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "72581413"
---
# <a name="-optioncompare"></a>-optioncompare

文字列比較の方法を指定します。

## <a name="syntax"></a>構文

```console
-optioncompare:{binary | text}
```

## <a name="remarks"></a>Remarks

2つの形式のいずれかで `-optioncompare` を指定できます。バイナリ文字列比較を使用する場合は `-optioncompare:binary`、テキスト文字列比較を使用する場合は `-optioncompare:text` です。 既定では、コンパイラは `-optioncompare:binary` を使用します。

Microsoft Windows では、現在のコードページによってバイナリ並べ替え順序が決定されます。 一般的なバイナリ並べ替え順序は次のとおりです。

`A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`

テキストベースの文字列比較は、システムのロケールによって決定される、大文字と小文字を区別しないテキストの並べ替え順序に基づいています。 一般的なテキストの並べ替え順序は次のとおりです。

`(A = a) < (À = à) < (B=b) < (E=e) < (Ê = ê) < (Z=z) < (Ø = ø)`

### <a name="to-set--optioncompare-in-the-visual-studio-ide"></a>Visual Studio IDE で-optioncompare を設定するには

1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。

2. **[コンパイル]** タブをクリックします。

3. **[オプションの比較]** ボックスの値を変更します。

### <a name="to-set--optioncompare-programmatically"></a>-Optioncompare プログラムを設定するには

[Option Compare ステートメント](../../../visual-basic/language-reference/statements/option-compare-statement.md)を参照してください。

## <a name="example"></a>例

次のコードは `ProjFile.vb` をコンパイルし、バイナリ文字列比較を使用します。

```console
vbc -optioncompare:binary projFile.vb
```

## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-optionexplicit](../../../visual-basic/reference/command-line-compiler/optionexplicit.md)
- [-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)
- [-optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Option Compare ステートメント](../../../visual-basic/language-reference/statements/option-compare-statement.md)
- [[Visual Basic の既定値] ([オプション] ダイアログ ボックス - [プロジェクト])](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
