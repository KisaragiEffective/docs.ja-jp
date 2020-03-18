---
title: -optionexplicit
ms.date: 07/20/2015
f1_keywords:
- /optionexplicit
- -optionexplicit
helpviewer_keywords:
- /optionexplicit compiler option [Visual Basic]
- optionexplicit compiler option [Visual Basic]
- -optionexplicit compiler option [Visual Basic]
ms.assetid: 5d296ab3-bafe-4c4d-9887-78f162ed86c7
ms.openlocfilehash: 5c0946b94bfe02d797d1a484088869375703eb6a
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005310"
---
# <a name="-optionexplicit"></a>-optionexplicit
変数が使用される前に宣言されていない場合、コンパイラはエラーを報告します。  
  
## <a name="syntax"></a>構文  
  
```console  
-optionexplicit[+ | -]  
```  
  
## <a name="arguments"></a>引数  
 `+` &#124; `-`  
 任意。 変数の明示的な宣言が必要な場合は、`-optionexplicit+` を指定します。 @No__t-0 オプションは既定値で、`-optionexplicit` と同じです。 @No__t-0 オプションを指定すると、変数の暗黙的な宣言が有効になります。  
  
## <a name="remarks"></a>コメント  
 ソースコードファイルに[Option Explicit ステートメント](../../../visual-basic/language-reference/statements/option-explicit-statement.md)が含まれている場合、ステートメントは `-optionexplicit` コマンドラインコンパイラ設定をオーバーライドします。  
  
### <a name="to-set--optionexplicit-in-the-visual-studio-ide"></a>Visual Studio IDE で-optionexplicit を設定するには  
  
1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。   
  
2. **[コンパイル]** タブをクリックします。  
  
3. **[Option Explicit]** ボックスの値を変更します。  
  
## <a name="example"></a>例  
 次のコードは、`-optionexplicit-` が指定されている場合にコンパイルされます。  
  
 [!code-vb[VbVbalrCompiler#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionExplicitOff.vb#5)]  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-optioncompare](../../../visual-basic/reference/command-line-compiler/optioncompare.md)
- [-optionstrict](../../../visual-basic/reference/command-line-compiler/optionstrict.md)
- [-optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Option Explicit ステートメント](../../../visual-basic/language-reference/statements/option-explicit-statement.md)
- [[Visual Basic の既定値] ([オプション] ダイアログ ボックス - [プロジェクト])](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
