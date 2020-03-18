---
title: '方法: リフレクション出力を使用してジェネリック型を定義する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- generics [.NET Framework], dynamic types
- reflection emit, generic types
ms.assetid: 07d5f01a-7b5b-40ea-9b15-f21561098fe4
ms.openlocfilehash: b553fd2235c73cf879474dc4f44f958dddcb649c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130158"
---
# <a name="how-to-define-a-generic-type-with-reflection-emit"></a>方法: リフレクション出力を使用してジェネリック型を定義する
このトピックでは、2 種類のパラメーターを持つ単純なジェネリック型を作成する方法、クラス制約、インターフェイス制約、特殊な制約をパラメーターに適用する方法、パラメーターの型や戻り値の型としてクラスの型パラメーターを使用するメンバーを作成する方法を紹介します。  
  
> [!IMPORTANT]
> メソッドはジェネリック型に属し、その型の型パラメーターを使用するだけであるため、ジェネリックではありません。 メソッドがジェネリックになるのは、そのメソッドが独自の型パラメーター リストを持つ場合だけです。 ジェネリック型のほとんどのメソッドは、この例のように、ジェネリックではありません。 ジェネリック メソッドの出力の例は、「[方法: リフレクション出力を使用してジェネリック メソッドを定義する](how-to-define-a-generic-method-with-reflection-emit.md)」を参照してください。  
  
### <a name="to-define-a-generic-type"></a>ジェネリック型を定義するには  
  
1. `GenericEmitExample1` という名前の動的アセンブリを定義します。 この例では、アセンブリが実行され、ディスクに保存されます。そのため、<xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndSave?displayProperty=nameWithType> が指定されています。  
  
     [!code-cpp[EmitGenericType#2](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#2)]
     [!code-csharp[EmitGenericType#2](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#2)]
     [!code-vb[EmitGenericType#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#2)]  
  
2. 動的モジュールを定義します。 アセンブリは実行可能モジュールで構成をされます。 単一モジュールのアセンブリの場合、モジュール名はアセンブリ名と同じであり、ファイル名はモジュール名に拡張子が付いたものになります。  
  
     [!code-cpp[EmitGenericType#3](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#3)]
     [!code-csharp[EmitGenericType#3](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#3)]
     [!code-vb[EmitGenericType#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#3)]  
  
3. クラスを定義します。 この例では、クラスに `Sample` という名前が付けられています。  
  
     [!code-cpp[EmitGenericType#4](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#4)]
     [!code-csharp[EmitGenericType#4](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#4)]
     [!code-vb[EmitGenericType#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#4)]  
  
4. パラメーターの名前を格納している文字列の配列を `Sample` メソッドに渡して、<xref:System.Reflection.Emit.TypeBuilder.DefineGenericParameters%2A?displayProperty=nameWithType> のジェネリック型パラメーターを定義します。 これによりクラスがジェネリック型になります。 戻り値は、型パラメーターを表す <xref:System.Reflection.Emit.GenericTypeParameterBuilder> オブジェクトの配列になります。型パラメーターは出力されたコードで利用できます。  
  
     次のコードでは、`Sample` が型パラメーターの `TFirst` と `TSecond` を持つジェネリック型になります。 コードを読みやすくするために、各 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> が型パラメーターと同じ名前の変数に入ります。  
  
     [!code-cpp[EmitGenericType#5](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#5)]
     [!code-csharp[EmitGenericType#5](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#5)]
     [!code-vb[EmitGenericType#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#5)]  
  
5. 型パラメーターに特殊な制約を追加します。 この例では、型パラメーター `TFirst` が、パラメーターのないコンストラクターを持つ型と参照型に制約されています。  
  
     [!code-cpp[EmitGenericType#6](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#6)]
     [!code-csharp[EmitGenericType#6](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#6)]
     [!code-vb[EmitGenericType#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#6)]  
  
6. オプションで、型パラメーターにクラスの制約とインターフェイスの制約を追加します。 この例では、型パラメーター `TFirst` が、変数 `baseType` に含まれる <xref:System.Type> オブジェクトで表される基底クラスから派生し、型が変数の `interfaceA` と `interfaceB` に含まれるインターフェイスを実装する型に制約されています。 これらの変数の宣言と割り当てについては、コード例を参照してください。  
  
     [!code-cpp[EmitGenericType#7](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#7)]
     [!code-csharp[EmitGenericType#7](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#7)]
     [!code-vb[EmitGenericType#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#7)]  
  
7. フィールドを定義します。 この例では、フィールドの型は型パラメーター `TFirst` により指定されます。 <xref:System.Reflection.Emit.GenericTypeParameterBuilder> は <xref:System.Type> から派生します。つまり、ジェネリック型パラメーターは、型を利用できるあらゆる場所で利用できます。  
  
     [!code-cpp[EmitGenericType#21](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#21)]
     [!code-csharp[EmitGenericType#21](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#21)]
     [!code-vb[EmitGenericType#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#21)]  
  
8. ジェネリック型の型パラメーターを使用するメソッドを定義します。 そのようなメソッドは、独自の型パラメーター リストが与えられない限り、ジェネリックにならないことに注意してください。 次のコードでは、`TFirst` の配列を受け取り、その配列のすべての要素を含む `List<TFirst>` (Visual Basic の場合は `List(Of TFirst)`) を返す `static` メソッド (Visual Basic の場合は `Shared`) が定義されます。 このメソッドを定義するには、ジェネリック型定義の `List<T>` で <xref:System.Type.MakeGenericType%2A> を呼び出し、型 `List<TFirst>` を作成する必要があります。 (Visual Basic で`GetType` `typeof` 演算子を使用してジェネリック型の定義を取得すると、`T` は省略されます)。パラメーターの型は、<xref:System.Type.MakeArrayType%2A> メソッドを使用して作成されます。  
  
     [!code-cpp[EmitGenericType#22](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#22)]
     [!code-csharp[EmitGenericType#22](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#22)]
     [!code-vb[EmitGenericType#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#22)]  
  
9. メソッド本体を出力します。 メソッド本体は 3 つのオペコードで構成されます。それらのオペコードにより、入力配列をスタックにロードし、`IEnumerable<TFirst>` (入力要素をリストに入れるためのあらゆる作業を行う) を受け取る `List<TFirst>` コンストラクターを呼び出して戻る (スタックに新しい <xref:System.Collections.Generic.List%601> オブジェクトを残す) という動作が行われます。 このコードの出力の難しい部分は、コンストラクターの取得です。  
  
     <xref:System.Type.GetConstructor%2A> メソッドは <xref:System.Reflection.Emit.GenericTypeParameterBuilder> ではサポートされていません。そのため、`List<TFirst>` のコンストラクターを直接取得することはできません。 最初に、ジェネリック型定義 `List<T>` のコンストラクターを取得し、`List<TFirst>` の該当コンストラクターにそれを変換するメソッドを呼び出す必要があります。  
  
     このコード例で使用されているコンストラクターでは `IEnumerable<T>` が受け取られています。 ただし、これは <xref:System.Collections.Generic.IEnumerable%601> ジェネリック インターフェイスのジェネリック型定義ではありません。`IEnumerable<T>` の型パラメーター `T` に対して `List<T>` の型パラメーター `T` を代入する必要があります。 (これが紛らわしく見えるのは、いずれの型にも `T` という名前の型パラメーターが与えられているためです。 そのため、このコード例では `TFirst` と `TSecond`の名前を使用しています)。コンストラクターの引数の型を取得するには、`IEnumerable<T>` ジェネリック型の定義から開始し、`List<T>`の最初のジェネリック型パラメーターを使用して <xref:System.Type.MakeGenericType%2A> を呼び出します。 コンストラクター引数リストは配列として渡す必要があります。この例では引数が 1 つだけです。  
  
    > [!NOTE]
    > C# で `typeof` 演算子を使用するときは `IEnumerable(Of )` として、または Visual Basic で `GetType` 演算子を使用するときは `IEnumerable<>` としてジェネリック型定義が表されます。  
  
     これで、ジェネリック型定義で <xref:System.Type.GetConstructor%2A> を呼び出し、`List<T>` のコンストラクターを取得できます。 このコンストラクターを `List<TFirst>` の該当コンストラクターに変換するために、`List<T>` から静的 <xref:System.Reflection.Emit.TypeBuilder.GetConstructor%28System.Type%2CSystem.Reflection.ConstructorInfo%29?displayProperty=nameWithType> メソッドに `List<TFirst>` とコンストラクターを渡します。  
  
     [!code-cpp[EmitGenericType#23](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#23)]
     [!code-csharp[EmitGenericType#23](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#23)]
     [!code-vb[EmitGenericType#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#23)]  
  
10. 型を作成し、ファイルを保存します。  
  
     [!code-cpp[EmitGenericType#8](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#8)]
     [!code-csharp[EmitGenericType#8](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#8)]
     [!code-vb[EmitGenericType#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#8)]  
  
11. メソッドを呼び出します。 `ExampleMethod` はジェネリックではありませんが、それが属する型がジェネリックです。そのため、呼び出しが可能な <xref:System.Reflection.MethodInfo> を取得するには、`Sample` の型定義から、構成された型を作成する必要があります。 この構成された型は `TFirst` の制約を満たす `Example` クラスを使用します。それが参照型であり、既定のパラメーターなしのコンストラクターが与えられているためです。また、`TSecond` の制約を満たす `ExampleDerived` クラスを使用します。 (`ExampleDerived` のコードについては、「コード例」のセクションを参照してください)。これらの2つの型は、構築された型を作成するために <xref:System.Type.MakeGenericType%2A> に渡されます。 その後、<xref:System.Type.GetMethod%2A> メソッドを利用して <xref:System.Reflection.MethodInfo> が取得されます。  
  
     [!code-cpp[EmitGenericType#9](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#9)]
     [!code-csharp[EmitGenericType#9](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#9)]
     [!code-vb[EmitGenericType#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#9)]  
  
12. 次のコードでは、`Example` オブジェクトの配列が作成され、呼び出すメソッドの引数を表す型 <xref:System.Object> の配列にその配列が置かれ、<xref:System.Reflection.MethodBase.Invoke%28System.Object%2CSystem.Object%5B%5D%29> メソッドに渡されます。 <xref:System.Reflection.MethodBase.Invoke%2A> メソッドの最初の引数は null 参照です。メソッドが `static` であるためです。  
  
     [!code-cpp[EmitGenericType#10](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#10)]
     [!code-csharp[EmitGenericType#10](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#10)]
     [!code-vb[EmitGenericType#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#10)]  
  
## <a name="example"></a>例  
 次のコード例では、`Sample` という名前のクラス、基底クラス、2 つのインターフェイスが定義されています。 このプログラムでは、`Sample` の 2 つのジェネリック型パラメーターが定義され、ジェネリック型に変換されます。 型をジェネリックにするのは、型パラメーターだけです。 このプログラムではそれを、型パラメーターの定義の前後にテスト メッセージが表示されていることで確認できます。  
  
 型パラメーター `TSecond` は、基底クラスとインターフェイスを利用し、クラスとインターフェイスの制約を示すために使用されます。型パラメーター `TFirst` は特殊な制約を示すために使用されます。  
  
 このコード例では、フィールド型とメソッドのパラメーター/戻り値の型に対し、クラスの型パラメーターを利用してフィールドとメソッドが定義されます。  
  
 `Sample` クラスの作成後、メソッドが呼び出されます。  
  
 このプログラムには、ジェネリック型に関する情報を一覧表示するメソッドと、ある型パラメーターに対する特殊な制約を一覧表示するメソッドが含まれます。 完成した `Sample` クラスに関する情報の表示にこれらのメソッドが使用されます。  
  
 このプログラムでは、完成したモジュールが `GenericEmitExample1.dll` としてディスクに保存されます。そのため、[Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md) で開き、`Sample` クラスの MSIL を調べることができます。  
  
 [!code-cpp[EmitGenericType#1](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#1)]
 [!code-csharp[EmitGenericType#1](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#1)]
 [!code-vb[EmitGenericType#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#1)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Reflection.Emit.GenericTypeParameterBuilder>
- [リフレクション出力の使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y322t50(v=vs.100))
- [リフレクション出力による動的アセンブリのシナリオ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/tt9483fk(v=vs.100))
