---
title: '方法: CodeDOM を使用して XML ドキュメント ファイルを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CodeDOM, generating XML documentation
- XML documentation, creating using CodeDOM
- Code Document Object Model, generating XML documentation
ms.assetid: e3b80484-36b9-41dd-9d21-a2f9a36381dc
ms.openlocfilehash: cdd1f173274b6bd33c4a67ed8eb0974c4c8e8e70
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130191"
---
# <a name="how-to-create-an-xml-documentation-file-using-codedom"></a>方法: CodeDOM を使用して XML ドキュメント ファイルを作成する
CodeDOM を利用し、XML ドキュメントを生成するコードを作成できます。 XML ドキュメント コメントを含む CodeDOM グラフを作成する、コードを生成する、XML 文書出力を作成するコンパイラ オプションでその生成されたコードをコンパイルするというプロセスが行われます。  
  
### <a name="to-create-a-codedom-graph-that-contains-xml-documentation-comments"></a>XML ドキュメント コメントを含む CodeDOM グラフを作成するには  
  
1. サンプル アプリケーションの CodeDOM グラフを含む <xref:System.CodeDom.CodeCompileUnit> を作成します。  
  
2. <xref:System.CodeDom.CodeCommentStatement.%23ctor%2A> コンストラクターを使用します。このとき、`docComment` パラメーターを `true` に設定し、XML ドキュメント コメントの要素とテキストを作成します。  
  
     [!code-csharp[CodeDomHelloWorldSample#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomHelloWorldSample/cs/program.cs#4)]
     [!code-vb[CodeDomHelloWorldSample#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomHelloWorldSample/vb/program.vb#4)]  
  
### <a name="to-generate-the-code-from-the-codecompileunit"></a>CodeCompileUnit からコードを生成するには  
  
1. <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> メソッドを使用し、コードを生成し、コンパイルするソース ファイルを作成します。  
  
     [!code-csharp[CodeDomHelloWorldSample#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomHelloWorldSample/cs/program.cs#5)]
     [!code-vb[CodeDomHelloWorldSample#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomHelloWorldSample/vb/program.vb#5)]  
  
### <a name="to-compile-the-code-and-generate-the-documentation-file"></a>コードをコンパイルし、ドキュメント ファイルを生成するには  
  
1. <xref:System.CodeDom.Compiler.CompilerParameters> オブジェクトの <xref:System.CodeDom.Compiler.CompilerParameters.CompilerOptions%2A> プロパティに **/doc** コンパイラ オプションを追加し、オブジェクトを <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromFile%2A> メソッドに渡し、コードのコンパイル時に XML ドキュメント ファイルを作成します。  
  
     [!code-csharp[CodeDomHelloWorldSample#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomHelloWorldSample/cs/program.cs#6)]
     [!code-vb[CodeDomHelloWorldSample#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomHelloWorldSample/vb/program.vb#6)]  
  
## <a name="example"></a>例  
 次のコード例では、ドキュメント コメントのある CodeDOM グラフを作成し、グラフからコード ファイルを生成し、ファイルをコンパイルし、関連 XML ドキュメント ファイルを作成します。  
  
 [!code-csharp[CodeDomHelloWorldSample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomHelloWorldSample/cs/program.cs#1)]
 [!code-vb[CodeDomHelloWorldSample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomHelloWorldSample/vb/program.vb#1)]  
  
 このコード例では、HelloWorldDoc.xml ファイルで次の XML ドキュメントが作成されます。  
  
```xml  
<?xml version="1.0" ?>   
<doc>  
  <assembly>  
    <name>HelloWorld</name>   
  </assembly>  
  <members>  
    <member name="T:Samples.Class1">  
      <summary>  
        Create a Hello World application.   
        <seealso cref="M:Samples.Class1.Main" />   
      </summary>  
    </member>  
    <member name="M:Samples.Class1.Main">  
      <summary>  
        Main method for HelloWorld application.   
        <para>Add a new paragraph to the description.</para>   
      </summary>  
    </member>  
  </members>  
</doc>  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- このコード サンプルを正しく実行するには、`FullTrust` アクセス許可を設定する必要があります。  
  
## <a name="see-also"></a>関連項目

- [XML の使用によるコードのドキュメントの作成](../../visual-basic/programming-guide/program-structure/documenting-your-code-with-xml.md)
- [XML ドキュメント コメント](../../csharp/programming-guide/xmldoc/index.md)
- [XML に関するドキュメント](/cpp/ide/xml-documentation-visual-cpp)
