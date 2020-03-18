---
title: -optimize
ms.date: 07/20/2015
helpviewer_keywords:
- optimize compiler option [Visual Basic]
- /optimize compiler option [Visual Basic]
- optimization [Visual Basic], enabling
- -optimize compiler option [Visual Basic]
ms.assetid: fcba4a97-3622-4b87-a891-0f77deab4998
ms.openlocfilehash: e8daf4a49123623b6470bc3c6281869f1b9b3d0f
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005369"
---
# <a name="-optimize"></a>-optimize
コンパイラの最適化を有効または無効にします。  
  
## <a name="syntax"></a>構文  
  
```console  
-optimize[ + | - ]  
```  
  
## <a name="arguments"></a>引数  
  
|用語|Definition|  
|---|---|  
|`+` &#124; `-`|省略可。 `-optimize-` オプションは、コンパイラの最適化を無効にします。 `-optimize+` オプションを使用すると、最適化が有効になります。 既定では、最適化が無効になります。|  
  
## <a name="remarks"></a>コメント  
 コンパイラを最適化すると、出力ファイルのサイズが小さくなり、動作が速くなり、処理の効率が向上します。 ただし、最適化によって出力ファイルにコードが再配置されるため、`-optimize+` はデバッグが困難になることがあります。  
  
 アセンブリの `-target:module` で生成されるすべてのモジュールは、アセンブリと同じ `-optimize` 設定を使用する必要があります。 詳細については、「 [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)」を参照してください。  
  
 `-optimize` オプションと `-debug` オプションを組み合わせることができます。  
  
|Visual Studio 統合開発環境で-optimize を設定するには|  
|---|  
|1.**ソリューションエクスプローラー**でプロジェクトを選択します。 **[プロジェクト]** メニューの **[プロパティ]** をクリックします。<br />     <br />2. **[コンパイル]** タブをクリックします。<br />3. **[詳細設定]** ボタンをクリックします。<br />4. **[最適化を有効にする]** チェックボックスを変更します。|  
  
## <a name="example"></a>例  
 次のコードは `T2.vb` をコンパイルし、コンパイラの最適化を有効にします。  
  
```console
vbc t2.vb -optimize  
```  
  
## <a name="see-also"></a>参照

- [Visual Basic のコマンド ライン コンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [-debug (Visual Basic)](../../../visual-basic/reference/command-line-compiler/debug.md)
- [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
