---
title: Winmdexp.exe (Windows ランタイム メタデータのエクスポート ツール)
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Runtime Metadata Export Tool
- Winmdexp.exe
ms.assetid: d2ce0683-343d-403e-bb8d-209186f7a19d
ms.openlocfilehash: 52820b78f6ed7b02e80df66f90a01143b31d9b29
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74447280"
---
# <a name="winmdexpexe-windows-runtime-metadata-export-tool"></a>Winmdexp.exe (Windows ランタイム メタデータのエクスポート ツール)
Windows ランタイム メタデータ エクスポート ツール (Winmdexp.exe) は、.NET Framework モジュールを、Windows メタデータを含むファイルに変換します。 .NET Framework アセンブリと Windows ランタイム メタデータ ファイルでは同じ物理形式が使用されますが、メタデータ テーブルの内容に違いがあります。つまり、NET Framework アセンブリは自動的に Windows ランタイム コンポーネントとして使用できるわけではありません。 .NET Framework モジュールを Windows ランタイム コンポーネントに変換するプロセスは、"*エクスポート*" と呼ばれます。 .NET Framework 4.5 と .NET Framework 4.5.1 では、結果として生成される Windows メタデータ (.winmd) ファイルにメタデータと実装の両方が含まれます。  
  
 Visual Studio 2013 または Visual Studio 2012 で、C# および Visual Basic の **Windows ストア**にある **Windows ランタイム コンポーネント** テンプレートを使用する場合、コンパイラのターゲットは .winmdobj ファイルであり、後続のビルド ステップで Winmdexp.exe が呼び出され、.winmdobj ファイルが .winmd ファイルにエクスポートされます。 Windows ランタイム コンポーネントをビルドする場合は、この方法をお勧めします。 Visual Studio による制御より細かくビルド プロセスを制御する場合は、Winmdexp.exe ファイルを直接使用します。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、Visual Studio 用開発者コマンド プロンプト (または Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[コマンド プロンプト](developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```console  
winmdexp [options] winmdmodule  
```  
  
## <a name="parameters"></a>パラメーター  
  
|引数またはオプション|[説明]|  
|------------------------|-----------------|  
|`winmdmodule`|エクスポートするモジュール (.winmdobj) を指定します。 指定できるのは 1 つのモジュールのみです。 このモジュールを作成するには、`/target` ターゲットと共に `winmdobj` コンパイラ オプションを使用します。 「[-target:winmdobj (C# コンパイラ オプション)](../../csharp/language-reference/compiler-options/target-winmdobj-compiler-option.md)」または「[-target (Visual Basic)](../../visual-basic/reference/command-line-compiler/target.md)」を参照してください。|  
|`/docfile:` `docfile`<br /><br /> `/d:` `docfile`|Winmdexp.exe が生成する出力 XML ドキュメント ファイルを指定します。 .NET Framework 4.5 では、出力ファイルは基本的に入力 XML ドキュメント ファイルと同じです。|  
|`/moduledoc:` `docfile`<br /><br /> `/md:` `docfile`|コンパイラが `winmdmodule` と共に生成した XML ドキュメント ファイルの名前を指定します。|  
|`/modulepdb:` `symbolfile`<br /><br /> `/mp:` `symbolfile`|`winmdmodule` のシンボルを含むプログラム データベース (PDB) ファイルの名前を指定します。|  
|`/nowarn:` `warning`|指定した警告番号が使用されなくなります。 *warning* の場合は、エラー コードの数字部分だけを先行ゼロなしで指定してください。|  
|`/out:` `file`<br /><br /> `/o:` `file`|出力 Windows メタデータ (.winmd) ファイルの名前を指定します。|  
|`/pdb:` `symbolfile`<br /><br /> `/p:` `symbolfile`|エクスポートされた Windows メタデータ (.winmd) ファイルのシンボルを含む出力プログラム データベース (PDB) ファイルの名前を指定します。|  
|`/reference:` `winmd`<br /><br /> `/r:` `winmd`|エクスポート時に参照するメタデータ ファイル (.winmd またはアセンブリ) を指定します。 「\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5」(32 ビット コンピューターでは「\Program Files\\...」) にある参照アセンブリを使用する場合は、System.Runtime.dll と mscorlib.dll の両方への参照を含めます。|  
|`/utf8output`|UTF-8 エンコードでメッセージが出力される必要があることを指定します。|  
|`/warnaserror+`|すべての警告をエラーとして扱うよう指定しています。|  
|**@** `responsefile`|オプション (および、必要に応じて `winmdmodule`) を含む応答 (.rsp) ファイルを指定します。 `responsefile` の各行に 1 つの引数またはオプションが含まれている必要があります。|  
  
## <a name="remarks"></a>解説  
 Winmdexp.exe は、任意の .NET Framework アセンブリを .winmd ファイルに変換するようには設計されていません。 `/target:winmdobj` オプションでコンパイルされるモジュールが必要で、追加の制限が適用されます。 この中で最も重要な制限は、アセンブリの API サーフェスで公開されるすべての型は必ず Windows ランタイム型であるということです。 詳細については、「[C# および Visual Basic での Windows ランタイム コンポーネントの作成](https://docs.microsoft.com/previous-versions/br230301(v=vs.110))」という記事の「Windows ランタイム コンポーネントの宣言型」セクションを参照してください。
  
 C# または Visual Basic で Windows 8.x Store アプリまたは Windows ランタイム コンポーネントを作成する場合は、Windows ランタイムでのプログラミングをより自然にするためのサポートが .NET Framework で提供されています。 これは、記事「[Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)」で解説されています。 プロセスでは、一般的に使用される Windows ランタイム型が .NET Framework 型にマップされます。 Winmdexp.exe では、このプロセスが反転され、対応する Windows ランタイム型を使用する API サーフェスが生成されます。 たとえば、<xref:System.Collections.Generic.IList%601> インターフェイスから構築された型は、Windows ランタイムの <xref:Windows.Foundation.Collections.IVector%601> インターフェイスから構築された型にマップされます。  
  
## <a name="see-also"></a>参照

- [Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)
- [C# および Visual Basic での Windows ランタイム コンポーネントの作成](https://docs.microsoft.com/previous-versions/br230301(v=vs.110))
- [Winmdexp.exe のエラー メッセージ](winmdexp-exe-error-messages.md)
- [ビルド ツール、配置ツール、および構成ツール (.NET Framework)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd233108(v=vs.100))
