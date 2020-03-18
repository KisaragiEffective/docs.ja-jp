---
title: -nostdlib
ms.date: 03/13/2018
helpviewer_keywords:
- nostdlib compiler option [Visual Basic]
- -nostdlib compiler option [Visual Basic]
- /nostdlib compiler option [Visual Basic]
ms.assetid: 140381b8-dc96-4ad5-ae11-792c9ed0be4d
ms.openlocfilehash: db6b047f521d8ef44d2bd1b70b654a4233ebb1a7
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347914"
---
# <a name="-nostdlib-visual-basic"></a>-nostdlib (Visual Basic)
コンパイラが標準ライブラリを自動的に参照しないようにします。  
  
## <a name="syntax"></a>構文  
  
```console  
-nostdlib  
```  
  
## <a name="remarks"></a>コメント  
 `-nostdlib` オプションを指定すると、System .dll アセンブリへの自動参照が削除され、コンパイラによって Vbc.exe ファイルが読み取られなくなります。 Vbc.exe ファイルと同じディレクトリにある Vbc.exe ファイルは、一般的に使用される .NET Framework アセンブリを参照し、`System` と `Microsoft.VisualBasic` 名前空間をインポートします。  
  
> [!NOTE]
> Mscorlib.dll および Microsoft の .dll アセンブリは常に参照されます。  
  
> [!NOTE]
> `-nostdlib` オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="example"></a>例  
 次のコードは、標準ライブラリを参照せずに `T2.vb` をコンパイルします。 `My` オブジェクトを削除するには、`_MYTYPE` 条件付きコンパイル定数を文字列 "Empty" に設定する必要があります。  
  
```console
vbc -nostdlib -define:_MYTYPE=\"Empty\" T2.vb  
```  
  
## <a name="see-also"></a>関連項目

- [-noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)
- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [My で利用可能なオブジェクトのカスタマイズ](../../../visual-basic/developing-apps/customizing-extending-my/customizing-which-objects-are-available-in-my.md)
