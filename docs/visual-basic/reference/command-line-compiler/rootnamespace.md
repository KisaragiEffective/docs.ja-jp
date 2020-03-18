---
title: -rootnamespace
ms.date: 03/13/2018
f1_keywords:
- /rootnamespace
- rootnamespace
helpviewer_keywords:
- /rootnamespace compiler option [Visual Basic]
- -rootnamespace compiler option [Visual Basic]
- rootnamespace compiler option [Visual Basic]
ms.assetid: e9245edf-6bef-420d-a7c7-324117752783
ms.openlocfilehash: 4df4e74fc13c922f51f5b74c3c152bdea28b4431
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005208"
---
# <a name="-rootnamespace"></a>-rootnamespace
すべての型宣言に対して名前空間を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-rootnamespace:namespace  
```  
  
## <a name="arguments"></a>引数  
  
|項目|定義|  
|---|---|  
|`namespace`|現在のプロジェクトのすべての型宣言を囲む名前空間の名前。|  
  
## <a name="remarks"></a>コメント  
 Visual studio の実行可能ファイル (Devenv.exe) を使用して、Visual Studio 統合開発環境で作成されたプロジェクトをコンパイルする場合は、`-rootnamespace` を使用して、<xref:VSLangProj80.VBProjectProperties3.RootNamespace%2A> プロパティの値を指定します。 詳細については、「[Devenv コマンドラインスイッチ](/visualstudio/ide/reference/devenv-command-line-switches)」を参照してください。  
  
 共通言語ランタイムの MSIL 逆アセンブラー (`Ildasm.exe`) を使用して、出力ファイル内の名前空間の名前を表示します。  
  
|Visual Studio 統合開発環境で rootnamespace を設定するには|  
|---|  
|1. **ソリューション エクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。 <br />2. **[アプリケーション]** タブをクリックします。<br />3. **[ルート名前空間]** ボックスの値を変更します。|  
  
## <a name="example"></a>例  
 次のコードでは `In.vb` をコンパイルし、名前空間 `mynamespace` のすべての型宣言を囲みます。  
  
```console
vbc -rootnamespace:mynamespace in.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [Ildasm.exe (IL 逆アセンブラー)](../../../framework/tools/ildasm-exe-il-disassembler.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
