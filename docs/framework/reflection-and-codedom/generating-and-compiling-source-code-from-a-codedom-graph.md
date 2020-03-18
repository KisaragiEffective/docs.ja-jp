---
title: CodeDOM グラフからのソース コードの生成およびコンパイル
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- code compilers
- CodeDOM, generating source code
- Code Document Object Model, graphs
- templated code generation
- source code, generating
- dynamically representing source code
- generating CodeDOM graphs
- Code Document Object Model, generating source code
- translating language to language
- compiling assemblies
- generating source code in multiple languages
- graphing with CodeDOM
- dynamic compilation
- assemblies [.NET Framework], CodeDOM
- source code generation
- outputting source code by CodeDOM
- code generators
- compiling source code, multiple languages
- CodeDOM, graphs
ms.assetid: 6c864c8e-6dd3-4a65-ace0-36879d9a9c42
ms.openlocfilehash: a8d3bf7363cb887834a1c251aead05c75e2e3fe8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130230"
---
# <a name="generating-and-compiling-source-code-from-a-codedom-graph"></a>CodeDOM グラフからのソース コードの生成およびコンパイル
<xref:System.CodeDom.Compiler> 名前空間は、CodeDOM オブジェクト グラフからソース コードを生成し、サポートされているコンパイラでコンパイルを管理するためのインターフェイスを提供します。 コード プロバイダーは、CodeDOM グラフに基づいて、特定のプログラミング言語でソース コードを生成できます。 <xref:System.CodeDom.Compiler.CodeDomProvider> から派生したクラスは、通常、プロバイダーが対応している言語のコードを生成し、コンパイルするためのメソッドを提供します。  
  
## <a name="using-a-codedom-code-provider-to-generate-source-code"></a>CodeDOM コード プロバイダーを使用してソース コードを生成する  
 特定の言語でソース コードを生成するには、生成するソース コードの構造を表す CodeDOM グラフが必要です。  
  
 <xref:Microsoft.CSharp.CSharpCodeProvider> のインスタンスを作成する方法の例を次に示します。  
  
 [!code-cpp[CodeDomExample#21](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source3.cpp#21)]
 [!code-csharp[CodeDomExample#21](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source3.cs#21)]
 [!code-vb[CodeDomExample#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source3.vb#21)]  
  
 コード生成のグラフは通常、<xref:System.CodeDom.CodeCompileUnit> に含まれます。 CodeDOM グラフが含まれる **CodeCompileUnit** のコードを生成するには、コード プロバイダーの <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> メソッドを呼び出します。 このメソッドには <xref:System.IO.TextWriter> のパラメーターがあり、それがソース コードの生成に利用されます。そのため、場合により、書き込みに使用する **TextWriter** を最初に作成する必要があります。 次の例では、**CodeCompileUnit** からコードを生成し、生成したソース コードを HelloWorld.cs という名前のファイルに書き込んでいます。  
  
 [!code-cpp[CodeDomExample#22](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source3.cpp#22)]
 [!code-csharp[CodeDomExample#22](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source3.cs#22)]
 [!code-vb[CodeDomExample#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source3.vb#22)]  
  
## <a name="using-a-codedom-code-provider-to-compile-assemblies"></a>CodeDOM コード プロバイダーを使用してアセンブリをコンパイルする  
 **コンパイルを呼び出す**  
  
 CodeDom プロバイダーを利用してアセンブリをコンパイルするには、コンパイラが理解できる言語でコンパイルするソース コードか、コンパイルするソース コードの生成元になる CodeDOM グラフを用意する必要があります。  
  
 CodeDOM グラフからコンパイルする場合、コード プロバイダーの <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromDom%2A> メソッドにグラフを含む <xref:System.CodeDom.CodeCompileUnit> を渡します。 コンパイラが理解できる言語のソース コード ファイルがある場合、CodeDOM プロバイダーの <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromFile%2A> メソッドにソース コードを含むファイルの名前を渡します。 コンパイラが理解できる言語のソース コードを含む文字列を CodeDOM プロバイダーの <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromSource%2A> メソッドに渡すこともできます。  
  
 **コンパイル パラメーターの構成**  
  
 CodeDOM プロバイダーの標準的なコンパイル呼び出しメソッドにはすべて、型 <xref:System.CodeDom.Compiler.CompilerParameters> のパラメーターがあります。これはコンパイルに使用するオプションを示します。  
  
 **CompilerParameters** の <xref:System.CodeDom.Compiler.CompilerParameters.OutputAssembly%2A> プロパティに出力アセンブリのファイル名を指定できます。 指定しない場合、既定の出力ファイル名が使用されます。  
  
 既定では、新規の **CompilerParameters** は、<xref:System.CodeDom.Compiler.CompilerParameters.GenerateExecutable%2A> プロパティを **false** に設定して初期化されます。 実行可能ファイル プログラムをコンパイルする場合、**GenerateExecutable** プロパティを **true** に設定する必要があります。 **GenerateExecutable** を **false** に設定すると、コンパイラはクラス ライブラリを生成します。  
  
 CodeDOM グラフから実行可能ファイルをコンパイルする場合、グラフに <xref:System.CodeDom.CodeEntryPointMethod> を定義する必要があります。 コードのエントリ ポイントが複数存在するとき、場合によっては、使用するエントリ ポイントを定義するクラスの名前に **CompilerParameters** の <xref:System.CodeDom.Compiler.CompilerParameters.MainClass%2A> プロパティを設定する必要があります。  
  
 生成された実行可能ファイルにデバッグ情報を追加するには、<xref:System.CodeDom.Compiler.CompilerParameters.IncludeDebugInformation%2A> プロパティを **true** に設定します。  
  
 プロジェクトがアセンブリを参照する場合、コンパイルを呼び出すときに使用する **CompilerParameters** の <xref:System.CodeDom.Compiler.CompilerParameters.ReferencedAssemblies%2A> プロパティとして、<xref:System.Collections.Specialized.StringCollection> の項目としてのアセンブリ名を指定する必要があります。  
  
 <xref:System.CodeDom.Compiler.CompilerParameters.GenerateInMemory%2A> プロパティを **true** に設定することで、ディスクではなくメモリに書き込まれるアセンブリをコンパイルできます。 アセンブリをメモリに生成すると、<xref:System.CodeDom.Compiler.CompilerResults> の <xref:System.CodeDom.Compiler.CompilerResults.CompiledAssembly%2A> プロパティから生成されたアセンブリの参照がコードに与えられることがあります。 アセンブリをディスクに書き込む場合、**CompilerResults** の <xref:System.CodeDom.Compiler.CompilerResults.PathToAssembly%2A> プロパティから生成されたアセンブリのパスを取得できます。  
  
 コンパイル プロセスを呼び出すときに使用するカスタムのコマンドライン引数を指定するには、<xref:System.CodeDom.Compiler.CompilerParameters.CompilerOptions%2A> プロパティに文字列を設定します。  
  
 コンパイラ プロセスの呼び出しに Win32 セキュリティ トークンが必要な場合、<xref:System.CodeDom.Compiler.CompilerParameters.UserToken%2A> プロパティにトークンを指定します。  
  
 コンパイルされたアセンブリに Win32 リソース ファイルをリンクするには、<xref:System.CodeDom.Compiler.CompilerParameters.Win32Resource%2A> プロパティに Win32 リソース ファイルの名前を指定します。  
  
 コンパイルを中止する警告レベルを指定するには、コンパイルを中止する警告レベルを表す整数に <xref:System.CodeDom.Compiler.CompilerParameters.WarningLevel%2A> プロパティを設定します。 <xref:System.CodeDom.Compiler.CompilerParameters.TreatWarningsAsErrors%2A> プロパティを **true** に設定すると、警告が出た場合にコンパイルを中止するようにコンパイラを設定することもできます。  
  
 次のコード サンプルでは、<xref:System.CodeDom.Compiler.CodeDomProvider> クラスから派生した CodeDOM プロバイダーを利用し、ソース ファイルをコンパイルしています。  
  
 [!code-cpp[CodeDomExample#23](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source3.cpp#23)]
 [!code-csharp[CodeDomExample#23](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source3.cs#23)]
 [!code-vb[CodeDomExample#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source3.vb#23)]  
  
## <a name="languages-with-initial-support"></a>初期サポートの言語  
 .NET Framework は、C#、Visual Basic、C++、および JScript のコード コンパイラとコード ジェネレーターを提供します。 CodeDOM のサポートは、言語固有のコード ジェネレーターとコード コンパイラを実装することで、他の言語に拡張できます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.CodeDom>
- <xref:System.CodeDom.Compiler>
- [動的なソース コードの生成とコンパイル](dynamic-source-code-generation-and-compilation.md)
- [CodeDOM クイック リファレンス](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100))
