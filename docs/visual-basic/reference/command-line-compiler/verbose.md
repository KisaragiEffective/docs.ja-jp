---
title: -verbose
ms.date: 03/13/2018
helpviewer_keywords:
- verbose compiler option [Visual Basic]
- -verbose compiler option [Visual Basic]
- /verbose compiler option [Visual Basic]
ms.assetid: d1aec0c1-0261-421d-9adc-5b13756100be
ms.openlocfilehash: 8c5bc1d2ce331b8fe9461f91d64fbeab5a070b59
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004977"
---
# <a name="-verbose"></a>-verbose
コンパイラによって、詳細なステータスとエラーメッセージが生成されます。  
  
## <a name="syntax"></a>構文  
  
```console  
-verbose[+ | -]  
```  
  
## <a name="arguments"></a>引数  
 `+` &#124; `-`  
 省略可能です。 `-verbose` を指定すると `-verbose+` を指定した場合と同じになり、詳細なメッセージが生成されます。このオプションの既定値は `-verbose-` です。  
  
## <a name="remarks"></a>コメント  
 `-verbose` オプションを指定すると、コンパイラが生成したエラーの総数についての情報、モジュールが読み込んでいるアセンブリ、および現在コンパイルされているファイルが表示されます。
  
> [!NOTE]
> `-verbose` オプションは Visual Studio の開発環境内からは利用できません。このオプションを利用できるのは、コマンド ラインからコンパイルする場合のみです。  
  
## <a name="example"></a>例  
 次のコードは `In.vb` をコンパイルし、詳細な状態情報を表示するようにコンパイラに指示します。  
  
```console  
vbc -verbose in.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
