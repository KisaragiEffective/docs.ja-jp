---
title: 型の動的な読み込みおよび使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- late binding, about late binding
- early binding
- dynamically loading and using types
- implicit late binding
- reflection, dynamically using types
ms.assetid: db985bec-5942-40ec-b13a-771ae98623dc
ms.openlocfilehash: 940f334ec6a42c4d8da461d634051ff979b8f98d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130263"
---
# <a name="dynamically-loading-and-using-types"></a>型の動的な読み込みおよび使用
リフレクションは、言語コンパイラで使用される、暗黙の遅延バインディングを実装するインフラストラクチャを提供します。 バインディングとは、一意に指定した型に対応する宣言 (つまり、実装) を検索するプロセスです。 このプロセスがコンパイル時ではなく、実行時に発生する場合、それは遅延バインディングと呼ばれます。 Visual Basic のコードでは、暗黙の遅延バインディングを使用できます。Visual Basic のコンパイラからは、オブジェクトの型の取得にリフレクションを使用するヘルパー メソッドが呼び出されます。 ヘルパー メソッドに渡される引数により、実行時に適切なメソッドが呼び出されます。 これらの引数は、メソッドを呼び出すインスタンス (オブジェクト)、呼び出されたメソッド名 (文字列)、呼び出されたメソッドに渡される引数 (オブジェクトの配列) です。  
  
 次の例では、Visual Basic コンパイラがリフレクションを暗黙的に使用して、コンパイル時には型が不明なオブジェクトのメソッドを呼び出します。 **HelloWorld** クラスには、**PrintHello** メソッドに渡される、"Hello World" とこれに連結されたいくつかのテキストを出力する **PrintHello** メソッドがあります。 この例で呼び出される **PrintHello** メソッドは、実際には <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType> です。Visual Basic コードでは、オブジェクトの型 (helloObj) が実行時ではなく (遅延バインディング) コンパイル時に認識された (事前バインディング) かのように **PrintHello** メソッドを呼び出します。  
  
```vb
Module Hello  
    Sub Main()  
        ' Sets up the variable.  
        Dim helloObj As Object  
        ' Creates the object.  
        helloObj = new HelloWorld()  
        ' Invokes the print method as if it was early bound  
        ' even though it is really late bound.  
        helloObj.PrintHello("Visual Basic Late Bound")  
    End Sub  
End Module  
```  
  
## <a name="custom-binding"></a>カスタム バインド  
 コンパイラが暗黙的に遅延バインディングに使用するだけではなく、コードでリフレクションを使用して、明示的に遅延バインディングを実現できます。  
  
 [共通言語ランタイム](../../standard/clr.md)では、複数のプログラミング言語をサポートしていますが、バインド規則は言語によって異なります。 事前バインディングの場合、コード ジェネレーターがバインディングを完全に制御できます。 ただし、リフレクションによる遅延バインディングでは、カスタム バインドでバインディングを制御する必要があります。 <xref:System.Reflection.Binder> クラスには、メンバーの選択と呼び出しのカスタム コントロールがあります。  
  
 カスタム バインドを使用すると、実行時にアセンブリを読み込み、そのアセンブリ内の型についての情報を取得し、必要な型を指定し、その後でメソッドを呼び出したり、その型のフィールドやプロパティにアクセスしたりできます。 この方法は、コンパイル時にオブジェクトの型を特定できない場合、たとえば、オブジェクトの型がユーザーの入力に依存するような場合に便利です。  
  
 引数の型変換のない単純なカスタム バインダーの例を次に示します。 メインの例の前には、`Simple_Type.dll` のコードを示しています。 `Simple_Type.dll` を必ず構成し、プロジェクトのビルド時にこれに対する参照を含めます。  
  
 [!code-cpp[Conceptual.Types.Dynamic#1](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.dynamic/cpp/source1.cpp#1)]
 [!code-csharp[Conceptual.Types.Dynamic#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.dynamic/cs/source1.cs#1)]
 [!code-vb[Conceptual.Types.Dynamic#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.dynamic/vb/source1.vb#1)]  
  
### <a name="invokemember-and-createinstance"></a>InvokeMember および CreateInstance  
 型のメンバーを呼び出すには、<xref:System.Type.InvokeMember%2A?displayProperty=nameWithType> を使用します。 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> や <xref:System.Reflection.Assembly.CreateInstance%2A?displayProperty=nameWithType> などのさまざまなクラスの **CreateInstance** メソッドは、指定した型の新しいインスタンスを作成する **InvokeMember** の特殊な形式のメソッドです。 **Binder** クラスは、これらのメソッドのオーバーロードの解決や引数の強制変換に使用されます。  
  
 引数の強制変換 (型変換) とメンバー選択の可能な 3 つの組み合わせの例を次に示します。 ケース 1 では、引数の強制変換、またはメンバーの選択は必要ありません。 ケース 2 では、メンバーの選択のみが必要です。 ケース 3 では、引数の強制変換のみが必要です。  
  
 [!code-cpp[Conceptual.Types.Dynamic#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.dynamic/cpp/source2.cpp#2)]
 [!code-csharp[Conceptual.Types.Dynamic#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.dynamic/cs/source2.cs#2)]
 [!code-vb[Conceptual.Types.Dynamic#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.dynamic/vb/source2.vb#2)]  
  
 オーバーロード解決は、同じ名前のメンバーを複数使用できる場合に必要です。 <xref:System.Reflection.Binder.BindToMethod%2A?displayProperty=nameWithType> メソッドと <xref:System.Reflection.Binder.BindToField%2A?displayProperty=nameWithType> メソッドは、単一のメンバーへのバインドの解決に使用します。 **Binder.BindToMethod** は **get** プロパティ アクセサーと **set** プロパティ アクセサーを使用するプロパティ解決も提供します。  
  
 **BindToMethod** は、呼び出す <xref:System.Reflection.MethodBase> を返します。このような呼び出しが不可能な場合は、null 参照 (Visual Basic では **Nothing**) を返します。 通常は含まれますが、**MethodBase** の戻り値は、*match* パラメーターに含める必要はありません。  
  
 ByRef 引数が存在する場合、呼び出し元はそれらを取得することが必要な場合があります。 したがって、**BindToMethod** が引数配列を操作した場合、クライアントは、**Binder** を使用して、引数配列を元の形態に対応付けることができます。 これを行うには、呼び出し元は引数の順序が変わらないことが保証される必要があります。 引数が名前によって渡されるとき、**Binder** は呼び出し元が参照する引数配列の順序を変更します。 詳細については、「<xref:System.Reflection.Binder.ReorderArgumentArray%2A?displayProperty=nameWithType>」を参照してください。  
  
 利用可能なメンバーのセットは、その型と任意の基本型に定義されているメンバーです。 <xref:System.Reflection.BindingFlags> を指定した場合、いずれかのアクセシビリティのメンバーがそのセットに返されます。 **BindingFlags.NonPublic** を指定しない場合、バインダーはアクセシビリティ規則を適用する必要があります。 **Public** または **NonPublic** のバインディング フラグを指定した場合、**Instance** または **Static** バインディング フラグも指定しないと、メンバーは返されません。  
  
 指定した名前のメンバーが 1 人のみの場合、コールバックの必要はなく、バインディングはそのメソッドに対して行われます。 コード例のケース 1 は、コールバックが不要な場合を示しています。使用可能な **PrintBob** メソッドが 1 つしか存在しないため、コールバックは不要です。  
  
 利用可能なセットに複数のメンバーがある場合、これらすべてのメソッドは **BindToMethod** に渡され、適切なメソッドが選択され返されます。 コード例のケース 2 では、**PrintValue** というメソッドが 2 つあります。 **BindToMethod** を呼び出すと、適切なメソッドが選択されます。  
  
 <xref:System.Reflection.Binder.ChangeType%2A> は、実引数を選択されたメソッドの仮引数の型に変換する引数の強制変換 (型変換) を行います。 **ChangeType** は、型が正確に一致した場合でも、すべての引数について呼び出されます。  
  
 コード例のケース 3 では、値が "5.5" の **String** 型の実引数が **Double** 型の仮引数のメソッドに渡されます。 呼び出しが成功するには、文字列値 "5.5" を倍精度浮動小数点値に変換する必要があります。 この変換は、**ChangeType** が実行します。  
  
 **ChangeType** は、次の表の無損失または[拡大強制型変換](../../standard/base-types/type-conversion.md)のみを行います。  
  
|変換元の型|変換後の型|  
|-----------------|-----------------|  
|任意の型|その基本型|  
|任意の型|実装するインターフェイス|  
|Char|UInt16、UInt32、Int32、UInt64、Int64、Single、Double|  
|Byte|Char、UInt16、Int16、UInt32、Int32、UInt64、Int64、Single、Double|  
|SByte|Int16、Int32、Int64、Single、Double|  
|UInt16|UInt32、Int32、UInt64、Int64、Single、Double|  
|Int16|Int32、Int64、Single、Double|  
|UInt32|UInt64、Int64、Single、Double|  
|Int32|Int64、Single、Double|  
|UInt64|Single、Double|  
|Int64|Single、Double|  
|Single|Double|  
|非参照型|参照型|  
  
 <xref:System.Type> クラスは、参照を特定のメンバーとして解決する **Binder** 型のパラメーターを使用する **get** メソッドを持っています。 <xref:System.Type.GetConstructor%2A?displayProperty=nameWithType>、<xref:System.Type.GetMethod%2A?displayProperty=nameWithType>、および <xref:System.Type.GetProperty%2A?displayProperty=nameWithType> は、現在の型の特定のメンバーに関するシグネチャ情報を提供して、そのメンバーを検索します。 該当するメソッドの指定されたシグネチャ情報を選択するために、<xref:System.Reflection.Binder.SelectMethod%2A?displayProperty=nameWithType> および <xref:System.Reflection.Binder.SelectProperty%2A?displayProperty=nameWithType> がコールバックされます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- [型情報の表示](viewing-type-information.md)
- [.NET Framework における型変換](../../standard/base-types/type-conversion.md)
