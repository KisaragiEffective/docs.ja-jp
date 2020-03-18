---
title: .NET の StringBuilder クラスを使用する
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Remove method
- strings [.NET Framework], capacities
- StringBuilder object
- Replace method
- AppendFormat method
- Append method
- Insert method
- strings [.NET Framework], StringBuilder object
ms.assetid: 5c14867c-9a99-45bc-ae7f-2686700d377a
ms.openlocfilehash: 19ee90f3300e3b610eeefd4949baa2759b834a60
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73121673"
---
# <a name="using-the-stringbuilder-class-in-net"></a>.NET の StringBuilder クラスを使用する
<xref:System.String> オブジェクトは、変更できません。 <xref:System.String?displayProperty=nameWithType> クラスのメソッドのいずれかを使用するたびに、新しい文字列オブジェクトをメモリ内に作成します。その際、その新しいオブジェクトに対して領域を新たに割り当てる必要があります。 文字列に対して何度も変更を実行する必要がある場合、新しい <xref:System.String> オブジェクトの作成に関連したオーバーヘッドが高コストになる可能性があります。 新しいオブジェクトを作成せずに文字列を変更したい場合は、<xref:System.Text.StringBuilder?displayProperty=nameWithType> クラスを使用することができます。 たとえば、ループで多数の文字列を連結する場合に、<xref:System.Text.StringBuilder> クラスを使用してパフォーマンスを向上させることができます。  
  
## <a name="importing-the-systemtext-namespace"></a>System.Text 名前空間のインポート  
 <xref:System.Text.StringBuilder> クラスは、<xref:System.Text> 名前空間にあります。  完全修飾型名をコードに指定しなくてもすむように、<xref:System.Text> 名前空間をインポートすることができます。  
  
 [!code-cpp[Conceptual.StringBuilder#11](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#11)]
 [!code-csharp[Conceptual.StringBuilder#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#11)]
 [!code-vb[Conceptual.StringBuilder#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#11)]  
  
## <a name="instantiating-a-stringbuilder-object"></a>StringBuilder オブジェクトのインスタンス化  
 以下の例に示すように、オーバーロードされたコンストラクター メソッドの 1 つで変数を初期化することにより、<xref:System.Text.StringBuilder> クラスの新しいインスタンスを作成することができます。  
  
 [!code-cpp[Conceptual.StringBuilder#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#1)]
 [!code-csharp[Conceptual.StringBuilder#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#1)]
 [!code-vb[Conceptual.StringBuilder#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#1)]  
  
## <a name="setting-the-capacity-and-length"></a>容量と長さの設定  
 <xref:System.Text.StringBuilder> は、カプセル化する文字列内の文字数を拡張できるようにする動的オブジェクトですが、保持可能な最大文字数の値を指定することができます。 この値を、オブジェクトの容量と呼びます。これを現行の <xref:System.Text.StringBuilder> が保持する文字列の長さと混同すべきではありません。 たとえば、"Hello" という長さ 5 の文字列を持つ <xref:System.Text.StringBuilder> クラスの新しいインスタンスを作成するときに、オブジェクトの最大容量として 25 を指定することができます。 <xref:System.Text.StringBuilder> を変更する際、容量に達するまでは、自動再割り当ては発生しません。 容量に達すると、新しい領域が自動的に割り当てられ、容量が 2 倍になります。 オーバーロードされたコンストラクターのいずれかを使用して、<xref:System.Text.StringBuilder> クラスの容量を指定することができます。 次の例は、`myStringBuilder` オブジェクトを最大 25 の領域に拡張できることを示しています。  
  
 [!code-cpp[Conceptual.StringBuilder#2](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#2)]
 [!code-csharp[Conceptual.StringBuilder#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#2)]
 [!code-vb[Conceptual.StringBuilder#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#2)]  
  
 また、<xref:System.Text.StringBuilder.Capacity%2A> の読み取り/書き込みプロパティを使用して、オブジェクトの最大長を設定することができます。 次の例では、**Capacity** プロパティを使用して、オブジェクトの最大長を定義しています。  
  
 [!code-cpp[Conceptual.StringBuilder#3](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#3)]
 [!code-csharp[Conceptual.StringBuilder#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#3)]
 [!code-vb[Conceptual.StringBuilder#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#3)]  
  
 <xref:System.Text.StringBuilder.EnsureCapacity%2A> メソッドを使用して、現行 **StringBuilder** の容量を確認することができます。 容量が渡された値よりも大きい場合、変更は行われません。しかし、容量が渡された値より小さい場合は、渡された値と一致するよう現行の容量が変更されます。  
  
 <xref:System.Text.StringBuilder.Length%2A> プロパティを表示または設定することもできます。 **Length** プロパティを、**Capacity** プロパティより大きな値に設定する場合、**Capacity** プロパティは **Length** プロパティと同じ値に自動的に変更されます。 **Length** プロパティを、現行の **StringBuilder** 内の文字列の長さより小さい値に設定すると、文字列は短縮されます。  
  
## <a name="modifying-the-stringbuilder-string"></a>StringBuilder 文字列の変更  
 **StringBuilder** の内容の変更に使用できるメソッドを次の表に一覧表示します。  
  
|メソッド名|使用|  
|-----------------|---------|  
|<xref:System.Text.StringBuilder.Append%2A?displayProperty=nameWithType>|現行の **StringBuilder** の末尾に情報を追加します。|  
|<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>|文字列に渡される書式指定子を、書式設定されたテキスト文字列で置き換えます。|  
|<xref:System.Text.StringBuilder.Insert%2A?displayProperty=nameWithType>|現行の **StringBuilder** の指定されたインデックスに、文字列またはオブジェクトを挿入します。|  
|<xref:System.Text.StringBuilder.Remove%2A?displayProperty=nameWithType>|現行の **StringBuilder** から、指定された文字数を削除します。|  
|<xref:System.Text.StringBuilder.Replace%2A?displayProperty=nameWithType>|指定されたインデックスで、指定された文字を置き換えます。|  
  
### <a name="append"></a>追加  
 **Append** メソッドを使用して、現行 **StringBuilder** によって表される文字列の末尾にオブジェクトのテキストまたは文字列形式を追加することができます。 次の例では、**StringBuilder** を "Hello World" に初期設定し、テキストをオブジェクトの末尾に追加しています。 領域は、必要に応じて自動的に割り当てられます。  
  
 [!code-cpp[Conceptual.StringBuilder#4](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#4)]
 [!code-csharp[Conceptual.StringBuilder#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#4)]
 [!code-vb[Conceptual.StringBuilder#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#4)]  
  
### <a name="appendformat"></a>AppendFormat  
 <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> メソッドは、<xref:System.Text.StringBuilder> オブジェクトの末尾にテキストを追加します。 これは、書式設定される 1 つ以上のオブジェクトの <xref:System.IFormattable> 実装を呼び出すことにより、複合書式機能をサポートしています (詳細については、「[複合書式指定](../../../docs/standard/base-types/composite-formatting.md)」を参照してください)。 そのため、数値、日時、および列挙の値に対して標準書式文字列を受け取り、数値と日時の値、およびカスタム型に定義されている書式文字列に対してカスタム書式文字列を受け取ります。 (書式設定については、「[型の書式設定](../../../docs/standard/base-types/formatting-types.md)」を参照してください。)このメソッドを使用して、変数の書式をカスタマイズし、その値を <xref:System.Text.StringBuilder> に追加することができます。 次の例では、<xref:System.Text.StringBuilder.AppendFormat%2A> メソッドを使用して、<xref:System.Text.StringBuilder> オブジェクトの末尾に、通貨値として書式設定されている整数値を挿入しています。  
  
 [!code-cpp[Conceptual.StringBuilder#5](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#5)]
 [!code-csharp[Conceptual.StringBuilder#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#5)]
 [!code-vb[Conceptual.StringBuilder#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#5)]  
  
### <a name="insert"></a>挿入  
 <xref:System.Text.StringBuilder.Insert%2A> メソッドは、現行の <xref:System.Text.StringBuilder> オブジェクトの指定された位置に、文字列またはオブジェクトを追加します。 次の例では、このメソッドを使用して、<xref:System.Text.StringBuilder> オブジェクトの 6 番目の位置に単語を挿入しています。  
  
 [!code-cpp[Conceptual.StringBuilder#6](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#6)]
 [!code-csharp[Conceptual.StringBuilder#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#6)]
 [!code-vb[Conceptual.StringBuilder#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#6)]  
  
### <a name="remove"></a>削除  
 **Remove** メソッドを使用して、現行の <xref:System.Text.StringBuilder> オブジェクトから指定された文字数を削除します (0 から始まる指定されたインデックスで開始します)。 次の例では、**Remove** メソッドを使用して、<xref:System.Text.StringBuilder> オブジェクトを短縮しています。  
  
 [!code-cpp[Conceptual.StringBuilder#7](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#7)]
 [!code-csharp[Conceptual.StringBuilder#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#7)]
 [!code-vb[Conceptual.StringBuilder#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#7)]  
  
### <a name="replace"></a>Replace  
 **Replace** メソッドを使用して、<xref:System.Text.StringBuilder> オブジェクト内の文字を、指定された別の文字で置き換えることができます。 次の例では、**Replace** メソッドを使用して、感嘆符 (!) のすべてのインスタンスを求めて <xref:System.Text.StringBuilder> オブジェクトを検索し、疑問符 (?) で置き換えています。  
  
 [!code-cpp[Conceptual.StringBuilder#8](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#8)]
 [!code-csharp[Conceptual.StringBuilder#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#8)]
 [!code-vb[Conceptual.StringBuilder#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#8)]  
  
## <a name="converting-a-stringbuilder-object-to-a-string"></a>文字列への StringBuilder オブジェクトの変換  
 <xref:System.Text.StringBuilder> オブジェクトで表される文字列を <xref:System.String> パラメーターを持つメソッドに渡すかそれをユーザー インターフェイスに表示するには、事前に <xref:System.Text.StringBuilder> オブジェクトを <xref:System.String> オブジェクトに変換する必要があります。 この変換は、<xref:System.Text.StringBuilder.ToString%2A?displayProperty=nameWithType> メソッドを呼び出すことによって行います。 次の例では、いくつかの <xref:System.Text.StringBuilder> メソッドを呼び出し、その後、<xref:System.Text.StringBuilder.ToString?displayProperty=nameWithType> メソッドを呼び出して文字列を表示しています。  
  
 [!code-csharp[Conceptual.StringBuilder#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/tostringexample1.cs#10)]
 [!code-vb[Conceptual.StringBuilder#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/tostringexample1.vb#10)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Text.StringBuilder?displayProperty=nameWithType>
- [基本的な文字列操作](../../../docs/standard/base-types/basic-string-operations.md)
- [型の書式設定](../../../docs/standard/base-types/formatting-types.md)
