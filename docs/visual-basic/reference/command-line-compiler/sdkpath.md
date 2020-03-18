---
title: -sdkpath
ms.date: 07/20/2015
f1_keywords:
- sdkpath
- -sdkpath
helpviewer_keywords:
- -sdkpath compiler option [Visual Basic]
- /sdkpath compiler option [Visual Basic]
- sdkpath compiler option [Visual Basic]
ms.assetid: fec8a3f1-b791-4a37-8af7-344859f8212d
ms.openlocfilehash: 46cec7ac3cb78c4fc97e299535f9085eff6daeff
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72004693"
---
# <a name="-sdkpath"></a>-sdkpath
Mscorlib.dll と Microsoft.VisualBasic.dll の場所を指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-sdkpath:path  
```  
  
## <a name="arguments"></a>引数  
 `path`  
 Mscorlib.dll および Microsoft.VisualBasic.dll のコンパイルに使用するバージョンを格納するディレクトリ。 このパスは、読み込まれるまで検査されません。 ディレクトリ名に空白が含まれている場合は、文字列を二重引用符 (" ") で囲みます。  
  
## <a name="remarks"></a>コメント  
 このオプションは、Visual Basic コンパイラに対して、既定以外の場所から mscorlib.dll ファイルと Microsoft ファイルの .dll ファイルを読み込むように指示します。 @No__t-0 オプションは、 [-netcf](../../../visual-basic/reference/command-line-compiler/netcf.md)と共に使用するように設計されています。 .NET Compact Framework では、これらのサポートライブラリのさまざまなバージョンを使用して、デバイスでは検出されない型と言語機能を使用しないようにしています。  
  
> [!NOTE]
> `-sdkpath` オプションは Visual Studio の開発環境内からは利用できません。このオプションを利用できるのは、コマンド ラインからコンパイルする場合のみです。 `-sdkpath` オプションは Visual Basic デバイス プロジェクトがロードされている場合に設定されます。  
  
 @No__t-0 コンパイラオプションを使用して、コンパイラが Visual Basic ランタイムライブラリへの参照を使用せずにコンパイルするように指定できます。 詳細については、「 [-vbruntime](../../../visual-basic/reference/command-line-compiler/vbruntime.md)」を参照してください。  
  
## <a name="example"></a>例  
 次のコードでは、C ドライブ上の .NET Compact Framework の既定のインストール ディレクトリにある Mscorlib.dll のバージョンとMicrosoft.VisualBasic.dll のバージョンを使用して、.NET Compact Framework で `Myfile.vb` をコンパイルします。 通常、最新のバージョンの .NET Compact Framework を使用します。  
  
```console
vbc -netcf -sdkpath:"c:\Program Files\Microsoft Visual Studio .NET 2003\CompactFrameworkSDK\v1.0.5000\Windows CE " myfile.vb  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [-netcf](../../../visual-basic/reference/command-line-compiler/netcf.md)
- [-vbruntime](../../../visual-basic/reference/command-line-compiler/vbruntime.md)
