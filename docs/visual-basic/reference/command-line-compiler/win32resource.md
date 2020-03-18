---
title: -win32resource
ms.date: 03/13/2018
f1_keywords:
- -win32resource
- win32resource
helpviewer_keywords:
- /win32resource compiler option [Visual Basic]
- -win32resource compiler option [Visual Basic]
- win32resource compiler option [Visual Basic]
ms.assetid: e226946d-19ce-4cc9-91f5-aed24f77aa2b
ms.openlocfilehash: cee06adec89aac4b3e3f170df3bf932e466f3070
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004963"
---
# <a name="-win32resource"></a>-win32resource
Win32 リソースファイルを出力ファイルに挿入します。  
  
## <a name="syntax"></a>構文  
  
```console  
-win32resource:filename  
```  
  
## <a name="arguments"></a>引数  
 `filename`  
 出力ファイルに追加するリソースファイルの名前。 ファイル名にスペースが含まれている場合は、ファイル名を引用符 ("") で囲みます。  
  
## <a name="remarks"></a>コメント  
 Win32 リソースファイルは、Microsoft Windows リソースコンパイラ (RC) を使用して作成できます。  
  
 Win32 リソースには、**ファイルエクスプローラー**でアプリケーションを識別するのに役立つバージョンまたはビットマップ (アイコン) 情報を含めることができます。 '-win32resource' を指定しない場合、コンパイラはアセンブリのバージョンに基づいてバージョン情報を生成します。 '-win32resource' および `-win32icon` オプションは相互に排他的です。  
  
 .NET Framework リソースファイルを参照する場合は[-linkresource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/linkresource.md)を参照し、.NET Framework リソースファイルをアタッチする場合は[-resource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/resource.md)を参照してください。  
  
> [!NOTE]
> '-win32resource' オプションは、Visual Studio 開発環境内からは使用できません。これは、コマンドラインからコンパイルする場合にのみ使用できます。  
  
## <a name="example"></a>例  
 次のコードは `In.vb` をコンパイルし、Win32 リソースファイルをアタッチします。 `Rf.res` となります。  
  
```console  
vbc -win32resource:rf.res in.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
