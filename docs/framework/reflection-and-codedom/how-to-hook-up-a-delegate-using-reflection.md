---
title: '方法 : リフレクションを使用してデリゲートをフックする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- events [.NET Framework], adding event handlers with reflection
- reflection, adding event-handler delegates
- delegates [.NET Framework], adding event handlers with reflection
ms.assetid: 076ee62d-a964-449e-a447-c31b33518b81
ms.openlocfilehash: 14a9694708b36b23ecef453d530ad3b939a046ba
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130117"
---
# <a name="how-to-hook-up-a-delegate-using-reflection"></a>方法 : リフレクションを使用してデリゲートをフックする
リフレクションを使用して、アセンブリを読み込んで実行する場合、C# の `+=` 演算子や Visual Basic の [AddHandler ステートメント](../../visual-basic/language-reference/statements/addhandler-statement.md)のような言語機能を使用してイベントをフックすることはできません。 次の手順では、必要なすべての型をリフレクションによって取得することで、既存のメソッドをイベントにフックする方法と、リフレクション出力を使用して動的メソッドを作成し、それをイベントにフックする方法を示します。  
  
> [!NOTE]
> イベント処理デリゲートをフックするもう 1 つの方法については、<xref:System.Reflection.EventInfo> クラスの <xref:System.Reflection.EventInfo.AddEventHandler%2A> メソッドのコード例を参照してください。  
  
### <a name="to-hook-up-a-delegate-using-reflection"></a>リフレクションを使用してデリゲートをフックするには  
  
1. イベントを発生させる型を格納するアセンブリを読み込みます。 通常、アセンブリは <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> メソッドで読み込まれます。 この例を簡単にするために、現在のアセンブリの派生フォームを使用します。したがって、<xref:System.Reflection.Assembly.GetExecutingAssembly%2A> メソッドを使用して、現在のアセンブリを読み込みます。  
  
     [!code-cpp[HookUpDelegate#3](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#3)]
     [!code-csharp[HookUpDelegate#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#3)]
     [!code-vb[HookUpDelegate#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#3)]  
  
2. 型を表す <xref:System.Type> オブジェクトを取得し、型のインスタンスを作成します。 このフォームはパラメーターなしのコンストラクターを持つため、次のコードでは <xref:System.Activator.CreateInstance%28System.Type%29> メソッドを使用します。 作成する型がパラメーターなしのコンストラクターを持たない場合に使用できる、<xref:System.Activator.CreateInstance%2A> メソッドのその他のオーバーロードがいくつかあります。 アセンブリについては何もわからないという架空の状態を保つために、新しいインスタンスは <xref:System.Object> 型として格納されます (リフレクションを使用すると、事前に型名がわからなくてもアセンブリ内の型を取得できます)。  
  
     [!code-cpp[HookUpDelegate#4](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#4)]
     [!code-csharp[HookUpDelegate#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#4)]
     [!code-vb[HookUpDelegate#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#4)]  
  
3. イベントを表す <xref:System.Reflection.EventInfo> オブジェクトを取得し、<xref:System.Reflection.EventInfo.EventHandlerType%2A> プロパティを使用して、イベントの処理に使用するデリゲートの型を取得します。 次のコードでは、<xref:System.Reflection.EventInfo> イベントの <xref:System.Windows.Forms.Control.Click> が取得されます。  
  
     [!code-cpp[HookUpDelegate#5](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#5)]
     [!code-csharp[HookUpDelegate#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#5)]
     [!code-vb[HookUpDelegate#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#5)]  
  
4. イベントを処理するメソッドを表す <xref:System.Reflection.MethodInfo> オブジェクトを取得します。 このトピックで後ほど示す「使用例」セクションの完全なプログラム コードには、<xref:System.EventHandler> イベントを処理する <xref:System.Windows.Forms.Control.Click> デリゲートのシグネチャと一致するメソッドが含まれていますが、実行時に動的メソッドを生成することもできます。 詳細については、動的メソッドを使用して実行時にイベント ハンドラーを生成する場合の手順を参照してください。  
  
     [!code-cpp[HookUpDelegate#6](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#6)]
     [!code-csharp[HookUpDelegate#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#6)]
     [!code-vb[HookUpDelegate#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#6)]  
  
5. <xref:System.Delegate.CreateDelegate%2A> メソッドを使用して、デリゲートのインスタンスを作成します。 このメソッドは static (Visual Basic では `Shared`) であるため、デリゲート型を指定する必要があります。 <xref:System.Delegate.CreateDelegate%2A> を受け取る <xref:System.Reflection.MethodInfo> のオーバーロードを使用することをお勧めします。  
  
     [!code-cpp[HookUpDelegate#7](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#7)]
     [!code-csharp[HookUpDelegate#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#7)]
     [!code-vb[HookUpDelegate#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#7)]  
  
6. `add` アクセサー メソッドを取得し、このメソッドを呼び出してイベントをフックします。 すべてのイベントに、`add` アクセサーと `remove` アクセサーがあります。これらのアクセサーは高水準言語の構文によって隠ぺいされます。 たとえば、C# では `+=` 演算子を使用してイベントをフックし、Visual Basic では [AddHandler ステートメント](../../visual-basic/language-reference/statements/addhandler-statement.md)を使用します。 次のコードでは、`add` イベントの <xref:System.Windows.Forms.Control.Click> アクセサーを取得し、遅延バインディングによってこれを呼び出して、デリゲート インスタンスに渡します。 引数は配列として渡す必要があります。  
  
     [!code-cpp[HookUpDelegate#8](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#8)]
     [!code-csharp[HookUpDelegate#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#8)]
     [!code-vb[HookUpDelegate#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#8)]  
  
7. イベントをテストします。 次のコードで、このコード例で定義したフォームを表示します。 フォームをクリックすると、イベント ハンドラーが呼び出されます。  
  
     [!code-cpp[HookUpDelegate#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#12)]
     [!code-csharp[HookUpDelegate#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#12)]
     [!code-vb[HookUpDelegate#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#12)]  
  
<a name="procedureSection1"></a>   
### <a name="to-generate-an-event-handler-at-run-time-by-using-a-dynamic-method"></a>動的メソッドを使用して実行時にイベント ハンドラーを生成するには  
  
1. 軽量の動的メソッドとリフレクション出力を使用して、イベント ハンドラーのメソッドを実行時に生成できます。 イベント ハンドラーを作成するには、デリゲートの戻り値の型とパラメーターの型が必要です。 これらは、デリゲートの `Invoke` メソッドを調べることによって入手できます。 次のコードでは、`GetDelegateReturnType` メソッドと `GetDelegateParameterTypes` メソッドを使用して、この情報を取得します。 これらのメソッドのコードは、このトピックで後ほど示す「使用例」セクションにあります。  
  
     <xref:System.Reflection.Emit.DynamicMethod> に名前を付ける必要はないため、空の文字列を使用できます。 次のコードでは、最後の引数が動的メソッドを現在の型に関連付け、デリゲートが `Example` クラスのすべてのパブリック メンバーとプライベート メンバーにアクセスできるようにします。  
  
     [!code-cpp[HookUpDelegate#9](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#9)]
     [!code-csharp[HookUpDelegate#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#9)]
     [!code-vb[HookUpDelegate#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#9)]  
  
2. メソッド本体を生成します。 このメソッドは文字列を読み込み、文字列を受け取る <xref:System.Windows.Forms.MessageBox.Show%2A?displayProperty=nameWithType> メソッドのオーバーロードを呼び出します。次に、戻り値をスタックからポップし (ハンドラーには戻り値の型がないため)、返されます。 動的メソッドの出力の詳細については、「[方法: 動的メソッドを定義および実行する](how-to-define-and-execute-dynamic-methods.md)」を参照してください。  
  
     [!code-cpp[HookUpDelegate#10](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#10)]
     [!code-csharp[HookUpDelegate#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#10)]
     [!code-vb[HookUpDelegate#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#10)]  
  
3. <xref:System.Reflection.Emit.DynamicMethod.CreateDelegate%2A> メソッドを呼び出して、動的メソッドを完了します。 `add` アクセサーを使用して、イベントの呼び出しリストにデリゲートを追加します。  
  
     [!code-cpp[HookUpDelegate#11](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#11)]
     [!code-csharp[HookUpDelegate#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#11)]
     [!code-vb[HookUpDelegate#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#11)]  
  
4. イベントをテストします。 次のコードでは、このコード例で定義したフォームを読み込みます。 フォームをクリックすると、定義済みイベント ハンドラーと出力されたイベント ハンドラーの両方が呼び出されます。  
  
     [!code-cpp[HookUpDelegate#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#12)]
     [!code-csharp[HookUpDelegate#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#12)]
     [!code-vb[HookUpDelegate#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#12)]  
  
## <a name="example"></a>例  
 リフレクションを使用して既存のメソッドをイベントにフックする方法、および <xref:System.Reflection.Emit.DynamicMethod> クラスを使用してメソッドを実行時に出力し、イベントにフックする方法を次のコード例に示します。  
  
 [!code-cpp[HookUpDelegate#1](../../../samples/snippets/cpp/VS_Snippets_CLR/HookUpDelegate/cpp/source.cpp#1)]
 [!code-csharp[HookUpDelegate#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HookUpDelegate/cs/source.cs#1)]
 [!code-vb[HookUpDelegate#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HookUpDelegate/vb/source.vb#1)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Emit.DynamicMethod>
- <xref:System.Activator.CreateInstance%2A>
- <xref:System.Delegate.CreateDelegate%2A>
- [方法: 動的メソッドを定義および実行する](how-to-define-and-execute-dynamic-methods.md)
- [リフレクション](reflection.md)
