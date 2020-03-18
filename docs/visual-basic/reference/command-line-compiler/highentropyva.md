---
title: -highentropyva
ms.date: 03/10/2018
helpviewer_keywords:
- highentropyva compiler option (Visual Basic)
- /highentropyva compiler option (Visual Basic)
ms.assetid: ff25f20a-6ca2-467b-9e52-5cf439f5028e
ms.openlocfilehash: 7934dcaada4675bf687624bef5ed1ea25e842832
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344251"
---
# <a name="-highentropyva-visual-basic"></a>-highentropyva (Visual Basic)
64ビットの実行可能ファイルまたは[-platform: anycpu](../../../visual-basic/reference/command-line-compiler/platform.md)コンパイラオプションによってマークされた実行可能ファイルが、高エントロピアドレス空間レイアウトのランダム化 (ASLR) をサポートするかどうかを示します。  
  
## <a name="syntax"></a>構文  
  
```console  
-highentropyva[+ | -]  
```  
  
## <a name="arguments"></a>引数  
 `+` &#124; `-`  
 省略可。 このオプションは、既定ではオフになっています。また、`-highentropyva-`を指定した場合はです。 `-highentropyva` または `-highentropyva+`を指定した場合、オプションは on になります。  
  
## <a name="remarks"></a>コメント  
 このオプションを指定すると、カーネルが ASLR の一部としてプロセスのアドレス空間レイアウトをランダムに処理したときに、互換性のあるバージョンの Windows カーネルでエントロピを使用できるようになります。 カーネルがより高いエントロピを使用する場合は、スタックやヒープなどのメモリ領域により多くのアドレスを割り当てることができます。 これによって特定のメモリ領域の位置を推測しづらくなる効果が得られます。  
  
 このオプションが on に設定されている場合、ターゲットの実行可能ファイルとそれが依存するモジュールは、これらのモジュールが64ビットプロセスとして実行されている場合、4ギガバイト (GB) を超えるポインター値を処理できる必要があります。  
  
## <a name="see-also"></a>参照

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
