---
title: CodeDOM の使用方法
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- code compilers
- Code Document Object Model
- Code Document Object Model, graphs
- types, CodeDOM
- namespaces [.NET Framework], CodeDOM
- templated code generation
- dynamically representing source code
- source code models
- CodeDOM
- graphing with CodeDOM
- dynamic compilation
- code generators
- CodeDOM, graphs
ms.assetid: 0444ddf3-c3f6-44ed-a999-f710d9c3e0cf
ms.openlocfilehash: c4cab79976acae236de5a8eaad5a42cdba7d04f9
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130006"
---
# <a name="using-the-codedom"></a>CodeDOM の使用方法
CodeDOM には一般的なさまざま種類のソース コード要素を表す型が用意されています。 オブジェクト グラフをアセンブルする CodeDOM 要素を使用すると、ソース コード モデルを構築するプログラムをデザインできます。 このオブジェクト グラフは、サポートされているプログラミング言語用の CodeDOM コード ジェネレーターを使用して、ソース コードとしてレンダリングできます。 また、CodeDOM を使用して、ソース コードをバイナリ アセンブリにコンパイルすることもできます。  
  
 CodeDOM の一般的な使用方法の例は次のとおりです。  
  
- テンプレートを使ったコード生成。ASP.NET、XML Web サービス クライアント プロキシ、コード ウィザード、デザイナー、またはその他のコード出力機構のためのコードを生成します。  
  
- 動的コンパイル。1 つ以上の言語でのコードのコンパイルをサポートします。  
  
## <a name="building-a-codedom-graph"></a>CodeDOM グラフの構築  
 <xref:System.CodeDom> 名前空間には、言語の構文に依存しない、ソース コードの論理構造を表すクラスが用意されています。  
  
### <a name="the-structure-of-a-codedom-graph"></a>CodeDOM グラフの構造  
 CodeDOM グラフの構造はコンテナーのツリーに似ています。 コンパイル可能な CodeDOM グラフの最上位のコンテナー、つまりルートのコンテナーは <xref:System.CodeDom.CodeCompileUnit> です。 ソース コード モデルの各要素は、CodeDOM グラフ内の <xref:System.CodeDom.CodeObject> のプロパティを通じて CodeDOM グラフにリンクされている必要があります。  
  
### <a name="building-a-source-code-model-for-a-sample-hello-world-program"></a>サンプルの Hello World プログラム用ソース コード モデルの構築  
 単純な Hello World アプリケーションのコードを表す CodeDOM オブジェクト グラフを構築する手順の例を次に示します。 このコード サンプルのソース コード例全体については、「<xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType>」を参照してください。  
  
#### <a name="creating-a-compile-unit"></a>コンパイル単位の作成  
 CodeDOM は、<xref:System.CodeDom.CodeCompileUnit> と呼ばれるオブジェクトを定義します。このオブジェクトでは、コンパイルするソース コードをモデル化した CodeDOM オブジェクト グラフを参照できます。 **CodeCompileUnit** には、属性、名前空間、およびアセンブリへの参照を格納するためのプロパティが用意されています。  
  
 <xref:System.CodeDom.Compiler.CodeDomProvider> クラスから派生する CodeDom プロバイダーには、**CodeCompileUnit** が参照するオブジェクト グラフを処理するメソッドが含まれています。  
  
 簡単なアプリケーション用のオブジェクト グラフを作成するには、ソース コード モデルをアセンブルし、生成されたオブジェクト グラフを **CodeCompileUnit** から参照する必要があります。  
  
 新しいコンパイル単位を作成するには、次の例に示す構文を使います。  
  
 [!code-cpp[CodeDomExample#12](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#12)]
 [!code-csharp[CodeDomExample#12](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#12)]
 [!code-vb[CodeDomExample#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#12)]  
  
 <xref:System.CodeDom.CodeSnippetCompileUnit> には、既に対象言語で記述されているソース コードのセクションを格納できますが、この単位は別の言語にはレンダリングできません。  
  
#### <a name="defining-a-namespace"></a>名前空間の定義  
 名前空間を定義するには、<xref:System.CodeDom.CodeNamespace> を作成してから、適切なコンストラクターを使用するか **Name** プロパティを設定することにより、定義する名前空間の名前を割り当てます。  
  
 [!code-cpp[CodeDomExample#13](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#13)]
 [!code-csharp[CodeDomExample#13](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#13)]
 [!code-vb[CodeDomExample#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#13)]  
  
#### <a name="importing-a-namespace"></a>名前空間のインポート  
 名前空間のインポート ディレクティブを名前空間に追加するには、インポートする名前空間を示す <xref:System.CodeDom.CodeNamespaceImport> を **CodeNamespace.Imports** コレクションに追加します。  
  
 **System** 名前空間のインポートを、`samples` という名前の **CodeNamespace** の **Imports** コレクションに追加するコードを次に示します。  
  
 [!code-cpp[CodeDomExample#14](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#14)]
 [!code-csharp[CodeDomExample#14](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#14)]
 [!code-vb[CodeDomExample#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#14)]  
  
#### <a name="linking-code-elements-into-the-object-graph"></a>コード要素のオブジェクト グラフへのリンク  
 CodeDOM グラフを形成するすべてのコード要素は、CodeDOM グラフのルート オブジェクトのプロパティから直接参照される要素間の一連の参照によって、ツリーのルート要素である <xref:System.CodeDom.CodeCompileUnit> にリンクする必要があります。 オブジェクトをコンテナー オブジェクトのプロパティに設定し、コンテナー オブジェクトからの参照を確立します。  
  
 `samples` **CodeNamespace** をルートの **CodeCompileUnit** の **Namespaces** コレクション プロパティに追加するステートメントを次に示します。  
  
 [!code-cpp[CodeDomExample#15](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#15)]
 [!code-csharp[CodeDomExample#15](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#15)]
 [!code-vb[CodeDomExample#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#15)]  
  
#### <a name="defining-a-type"></a>型の定義  
 CodeDOM を使ってクラス、構造体、インターフェイス、または列挙体を宣言するには、新しい <xref:System.CodeDom.CodeTypeDeclaration> を作成し、名前を割り当てます。 このために、**Name** プロパティを設定するコンストラクターのオーバーロードを使用する例を次に示します。  
  
 [!code-cpp[CodeDomExample#16](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#16)]
 [!code-csharp[CodeDomExample#16](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#16)]
 [!code-vb[CodeDomExample#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#16)]  
  
 名前空間に型を追加するには、名前空間に追加する型を表す <xref:System.CodeDom.CodeTypeDeclaration> を、**CodeNamespace** の **Types** コレクションに追加します。  
  
 `class1` という名前のクラスを `samples` という名前の **CodeNamespace** に追加する例を次に示します。  
  
 [!code-cpp[CodeDomExample#17](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#17)]
 [!code-csharp[CodeDomExample#17](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#17)]
 [!code-vb[CodeDomExample#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#17)]  
  
#### <a name="adding-class-members-to-a-class"></a>クラスへのクラス メンバーの追加  
 <xref:System.CodeDom> 名前空間には、クラス メンバーを表すために使用できるさまざまな要素が用意されています。 各クラス メンバーは、<xref:System.CodeDom.CodeTypeDeclaration> の **Members** コレクションに追加できます。  
  
#### <a name="defining-a-code-entry-point-method-for-an-executable"></a>実行可能プログラムのコード エントリ ポイント メソッドの定義  
 実行可能プログラムのコードを記述する場合は、プログラムの実行を開始するメソッドを表す <xref:System.CodeDom.CodeEntryPointMethod> を作成して、プログラムのエントリ ポイントを指定する必要があります。  
  
 "Hello World!" と出力するための **System.Console.WriteLine** を呼び出す <xref:System.CodeDom.CodeMethodInvokeExpression> を格納しているエントリ ポイント メソッドを定義する例を次に示します。  
  
 [!code-cpp[CodeDomExample#18](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#18)]
 [!code-csharp[CodeDomExample#18](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#18)]
 [!code-vb[CodeDomExample#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#18)]  
  
 `Start` という名前のエントリ ポイント メソッドを `class1` の **Members** コレクションに追加するステートメントを次に示します。  
  
 [!code-cpp[CodeDomExample#19](../../../samples/snippets/cpp/VS_Snippets_CLR/CodeDomExample/CPP/source2.cpp#19)]
 [!code-csharp[CodeDomExample#19](../../../samples/snippets/csharp/VS_Snippets_CLR/CodeDomExample/CS/source2.cs#19)]
 [!code-vb[CodeDomExample#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CodeDomExample/VB/source2.vb#19)]  
  
 これで、<xref:System.CodeDom.CodeCompileUnit> という名前の `compileUnit` に、簡単な Hello World プログラムの CodeDOM グラフが追加されました。 CodeDOM グラフからのコードの生成およびコンパイルについては、「[CodeDOM グラフからのソース コードの生成およびプログラムのコンパイル](generating-and-compiling-source-code-from-a-codedom-graph.md)」を参照してください。  
  
### <a name="more-information-on-building-a-codedom-graph"></a>CodeDOM グラフの構築に関する詳細  
 CodeDOM では、共通言語ランタイムをサポートするプログラミング言語の一般的なさまざまな種類のコード要素をサポートします。 CodeDOM は、プログラミング言語のすべての機能を表す要素を提供するようにはデザインされていません。 CodeDOM 要素で簡単に表すことができないコードは、<xref:System.CodeDom.CodeSnippetExpression>、<xref:System.CodeDom.CodeSnippetStatement>、<xref:System.CodeDom.CodeSnippetTypeMember>、または <xref:System.CodeDom.CodeSnippetCompileUnit> にカプセル化できます。 ただし、CodeDOM ではコードを自動的に他の言語に変換することはできません。  
  
 CodeDOM のそれぞれの型については、<xref:System.CodeDom> 名前空間の説明を参照してください。  
  
 特定の種類のコード要素を表す CodeDOM 要素を探すための一覧については、「[CodeDOM Quick Reference](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100))」(CodeDOM クイック リファレンス) を参照してください。
