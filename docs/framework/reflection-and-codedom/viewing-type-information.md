---
title: 型情報の表示
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- types, viewing type information
- Type object
- viewing type information
- reflection, viewing type information
ms.assetid: 7e7303a9-4064-4738-b4e7-b75974ed70d2
ms.openlocfilehash: bf119ff547df59cd369d688fd81ab058893614f2
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130017"
---
# <a name="viewing-type-information"></a>型情報の表示
<xref:System.Type?displayProperty=nameWithType> クラスは、リフレクションの中心です。 共通言語ランタイムは、リフレクションの要求時に読み込まれる型の**型**を作成します。 **型**オブジェクトのメソッド、フィールド、プロパティ、および入れ子になったクラスから、その型に関することがすべてわかります。  
  
 <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> または <xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType> を使用して、読み込まれていないアセンブリから**型**オブジェクトを取得します。これは、目的の型の名前で渡されます。 既に読み込まれているアセンブリから**型**オブジェクトを取得するには、<xref:System.Type.GetType%2A?displayProperty=nameWithType> を使用します。 <xref:System.Reflection.Module.GetType%2A?displayProperty=nameWithType> および <xref:System.Reflection.Module.GetTypes%2A?displayProperty=nameWithType> は、モジュール**型**オブジェクトを取得します。  
  
> [!NOTE]
> ジェネリック型およびメソッドをチェックして操作したい場合は、「[Reflection and Generic Types](reflection-and-generic-types.md)」(リフレクション型とジェネリック型) および「[How to: Examine and Instantiate Generic Types with Reflection](how-to-examine-and-instantiate-generic-types-with-reflection.md)」(方法: リフレクションを使用してジェネリック型をチェックおよびインスタンス化する) に記載された追加情報を参照してください。  
  
 次の例は、アセンブリの <xref:System.Reflection.Assembly> オブジェクトおよびモジュールの取得に必要な構文を示しています。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source5.cpp#6)]
 [!code-csharp[Conceptual.Types.ViewInfo#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source5.cs#6)]
 [!code-vb[Conceptual.Types.ViewInfo#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source5.vb#6)]  
  
 次の例は、読み込まれたアセンブリからの**型**オブジェクトの取得を示しています。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source5.cpp#7)]
 [!code-csharp[Conceptual.Types.ViewInfo#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source5.cs#7)]
 [!code-vb[Conceptual.Types.ViewInfo#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source5.vb#7)]  
  
 **型**を取得すると、その型のメンバーに関する情報をさまざまな方法で確認できます。 たとえば、その型の各メンバーについて説明した <xref:System.Reflection.MemberInfo> オブジェクトの配列を取得する <xref:System.Type.GetMembers%2A?displayProperty=nameWithType> メソッドを呼び出すことで、すべての型のメンバーについて確認できます。  
  
 **Type** クラスのメソッドを使用して、名前で指定した 1 つまたは複数のコンストラクター、メソッド、イベント、フィールド、プロパティに関する情報を取得することもできます。 たとえば、<xref:System.Type.GetConstructor%2A?displayProperty=nameWithType> は現在のクラスの特定のコンストラクターをカプセル化します。  
  
 **型**がある場合、<xref:System.Type.Module%2A?displayProperty=nameWithType> プロパティを使用して、その型が含まれるモジュールをカプセル化するオブジェクトを取得できます。 モジュールが含まれるアセンブリをカプセル化するオブジェクトを見つけるには、<xref:System.Reflection.Module.Assembly%2A?displayProperty=nameWithType> プロパティを使用します。 <xref:System.Type.Assembly%2A?displayProperty=nameWithType> プロパティを使用することで、型を直接カプセル化するアセンブリを取得できます。  
  
## <a name="systemtype-and-constructorinfo"></a>System.Type および ConstructorInfo  
 次の例は、クラス (この場合は <xref:System.String> クラス) のコンストラクターを一覧表示する方法を示しています。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#1](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source1.cpp#1)]
 [!code-csharp[Conceptual.Types.ViewInfo#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source1.cs#1)]
 [!code-vb[Conceptual.Types.ViewInfo#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source1.vb#1)]  
  
## <a name="memberinfo-methodinfo-fieldinfo-and-propertyinfo"></a>MemberInfo、MethodInfo、FieldInfo、および PropertyInfo  
 <xref:System.Reflection.MemberInfo>、<xref:System.Reflection.MethodInfo>、<xref:System.Reflection.FieldInfo>、または <xref:System.Reflection.PropertyInfo> オブジェクトを使用して型のメソッド、プロパティ、イベント、およびフィールドに関する情報を取得します。  
  
 次の例では、**MemberInfo** を使用して **System.IO.File** クラスのメンバー数を一覧表示し、<xref:System.Type.IsPublic%2A> プロパティを使用してクラスの可視性を決定しています。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source2.cpp#2)]
 [!code-csharp[Conceptual.Types.ViewInfo#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source2.cs#2)]
 [!code-vb[Conceptual.Types.ViewInfo#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source2.vb#2)]  
  
 次の例では、指定されたメンバーの型を調べています。 **MemberInfo** クラスのメンバーにリフレクションを実行し、その型を一覧表示しています。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source3.cpp#3)]
 [!code-csharp[Conceptual.Types.ViewInfo#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source3.cs#3)]
 [!code-vb[Conceptual.Types.ViewInfo#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source3.vb#3)]  
  
 次の例では、すべてのリフレクション **\*Info** クラスに <xref:System.Reflection.BindingFlags> を使用して、指定したクラスのすべてのメンバー (コンストラクター、フィールド、プロパティ、イベント、およびメソッド) を一覧表示しています。メンバーは静的およびインスタンスのカテゴリに分割されています。  
  
 [!code-cpp[Conceptual.Types.ViewInfo#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source4.cpp#4)]
 [!code-csharp[Conceptual.Types.ViewInfo#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source4.cs#4)]
 [!code-vb[Conceptual.Types.ViewInfo#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source4.vb#4)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Reflection.BindingFlags>
- <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType>
- <xref:System.Type.GetType%2A?displayProperty=nameWithType>
- <xref:System.Type.GetMembers%2A?displayProperty=nameWithType>
- <xref:System.Type.GetFields%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Module.GetType%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Module.GetTypes%2A?displayProperty=nameWithType>
- <xref:System.Reflection.MemberInfo>
- <xref:System.Reflection.ConstructorInfo>
- <xref:System.Reflection.MethodInfo>
- <xref:System.Reflection.FieldInfo>
- <xref:System.Reflection.EventInfo>
- <xref:System.Reflection.ParameterInfo>
- [リフレクションとジェネリック型](reflection-and-generic-types.md)
