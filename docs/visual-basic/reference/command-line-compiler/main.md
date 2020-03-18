---
title: -main
ms.date: 03/13/2018
helpviewer_keywords:
- main compiler option [Visual Basic]
- /main compiler option [Visual Basic]
- -main compiler option [Visual Basic]
ms.assetid: 83fc339d-6652-415d-b205-b5133319b5b0
ms.openlocfilehash: 91f2a27ed9b6fb296dbb9e50fc488fd012311890
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005508"
---
# <a name="-main"></a>-main
`Sub Main` プロシージャを格納するクラスまたはモジュールを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-main:location  
```  
  
## <a name="arguments"></a>引数  
 `location`  
 必須。 プログラムの開始時に呼び出される @no__t 0 のプロシージャを含むクラスまたはモジュールの名前。 この形式は、 **main: module**または **-main: namespace. module**の形式にすることができます。  
  
## <a name="remarks"></a>コメント  
 このオプションは、実行可能ファイルまたは Windows 実行可能プログラムを作成するときに使用します。 **-Main**オプションを省略した場合、コンパイラは、すべてのパブリッククラスとモジュールで有効な共有 `Sub Main` を検索します。  
  
 @No__t-1 プロシージャのさまざまな形式については、「 [Visual Basic の Main プロシージャ](../../../visual-basic/programming-guide/program-structure/main-procedure.md)」を参照してください。  
  
 @No__t-0 が <xref:System.Windows.Forms.Form> から継承するクラスである場合、コンパイラは、クラスに @no__t 3 プロシージャがない場合にアプリケーションを起動する既定の `Main` プロシージャを提供します。 これにより、開発環境で作成されたコマンドラインでコードをコンパイルできます。  
  
 [!code-vb[VbVbalrCompiler#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/Class1.vb#16)]  
  
### <a name="to-set--main-in-the-visual-studio-integrated-development-environment"></a>Visual Studio 統合開発環境で-main を設定するには  
  
1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。  
  
2. **[アプリケーション]** タブをクリックします。  
  
3. **[アプリケーションフレームワークを有効にする]** チェックボックスがオンになっていないことを確認します。  
  
4. **[スタートアップオブジェクト]** ボックスの値を変更します。  
  
## <a name="example"></a>例  
 次のコードでは `T2.vb` と `T3.vb` をコンパイルし、`Sub Main` プロシージャが `Test2` クラスで見つかることを指定します。  
  
```console
vbc t2.vb t3.vb -main:Test2  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Visual Basic の Main プロシージャ](../../../visual-basic/programming-guide/program-structure/main-procedure.md)
