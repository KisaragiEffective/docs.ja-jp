---
title: -vbruntime
ms.date: 03/13/2018
f1_keywords:
- vbruntime
- -vbruntime
helpviewer_keywords:
- vbruntime compiler option [Visual Basic]
- -vbruntime compiler option [Visual Basic]
- /vbruntime compiler option [Visual Basic]
ms.assetid: 1aa0239e-511a-4c29-957d-fd72877b350a
ms.openlocfilehash: 8c7789c6af7b82ecb40ecd73d09f64aa1da3fd4b
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005060"
---
# <a name="-vbruntime"></a>-vbruntime
コンパイラが Visual Basic Runtime Library を参照せずにコンパイルするか、特定のランタイム ライブラリを参照してコンパイルするかを指定します。  
  
## <a name="syntax"></a>構文  
  
```console  
-vbruntime:{ - | + | * | path }  
```  
  
## <a name="arguments"></a>引数  
 \-  
 Visual Basic ランタイムライブラリへの参照を使用せずにコンパイルします。  
  
 \+  
 既定の Visual Basic ランタイムライブラリへの参照を使用してコンパイルします。  
  
 \*  
 Visual Basic ランタイムライブラリへの参照を使用せずにコンパイルし、Visual Basic ランタイムライブラリからアセンブリにコア機能を埋め込みます。  
  
 `path`  
 指定したライブラリ (DLL) への参照を使用してコンパイルします。  
  
## <a name="remarks"></a>コメント  
 `-vbruntime` コンパイラオプションを使用すると、コンパイラが Visual Basic ランタイムライブラリへの参照を使用せずにコンパイルするように指定できます。 Visual Basic ランタイムライブラリへの参照を使用せずにコンパイルする場合、エラーまたは警告は、Visual Basic ランタイムヘルパーの呼び出しを生成するコードまたは言語コンストラクトに記録されます。 ( *Visual Basic ランタイムヘルパー*は、特定の言語のセマンティックを実行するために実行時に呼び出される、Microsoft によって定義される関数です)。  
  
 `-vbruntime+` オプションは、`-vbruntime` スイッチが指定されていない場合に発生するのと同じ動作を生成します。 `-vbruntime+` オプションを使用して、前の `-vbruntime` スイッチを上書きできます。  
  
 `-vbruntime-` または `-vbruntime:path` オプションを使用する場合、`My` 型のほとんどのオブジェクトは使用できません。  
  
## <a name="embedding-visual-basic-runtime-core-functionality"></a>埋め込み Visual Basic ランタイムコア機能  
 `-vbruntime*` オプションを使用すると、ランタイムライブラリへの参照を使用せずにコンパイルできます。 代わりに、Visual Basic ランタイムライブラリのコア機能がユーザーアセンブリに埋め込まれます。 このオプションは、アプリケーションが Visual Basic ランタイムを含まないプラットフォームで実行されている場合に使用できます。  
  
 次のランタイムメンバーが埋め込まれています。  
  
- <xref:Microsoft.VisualBasic.CompilerServices.Conversions> クラス  
  
- <xref:Microsoft.VisualBasic.Strings.AscW%28System.Char%29> メソッド  
  
- <xref:Microsoft.VisualBasic.Strings.AscW%28System.String%29> メソッド  
  
- <xref:Microsoft.VisualBasic.Strings.ChrW%28System.Int32%29> メソッド  
  
- <xref:Microsoft.VisualBasic.Constants.vbBack> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbCr> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbCrLf> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbFormFeed> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbLf> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbNewLine> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbNullChar> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbNullString> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbTab> 定数  
  
- <xref:Microsoft.VisualBasic.Constants.vbVerticalTab> 定数  
  
- `My` 型の一部のオブジェクト  
  
 `-vbruntime*` オプションを使用してコンパイルし、コードがコア機能に埋め込まれていない Visual Basic ランタイムライブラリからメンバーを参照している場合、コンパイラは、メンバーが使用できないことを示すエラーを返します。  
  
## <a name="referencing-a-specified-library"></a>指定したライブラリの参照  
 `path` 引数を使用すると、既定の Visual Basic ランタイムライブラリの代わりに、カスタムランタイムライブラリへの参照を使用してコンパイルできます。  
  
 `path` 引数の値が DLL への完全修飾パスの場合、コンパイラはそのファイルをランタイムライブラリとして使用します。 `path` 引数の値が DLL への完全修飾パスではない場合、Visual Basic コンパイラは最初に現在のフォルダー内の特定の DLL を検索します。 次に、 [-sdkpath](../../../visual-basic/reference/command-line-compiler/sdkpath.md)コンパイラオプションを使用して、指定したパスを検索します。 `-sdkpath` コンパイラオプションが使用されていない場合、コンパイラは、識別された DLL を .NET Framework フォルダー (`%systemroot%\Microsoft.NET\Framework\versionNumber`) で検索します。  
  
## <a name="example"></a>例  
 次の例は、`-vbruntime` オプションを使用して、カスタムライブラリへの参照を使用してコンパイルする方法を示しています。  
  
```console
vbc -vbruntime:C:\VBLibraries\CustomVBLibrary.dll  
```  
  
## <a name="see-also"></a>関連項目

- [Visual Basic Core – Visual Studio 2010 SP1 の新しいコンパイルモード](https://devblogs.microsoft.com/vbteam/vb-core-new-compilation-mode-in-visual-studio-2010-sp1/)
- [Visual Basic コマンドラインコンパイラ](../../../visual-basic/reference/command-line-compiler/index.md)
- [コンパイルコマンドラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [-sdkpath](../../../visual-basic/reference/command-line-compiler/sdkpath.md)
