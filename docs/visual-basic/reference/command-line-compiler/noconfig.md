---
title: -noconfig
ms.date: 03/13/2018
helpviewer_keywords:
- noconfig compiler option [Visual Basic]
- -noconfig compiler option [Visual Basic]
- /noconfig compiler option [Visual Basic]
ms.assetid: a7405067-bd21-4171-adf4-a126fa3ad6c3
ms.openlocfilehash: c57ed1699d110959e9faf3dc3d43bcc200851c1c
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005436"
---
# <a name="-noconfig"></a>-noconfig
コンパイラが、一般的に使用される .NET Framework アセンブリを自動的に参照したり、`System` と `Microsoft.VisualBasic` の名前空間をインポートしたりしないように指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-noconfig  
```  
  
## <a name="remarks"></a>コメント  
 @No__t-0 オプションは、Vbc.exe ファイルと同じディレクトリにある Vbc.exe ファイルでコンパイルしないようにコンパイラに指示します。 Vbc.exe ファイルは、一般的に使用される .NET Framework アセンブリを参照し、@no__t 0 と `Microsoft.VisualBasic` の名前空間をインポートします。 コンパイラは、`-nostdlib` オプションが指定されていない限り、システム .dll アセンブリを暗黙的に参照します。 @No__t-0 オプションは、Vbc.exe を使用してコンパイルしないようにコンパイラに指示します。または、自動的に .dll アセンブリを参照します。  
  
> [!NOTE]
> Mscorlib.dll および Microsoft の .dll アセンブリは常に参照されます。  
  
 Vbc.exe ファイルを変更して、すべての Vbc.exe コンパイルに含める必要のある追加のコンパイラオプションを指定することができます (`-noconfig` オプションを指定する場合を除く)。 詳しくは、「[@ (応答ファイルの指定)](../../../visual-basic/reference/command-line-compiler/specify-response-file.md)」をご覧ください。  
  
 コンパイラは、最後に `vbc` コマンドに渡されたオプションを処理します。 したがって、コマンドラインのオプションは、Vbc.exe ファイルの同じオプションの設定よりも優先されます。  
  
> [!NOTE]
> @No__t-0 オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="see-also"></a>関連項目

- [-nostdlib (Visual Basic)](../../../visual-basic/reference/command-line-compiler/nostdlib.md)
- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [@ (応答ファイルの指定)](../../../visual-basic/reference/command-line-compiler/specify-response-file.md)
- [-reference (Visual Basic)](../../../visual-basic/reference/command-line-compiler/reference.md)
