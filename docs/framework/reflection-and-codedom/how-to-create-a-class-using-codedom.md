---
title: '方法 : CodeDOM を使用してクラスを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Code Document Object Model, graphs
- Code Document Object Model, creating classes
- graphing with CodeDOM
- CodeDOM, creating classes
- CodeDOM, graphs
ms.assetid: 0ceb70fe-36e1-49bb-922b-e9f615c20a14
ms.openlocfilehash: ff7c9d1593c8e75f9bcaeda6577c7cb941719749
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130200"
---
# <a name="how-to-create-a-class-using-codedom"></a>方法 : CodeDOM を使用してクラスを作成する
2 つのフィールド、3 つのプロパティ、1 つのメソッド、1 つのコンストラクター、1 つのエントリ ポイントを含むクラスを生成する CodeDOM グラフを作成し、コンパイルする方法を次に示します。  
  
1. CodeDOM コードを使用するコンソール アプリケーションを作成し、クラスのソース コードを生成します。  
  
     この例では、生成元のクラスの名前が `Sample` で、生成されるコードが SampleCode という名前のファイルの `CodeDOMCreatedClass` という名前のクラスになります。  
  
2. 生成元のクラスでは、CodeDOM グラフを初期化し、CodeDOM メソッドを利用して生成されるクラスのメンバー、コンストラクター、エントリ ポイント (`Main` メソッド) を定義します。  
  
     この例では、生成されるクラスに 2 つのフィールド、3 つのプロパティ、1 つのコンストラクター、1 つのメソッド、1 つの `Main` メソッドがあります。  
  
3. 生成元のクラスで、言語固有のコード プロバイダーを作成し、その <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> メソッドを呼び出してグラフからコードを生成します。  
  
4. アプリケーションをコンパイルして実行し、コードを生成します。  
  
     この例では、生成されたコードが SampleCode という名前のファイルに入っています。 そのコードをコンパイルして実行し、サンプル出力を確認します。  
  
### <a name="to-create-the-application-that-will-execute-the-codedom-code"></a>CodeDOM コードを実行するアプリケーションを作成するには  
  
- CodeDOM コードを含むコンソール アプリケーション クラスを作成します。 このクラスでアセンブリ (<xref:System.CodeDom.CodeCompileUnit>) とクラス (<xref:System.CodeDom.CodeTypeDeclaration>) を参照するためのグローバル フィールドを定義し、生成されるソース ファイルの名前を指定し、`Main` メソッドを宣言します。  
  
     [!code-csharp[CodeDOM Class Sample Main#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample Main/CS/program.cs#1)]
     [!code-vb[CodeDOM Class Sample Main#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample Main/VB/program.vb#1)]  
  
### <a name="to-initialize-the-codedom-graph"></a>CodeDOM グラフを初期化するには  
  
- コンソール アプリケーション クラスのコンストラクターで、アセンブリとクラスを初期化し、適切な宣言を CodeDOM グラフに追加します。  
  
     [!code-csharp[CodeDOM Class Sample#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#2)]
     [!code-vb[CodeDOM Class Sample#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#2)]  
  
### <a name="to-add-members-to-the-codedom-graph"></a>CodeDOM グラフにメンバーを追加するには  
  
- クラスの <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> プロパティに <xref:System.CodeDom.CodeMemberField> オブジェクトを追加することで CodeDOM グラフにフィールドを追加します。  
  
     [!code-csharp[CodeDOM Class Sample#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#3)]
     [!code-vb[CodeDOM Class Sample#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#3)]  
  
- クラスの <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> プロパティに <xref:System.CodeDom.CodeMemberProperty> オブジェクトを追加することで CodeDOM グラフにプロパティを追加します。  
  
     [!code-csharp[CodeDOM Class Sample#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#4)]
     [!code-vb[CodeDOM Class Sample#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#4)]  
  
- クラスの <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> プロパティに <xref:System.CodeDom.CodeMemberMethod> オブジェクトを追加することで CodeDOM グラフにメソッドを追加します。  
  
     [!code-csharp[CodeDOM Class Sample#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#5)]
     [!code-vb[CodeDOM Class Sample#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#5)]  
  
- クラスの <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> プロパティに <xref:System.CodeDom.CodeConstructor> オブジェクトを追加することで CodeDOM グラフにコンストラクターを追加します。  
  
     [!code-csharp[CodeDOM Class Sample#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#6)]
     [!code-vb[CodeDOM Class Sample#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#6)]  
  
- クラスの <xref:System.CodeDom.CodeTypeDeclaration.Members%2A> プロパティに <xref:System.CodeDom.CodeEntryPointMethod> オブジェクトを追加することで CodeDOM グラフにエントリ ポイントを追加します。  
  
     [!code-csharp[CodeDOM Class Sample#7](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#7)]
     [!code-vb[CodeDOM Class Sample#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#7)]  
  
### <a name="to-generate-the-code-from-the-codedom-graph"></a>CodeDOM グラフからコードを生成するには  
  
- <xref:System.CodeDom.Compiler.CodeDomProvider.GenerateCodeFromCompileUnit%2A> メソッドを呼び出すことで CodeDOM グラフからソース コードを生成します。  
  
     [!code-csharp[CodeDOM Class Sample#8](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#8)]
     [!code-vb[CodeDOM Class Sample#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#8)]  
  
### <a name="to-create-the-graph-and-generate-the-code"></a>グラフを作成し、コードを生成するには  
  
1. 前の手順で作成したメソッドを最初の手順で定義した `Main` メソッドに追加します。  
  
     [!code-csharp[CodeDOM Class Sample#9](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#9)]
     [!code-vb[CodeDOM Class Sample#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#9)]  
  
2. 生成元のクラスをコンパイルし、実行します。  
  
## <a name="example"></a>例  
 次のコード サンプルは、前の手順のコードです。  
  
 [!code-csharp[CodeDOM Class Sample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/program.cs#1)]
 [!code-vb[CodeDOM Class Sample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/program.vb#1)]  
  
 前のサンプルをコンパイルし、実行すると、次のソース コードが生成されます。  
  
 [!code-csharp[CodeDOM Class Sample#99](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDOM Class Sample/CS/SampleCode.cs#99)]
 [!code-vb[CodeDOM Class Sample#99](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDOM Class Sample/VB/SampleCode.vb#99)]  
  
 生成されたソース コードは、コンパイルされ、実行されると、次の内容を出力します。  
  
```output
The object:  
 width = 5.3,  
 height = 6.9,  
 area = 36.57  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- このコード サンプルを正しく実行するには、`FullTrust` アクセス許可を設定する必要があります。  
  
## <a name="see-also"></a>関連項目

- [CodeDOM の使用方法](using-the-codedom.md)
- [CodeDOM グラフからのソース コードの生成およびコンパイル](generating-and-compiling-source-code-from-a-codedom-graph.md)
