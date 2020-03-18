---
title: -optionstrict
ms.date: 07/20/2015
f1_keywords:
- -optionstrict
helpviewer_keywords:
- -optionstrict compiler option [Visual Basic]
- optionstrict compiler option [Visual Basic]
- /optionstrict compiler option [Visual Basic]
ms.assetid: c7b10086-0fa4-49db-b3c8-4ae0db5957da
ms.openlocfilehash: 469e22aef9d746fc55e04ba884d17d60d8baa85a
ms.sourcegitcommit: 1f12db2d852d05bed8c53845f0b5a57a762979c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "72583076"
---
# <a name="-optionstrict"></a>-optionstrict

厳密な型のセマンティクスを適用して、暗黙的な型変換を制限します。

## <a name="syntax"></a>構文

```console
-optionstrict[+ | -]
-optionstrict[:custom]
```

## <a name="arguments"></a>引数

`+` &#124; `-`  
省略可能です。 `-optionstrict+` を指定すると、暗黙の型変換が制限されます。このオプションの既定値は `-optionstrict-` です。 `-optionstrict+` オプションは `-optionstrict` と同じ動作になります。寛容な型指定規則では、どちらも使用できます。

`custom`  
必ず指定します。厳密な言語規則が守られていない場合に警告します。

## <a name="remarks"></a>Remarks

`-optionstrict+` を指定すると、拡張型変換だけが暗黙のうちに行われます。10 進型 (`Decimal`) のオブジェクトを整数型(`Integer`) のオブジェクトに代入するなど、縮小による暗黙の型変換はエラーとして報告されます。

暗黙の縮小変換で警告を生成するには、`-optionstrict:custom` を使用します。特定の警告を無視するには `-warnaserror:numberlist`、特定の警告をエラーとして扱うようにするには`-nowarn:numberlist` を使用します。


### <a name="to-set--optionstrict-in-the-visual-studio-ide"></a>Visual Studio IDE で-optionstrict を設定するには

1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **プロジェクト** メニューの プロパティ をクリックし**ます。**

2. **[コンパイル]** タブをクリックします。

3. **[Option Strict]** ボックスの値を変更します。

### <a name="to-set--optionstrict-programmatically"></a>プログラムによって-optionstrict を設定するには

「[Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)」を参照してください。

## <a name="example"></a>例

次のコードは、厳密な型のセマンティクスを使用して `Test.vb` をコンパイルします。

```console
vbc -optionstrict+ test.vb
```

## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-optioncompare](../../../visual-basic/reference/command-line-compiler/optioncompare.md)
- [-optionexplicit](../../../visual-basic/reference/command-line-compiler/optionexplicit.md)
- [-optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)
- [-nowarn](../../../visual-basic/reference/command-line-compiler/nowarn.md)
- [-warnaserror (Visual Basic)](../../../visual-basic/reference/command-line-compiler/warnaserror.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [[Visual Basic の既定値] ([オプション] ダイアログ ボックス - [プロジェクト])](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
