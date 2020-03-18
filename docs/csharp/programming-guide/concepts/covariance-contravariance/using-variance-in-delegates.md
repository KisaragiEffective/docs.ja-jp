---
title: デリゲートの変性の使用 (C#)
ms.date: 07/20/2015
ms.assetid: 1638c95d-dc8b-40c1-972c-c2dcf84be55e
ms.openlocfilehash: 83e86e760edb17f6d9ae61864c154062d41416e4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79169767"
---
# <a name="using-variance-in-delegates-c"></a>デリゲートの変性の使用 (C#)
メソッドをデリゲートに割り当てると、"*共変性*" と "*反変性*" により、デリゲート型をメソッドのシグネチャに柔軟に一致させることができます。 共変性により、メソッドの戻り値の型の派生を、デリゲートに定義されている型よりも強くできます。 また、反変性により、メソッドのパラメーター型の派生をデリゲート型よりも弱くできます。  
  
## <a name="example-1-covariance"></a>例 1: 共変性  
  
### <a name="description"></a>[説明]  
 この例は、デリゲート シグネチャ内の戻り値の型から派生した戻り値の型を持つメソッドで、デリゲートをどのように使用できるかを示しています。 `DogsHandler` が返すデータ型は `Dogs` です。これは、デリゲートに定義された `Mammals` 型の派生型です。  
  
### <a name="code"></a>コード  
  
```csharp  
class Mammals {}  
class Dogs : Mammals {}  
  
class Program  
{  
    // Define the delegate.  
    public delegate Mammals HandlerMethod();  
  
    public static Mammals MammalsHandler()  
    {  
        return null;  
    }  
  
    public static Dogs DogsHandler()  
    {  
        return null;  
    }  
  
    static void Test()  
    {  
        HandlerMethod handlerMammals = MammalsHandler;  
  
        // Covariance enables this assignment.  
        HandlerMethod handlerDogs = DogsHandler;  
    }  
}  
```  
  
## <a name="example-2-contravariance"></a>例 2: 反変性  
  
### <a name="description"></a>[説明]

この例は、型がデリゲート シグネチャ パラメーター型の基本データ型であるパラメーターを持つメソッドでデリゲートを使用する方法を示しています。 反変性により、複数のハンドラーの代わりに単一のイベント ハンドラーを使用できます。 次の例では、2 つのデリゲートを使用します。

- <xref:System.Windows.Forms.KeyEventHandler>Button.KeyDown[ イベントのシグネチャを定義する ](xref:System.Windows.Forms.Control.KeyDown) デリゲート。 そのシグネチャ:

   ```csharp
   public delegate void KeyEventHandler(object sender, KeyEventArgs e)
   ```

- <xref:System.Windows.Forms.MouseEventHandler>Button.MouseClick[ イベントのシグネチャを定義する ](xref:System.Windows.Forms.Control.MouseDown) デリゲート。 そのシグネチャ:

   ```csharp
   public delegate void MouseEventHandler(object sender, MouseEventArgs e)
   ```

この例では、イベント ハンドラーと <xref:System.EventArgs> パラメーターが定義され、そのパラメーターを使用し、`Button.KeyDown` と `Button.MouseClick` の両方のイベントが処理されます。 これが可能になるのは、<xref:System.EventArgs> は <xref:System.Windows.Forms.KeyEventArgs> と <xref:System.Windows.Forms.MouseEventArgs> の両方の基本データ型であるためです。
  
### <a name="code"></a>コード  
  
```csharp  
// Event handler that accepts a parameter of the EventArgs type.  
private void MultiHandler(object sender, System.EventArgs e)  
{  
    label1.Text = System.DateTime.Now.ToString();  
}  
  
public Form1()  
{  
    InitializeComponent();  
  
    // You can use a method that has an EventArgs parameter,  
    // although the event expects the KeyEventArgs parameter.  
    this.button1.KeyDown += this.MultiHandler;  
  
    // You can use the same method
    // for an event that expects the MouseEventArgs parameter.  
    this.button1.MouseClick += this.MultiHandler;  
  
}  
```  
  
## <a name="see-also"></a>参照

- [デリゲートの変性 (C#)](./variance-in-delegates.md)
- [Func および Action 汎用デリゲートでの分散の使用 (C#)](./using-variance-for-func-and-action-generic-delegates.md)
