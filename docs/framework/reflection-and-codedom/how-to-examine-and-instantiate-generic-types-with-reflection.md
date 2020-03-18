---
title: '方法 : リフレクションを使用してジェネリック型をチェックおよびインスタンス化する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection, generic types
- generics [.NET Framework], reflection
ms.assetid: f93b03b0-1778-43fc-bc6d-35983d210e74
ms.openlocfilehash: 62e4e066740d0422f8f7045b043a5725278c209c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130129"
---
# <a name="how-to-examine-and-instantiate-generic-types-with-reflection"></a>方法 : リフレクションを使用してジェネリック型をチェックおよびインスタンス化する
ジェネリック型の情報は、他の型の情報と同じ方法で取得します。つまり、ジェネリック型を表す <xref:System.Type> オブジェクトをチェックすることによって取得します。 根本的な違いとして、ジェネリック型には、そのジェネリック型パラメーターを表す <xref:System.Type> オブジェクトの一覧が含まれています。 このセクションでは、まず、ジェネリック型をチェックする手順を示します。  
  
 ジェネリック型の定義の型パラメーターに型引数をバインドすることにより、構築された型を表す <xref:System.Type> オブジェクトを作成できます。 2 番目の手順でこの方法を示します。  
  
### <a name="to-examine-a-generic-type-and-its-type-parameters"></a>ジェネリック型とその型パラメーターを確認するには  
  
1. ジェネリック型を表す <xref:System.Type> のインスタンスを取得します。 次のコードでは、C# の `typeof` 演算子 (Visual Basic では `GetType`、Visual C++ では `typeid`) を使用して型を取得します。 <xref:System.Type> オブジェクトを取得する他の方法については、<xref:System.Type> クラスのトピックを参照してください。 この手順では、これ以降、型は `t` という名前のメソッド パラメーターに格納されます。  
  
     [!code-cpp[HowToGeneric#2](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#2)]
     [!code-csharp[HowToGeneric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#2)]
     [!code-vb[HowToGeneric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#2)]  
  
2. <xref:System.Type.IsGenericType%2A> プロパティを使用して、型がジェネリックかどうかを確認し、<xref:System.Type.IsGenericTypeDefinition%2A> プロパティを使用して型がジェネリック型の定義かどうかを確認します。  
  
     [!code-cpp[HowToGeneric#3](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#3)]
     [!code-csharp[HowToGeneric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#3)]
     [!code-vb[HowToGeneric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#3)]  
  
3. <xref:System.Type.GetGenericArguments%2A> メソッドを使用して、ジェネリック型引数が格納された配列を取得します。  
  
     [!code-cpp[HowToGeneric#4](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#4)]
     [!code-csharp[HowToGeneric#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#4)]
     [!code-vb[HowToGeneric#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#4)]  
  
4. 各型引数について、<xref:System.Type.IsGenericParameter%2A> プロパティを使用して、その型引数が型パラメーター (ジェネリック型定義の型パラメーターなど) であるか、型パラメーターに指定された型 (構築された型の型パラメーターなど) であるかを確認します。  
  
     [!code-cpp[HowToGeneric#5](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#5)]
     [!code-csharp[HowToGeneric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#5)]
     [!code-vb[HowToGeneric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#5)]  
  
5. 型システムでは、ジェネリック型パラメーターは、通常の型と同様に <xref:System.Type> のインスタンスによって表されます。 次のコードでは、ジェネリック型パラメーターを表す <xref:System.Type> オブジェクトの名前とパラメーター位置を表示します。 パラメーター位置はここでは重要な情報ではありませんが、別のジェネリック型の型引数として使用されている型パラメーターをチェックする場合に重要となります。  
  
     [!code-cpp[HowToGeneric#6](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#6)]
     [!code-csharp[HowToGeneric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#6)]
     [!code-vb[HowToGeneric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#6)]  
  
6. <xref:System.Type.GetGenericParameterConstraints%2A> メソッドを使用して、ジェネリック型パラメーターの基本型の制約とインターフェイスの制約を確認し、単一の配列内のすべての制約を取得します。 制約は、特定の順序であるという保証はありません。  
  
     [!code-cpp[HowToGeneric#7](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#7)]
     [!code-csharp[HowToGeneric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#7)]
     [!code-vb[HowToGeneric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#7)]  
  
7. <xref:System.Type.GenericParameterAttributes%2A> プロパティを使用して、型パラメーターの特殊な制約 (参照型であることが必要など) を確認します。 このプロパティには、分散を表す値も含まれています。次のコードに示すように、この値にはマスクを設定できます。  
  
     [!code-cpp[HowToGeneric#8](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#8)]
     [!code-csharp[HowToGeneric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#8)]
     [!code-vb[HowToGeneric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#8)]  
  
8. 特殊な制約の属性はフラグであり、特殊な制約がないことを表すフラグ (<xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>) と同じフラグが、共変性や反変性がないことも表します。 したがって、これらの条件のいずれかを調べるには、適切なマスクを使用する必要があります。 ここでは、<xref:System.Reflection.GenericParameterAttributes.SpecialConstraintMask?displayProperty=nameWithType> を使用して、特殊な制約のフラグを切り分けます。  
  
     [!code-cpp[HowToGeneric#9](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#9)]
     [!code-csharp[HowToGeneric#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#9)]
     [!code-vb[HowToGeneric#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#9)]  
  
## <a name="constructing-an-instance-of-a-generic-type"></a>ジェネリック型のインスタンスの構築  
 ジェネリック型はテンプレートに似ています。 ジェネリック型パラメーターの実際の型を指定しない限り、インスタンスを作成することはできません。 リフレクションを使用して実行時にこれを行うには、<xref:System.Type.MakeGenericType%2A> メソッドが必要です。  
  
#### <a name="to-construct-an-instance-of-a-generic-type"></a>ジェネリック型のインスタンスを構築するには  
  
1. ジェネリック型を表す <xref:System.Type> オブジェクトを取得します。 次のコードでは、2 とおりの方法でジェネリック型 <xref:System.Collections.Generic.Dictionary%602> を取得します。型を示す文字列を指定して <xref:System.Type.GetType%28System.String%29?displayProperty=nameWithType> メソッド オーバーロードを使用する方法と、構築された型 `Dictionary\<String, Example>` (Visual Basic では `Dictionary(Of String, Example)`) で <xref:System.Type.GetGenericTypeDefinition%2A> メソッドを呼び出す方法です。 <xref:System.Type.MakeGenericType%2A> メソッドには、ジェネリック型の定義が必要です。  
  
     [!code-cpp[HowToGeneric#10](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#10)]
     [!code-csharp[HowToGeneric#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#10)]
     [!code-vb[HowToGeneric#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#10)]  
  
2. 型引数の配列を構築して、型パラメーターと置き換えます。 配列には、型パラメーター リストに表示される順序と同じ順序で、正確な数の <xref:System.Type> オブジェクトを格納する必要があります。 この場合、キー (最初の型パラメーター) の型は <xref:System.String> であり、ディクショナリの値は `Example` というクラスのインスタンスです。  
  
     [!code-cpp[HowToGeneric#11](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#11)]
     [!code-csharp[HowToGeneric#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#11)]
     [!code-vb[HowToGeneric#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#11)]  
  
3. <xref:System.Type.MakeGenericType%2A> メソッドを呼び出して型引数を型パラメーターにバインドし、型を構築します。  
  
     [!code-cpp[HowToGeneric#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#12)]
     [!code-csharp[HowToGeneric#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#12)]
     [!code-vb[HowToGeneric#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#12)]  
  
4. <xref:System.Activator.CreateInstance%28System.Type%29> メソッド オーバーロードを使用して、構築された型のオブジェクトを作成します。 次のコードでは、作成された `Dictionary<String, Example>` オブジェクトに、`Example` クラスの 2 つのインスタンスを格納します。  
  
     [!code-cpp[HowToGeneric#13](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#13)]
     [!code-csharp[HowToGeneric#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#13)]
     [!code-vb[HowToGeneric#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#13)]  
  
## <a name="example"></a>例  
 `DisplayGenericType` メソッドを定義して、コードで使用されているジェネリック型の定義と構築された型をチェックし、この情報を表示するコード例を次に示します。 `DisplayGenericType` メソッドは、<xref:System.Type.IsGenericType%2A>、<xref:System.Type.IsGenericParameter%2A>、<xref:System.Type.GenericParameterPosition%2A> の各プロパティと <xref:System.Type.GetGenericArguments%2A> メソッドの使用方法を示します。  
  
 この例では、`DisplayGenericParameter` メソッドも定義してジェネリック型パラメーターをチェックし、その制約を表示します。  
  
 このコード例では、型パラメーターの制約を示すジェネリック型を含む一連のテスト用の型を定義し、これらの型の情報を表示する方法を示します。  
  
 この例では、型引数の配列を作成し、<xref:System.Type.MakeGenericType%2A> メソッドを呼び出して、<xref:System.Collections.Generic.Dictionary%602> クラスから型を構築します。 プログラムでは、<xref:System.Type.MakeGenericType%2A> を使用して構築された <xref:System.Type> オブジェクトと、`typeof` (Visual Basic では `GetType`) を使用して取得した <xref:System.Type> オブジェクトを比較し、これらが同じであることを示します。 同様に、<xref:System.Type.GetGenericTypeDefinition%2A> メソッドを使用して構築された型のジェネリック型の定義を取得し、<xref:System.Collections.Generic.Dictionary%602> クラスを表す <xref:System.Type> オブジェクトと比較します。  
  
 [!code-cpp[HowToGeneric#1](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#1)]
 [!code-csharp[HowToGeneric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#1)]
 [!code-vb[HowToGeneric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#1)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Type>
- <xref:System.Reflection.MethodInfo>
- [リフレクションとジェネリック型](reflection-and-generic-types.md)
- [型情報の表示](viewing-type-information.md)
- [ジェネリック](../../standard/generics/index.md)
