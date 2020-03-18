---
title: 属性に格納されている情報の取得
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- retrieving attributes
- multiple attribute instances
- attributes [.NET Framework], retrieving
ms.assetid: 37dfe4e3-7da0-48b6-a3d9-398981524e1c
ms.openlocfilehash: 4f0f3555ae1ab7e662d5f88ac65739a7c791a964
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78158078"
---
# <a name="retrieving-information-stored-in-attributes"></a>属性に格納されている情報の取得
カスタム属性の取得は簡単なプロセスです。 まず、取得する属性のインスタンスを宣言します。 次に、<xref:System.Attribute.GetCustomAttribute%2A?displayProperty=nameWithType> メソッドを使用して、取得する属性の値に新しい属性を初期化します。 新しい属性が初期化されたら、そのプロパティを使用して値を取得できるようになります。  
  
> [!IMPORTANT]
> このトピックでは、実行コンテキストに読み込まれるコードのカスタム属性を取得する方法について説明します。 リフレクションのみのコンテキストに読み込まれたコードの属性を取得するには、「<xref:System.Reflection.CustomAttributeData>方法: リフレクションのみのコンテキストにアセンブリを読み込む[」で説明されているように、](../../../docs/framework/reflection-and-codedom/how-to-load-assemblies-into-the-reflection-only-context.md) クラスを使用する必要があります。  
  
 このセクションでは、以下の属性の取得方法について説明します。  
  
- [属性の単一のインスタンスを取得する](#cpconretrievingsingleinstanceofattribute)  
  
- [同じスコープに適用された属性の複数のインスタンスを取得する](#cpconretrievingmultipleinstancesofattributeappliedtosamescope)  
  
- [異なるスコープに適用された属性の複数のインスタンスを取得する](#cpconretrievingmultipleinstancesofattributeappliedtodifferentscopes)  
  
<a name="cpconretrievingsingleinstanceofattribute"></a>
## <a name="retrieving-a-single-instance-of-an-attribute"></a>属性の単一のインスタンスを取得する  
 次の例では、(前のセクションで説明した) `DeveloperAttribute` がクラス レベルで `MainApp` クラスに適用されます。 `GetAttribute` メソッドは **GetCustomAttribute** を使用して、クラス レベルで `DeveloperAttribute` に格納されている値を取得してからコンソールに表示します。  
  
 [!code-cpp[Conceptual.Attributes.Usage#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#18)]
 [!code-csharp[Conceptual.Attributes.Usage#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#18)]
 [!code-vb[Conceptual.Attributes.Usage#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#18)]  
  
 このプログラムを実行すると、次のテキストが表示されます。  
  
```console  
The Name Attribute is: Joan Smith.  
The Level Attribute is: 42.  
The Reviewed Attribute is: True.  
```  
  
 属性が見つからない場合、**GetCustomAttribute** メソッドは `MyAttribute` を null 値に初期化します。 この例では、そのようなインスタンスの `MyAttribute` を確認し、属性が見つからない場合はユーザーに通知します。 クラス スコープ内に `DeveloperAttribute` 見つからない場合は、次のメッセージがコンソールに表示されます。  
  
```console  
The attribute was not found.
```  
  
 この例では、属性の定義が現在の名前空間にあると仮定しています。 属性の定義が現在の名前空間にない場合は、その定義が存在する名前空間を忘れずにインポートしてください。  
  
<a name="cpconretrievingmultipleinstancesofattributeappliedtosamescope"></a>
## <a name="retrieving-multiple-instances-of-an-attribute-applied-to-the-same-scope"></a>同じスコープに適用された属性の複数のインスタンスを取得する  
 前の例では、検査するクラスと検索する特定の属性が <xref:System.Attribute.GetCustomAttribute%2A> に渡されます。 このコードは、クラス レベルで属性のインスタンスが 1 つのみ適用される場合に動作します。 ただし、属性のインスタンスが同じクラス レベルに複数適用されている場合、**GetCustomAttribute** メソッドはすべての情報を取得しません。 同じ属性のインスタンスが同じスコープに複数適用されている場合、<xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType> を使用して、属性のすべてのインスタンスを配列に配置することができます。 たとえば、同じクラスのクラス レベルで 2 インスタンスの `DeveloperAttribute` が適用された場合、両方の属性にある情報を表示するように `GetAttribute` メソッドを変更できます。 ただし、同じレベルで複数の属性を適用するには、**で**AllowMultiple**プロパティを**true<xref:System.AttributeUsageAttribute> に設定して属性を定義する必要があります。  
  
 次のコード例は、**GetCustomAttributes** メソッドを使用して、指定されたクラスで `DeveloperAttribute` のすべてのインスタンスを参照する配列を作成する方法を示しています。 すべての属性の値がコンソールに表示されます。  
  
 [!code-cpp[Conceptual.Attributes.Usage#19](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#19)]
 [!code-csharp[Conceptual.Attributes.Usage#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#19)]
 [!code-vb[Conceptual.Attributes.Usage#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#19)]  
  
 このコードでは、属性が見つからない場合、ユーザーに警告されます。 それ以外の場合は、`DeveloperAttribute` の両方のインスタンスに含まれる情報が表示されます。  
  
<a name="cpconretrievingmultipleinstancesofattributeappliedtodifferentscopes"></a>
## <a name="retrieving-multiple-instances-of-an-attribute-applied-to-different-scopes"></a>異なるスコープに適用された属性の複数のインスタンスを取得する  
 <xref:System.Attribute.GetCustomAttributes%2A> メソッドと <xref:System.Attribute.GetCustomAttribute%2A> メソッドは、クラス全体を検索し、そのクラスに含まれる属性のすべてのインスタンスを返す処理を行いません。 代わりに、指定されたメソッドまたはメンバーを一度に 1 つのみ検索します。 各メンバーに同じ属性が適用されているクラスがあり、それらのメンバーに適用されているすべての属性の値を取得する場合は、すべてのメソッドまたはメンバーを個別に **GetCustomAttributes** および **GetCustomAttribute** に渡す必要があります。  
  
 次のコード例では、クラスをパラメーターとして受け取り、クラス レベルとそのクラスの個々のメソッドごとに `DeveloperAttribute` (以前に定義) を検索します。  
  
 [!code-cpp[Conceptual.Attributes.Usage#20](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.attributes.usage/cpp/source3.cpp#20)]
 [!code-csharp[Conceptual.Attributes.Usage#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.attributes.usage/cs/source3.cs#20)]
 [!code-vb[Conceptual.Attributes.Usage#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.attributes.usage/vb/source3.vb#20)]  
  
 メソッド レベルまたはクラス レベルで `DeveloperAttribute` のインスタンスが見つからない場合、`GetAttribute` メソッドは属性が見つからなかったことをユーザーに通知し、その属性を含まないメソッドまたはクラスの名前を表示します。 属性が見つかると、`Name`、`Level`、および `Reviewed` フィールドがコンソールに表示されます。  
  
 <xref:System.Type> クラスのメンバーを使用して、渡されたクラスの個々のメソッドとメンバーを取得できます。 この例では、まず **Type** オブジェクトに対してクエリを実行して、クラス レベルの属性情報を取得します。 次に、<xref:System.Type.GetMethods%2A?displayProperty=nameWithType> を使用してすべてのメソッドのインスタンスを <xref:System.Reflection.MemberInfo?displayProperty=nameWithType> オブジェクトの配列に配置し、メソッド レベルの属性情報を取得します。 <xref:System.Type.GetProperties%2A?displayProperty=nameWithType> メソッドを使用して、プロパティ レベルまたは <xref:System.Type.GetConstructors%2A?displayProperty=nameWithType> で属性を確認したり、コンストラクター レベルで属性を確認したりすることもできます。  
  
## <a name="see-also"></a>参照

- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttribute%2A?displayProperty=nameWithType>
- <xref:System.Attribute.GetCustomAttributes%2A?displayProperty=nameWithType>
- [属性](../../../docs/standard/attributes/index.md)
