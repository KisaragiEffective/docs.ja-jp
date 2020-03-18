---
title: .NET Framework ガイドラインに準拠したイベントを発行する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- events [C#], implementation guidelines
ms.assetid: 9310ae16-8627-44a2-b08c-05e5976202b1
ms.openlocfilehash: 90c079b9f7dbf2a1d963b7eee4447145d7a10432
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79167536"
---
# <a name="how-to-publish-events-that-conform-to-net-framework-guidelines-c-programming-guide"></a>.NET Framework ガイドラインに準拠したイベントを発行する方法 (C# プログラミング ガイド)
ここでは、.NET Framework の標準のパターンに従うイベントをクラスおよび構造体に追加する方法について説明します。 .NET Framework クラス ライブラリ内のすべてのイベントは、次のように定義されている <xref:System.EventHandler> デリゲートに基づいています。  
  
```csharp  
public delegate void EventHandler(object sender, EventArgs e);  
```  
  
> [!NOTE]
> .NET Framework 2.0 には、このデリゲートのジェネリック バージョンである <xref:System.EventHandler%601> が導入されています。 次の例は、両方のバージョンの使用方法を示しています。  
  
 ユーザー定義のクラス内のイベントは、値を返すデリゲートを含む、あらゆる有効なデリゲートに基づいて発行できますが、一般的には、次の例のように <xref:System.EventHandler> を使用して、.NET Framework のパターンに基づいて発行することをお勧めします。  
  
### <a name="to-publish-events-based-on-the-eventhandler-pattern"></a>EventHandler パターンに基づいてイベントを発行するには  
  
1. (イベントと共にカスタム データを送信する必要がない場合は、この手順を省略して手順 3a. に進んでください。)パブリッシャー クラスとサブスクライバー クラスの両方から参照できるスコープで、カスタム データのクラスを宣言します。 次に、カスタム イベント データを保持する必須メンバーを追加します。 この例では、単純な文字列が 1 つ返されます。  
  
    ```csharp  
    public class CustomEventArgs : EventArgs  
    {  
        public CustomEventArgs(string s)  
        {  
            msg = s;  
        }  
        private string msg;  
        public string Message  
        {  
            get { return msg; }  
        }
    }  
    ```  
  
2. (ジェネリック バージョンの <xref:System.EventHandler%601> を使用する場合、この手順は省略します。)パブリッシャー クラスでデリゲートを宣言します。 *EventHandler* で終わる名前を指定します。 2 番目のパラメーターで、カスタムの EventArgs 型を指定します。  
  
    ```csharp  
    public delegate void CustomEventHandler(object sender, CustomEventArgs a);  
    ```  
  
3. 次のいずれかの手順を使用して、パブリッシャー クラスでイベントを宣言します。  
  
    1. カスタムの EventArgs クラスがない場合、Event 型は非ジェネリック バージョンの EventHandler デリゲートになります。 このデリゲートは、C# プロジェクトを作成したときに含まれている <xref:System> 名前空間で既に宣言されているため、ここで宣言する必要はありません。 パブリッシャー クラスに次のコードを追加します。  
  
        ```csharp  
        public event EventHandler RaiseCustomEvent;  
        ```  
  
    2. 非ジェネリック バージョンの <xref:System.EventHandler> を使用し、<xref:System.EventArgs> から派生したカスタム クラスがある場合は、パブリッシャー クラス内でイベントを宣言し、手順 2 のデリゲートを型として使用します。  
  
        ```csharp  
        public event CustomEventHandler RaiseCustomEvent;  
        ```  
  
    3. ジェネリック バージョンを使用する場合、カスタム デリゲートは不要です。 代わりに、パブリッシャー クラスでイベントの種類として `EventHandler<CustomEventArgs>` を指定します。山かっこの部分は、実際のクラス名で置き換えます。  
  
        ```csharp  
        public event EventHandler<CustomEventArgs> RaiseCustomEvent;  
        ```  
  
## <a name="example"></a>例  
 次の例は、前の手順を具体的に示しています。ここでは、カスタムの EventArgs クラスを使用し、イベントの種類として <xref:System.EventHandler%601> を使用しています。  
  
 [!code-csharp[csProgGuideEvents#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideEvents/CS/Events.cs#2)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Delegate>
- [C# プログラミング ガイド](../index.md)
- [イベント](./index.md)
- [デリゲート](../delegates/index.md)
