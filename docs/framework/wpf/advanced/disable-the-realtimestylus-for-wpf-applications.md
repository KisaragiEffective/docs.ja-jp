---
title: RealTimeStylus を無効にする
ms.date: 03/30/2017
ms.assetid: e0525309-5ede-4782-837d-dbf6e5554859
ms.openlocfilehash: 74145c32af7e9ebbc774a0301e205aa1eb1539b3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737941"
---
# <a name="disable-the-realtimestylus-for-wpf-applications"></a>WPF アプリケーションのリアルタイムなスタイラス入力を無効にする

Windows Presentation Foundation (WPF) には、Windows 7 タッチ入力を処理するためのサポートが組み込まれています。 サポートは、<xref:System.Windows.UIElement.OnStylusDown%2A>、<xref:System.Windows.UIElement.OnStylusUp%2A>、および <xref:System.Windows.UIElement.OnStylusMove%2A> イベントとしてのタブレットプラットフォームのリアルタイムスタイラス入力を通じて行われます。 Windows 7 では、マルチタッチ入力も Win32 WM_TOUCH ウィンドウメッセージとして提供されます。 これらの2つの Api は、同じ HWND で相互に排他的です。 タブレットプラットフォーム (WPF アプリケーションの既定値) を使用してタッチ入力を有効にすると、WM_TOUCH メッセージが無効になります。 そのため、WM_TOUCH を使用して WPF ウィンドウからタッチメッセージを受信するには、WPF の組み込みのスタイラスサポートを無効にする必要があります。 これは、WM_TOUCH を使用するコンポーネントをホストする WPF ウィンドウなどのシナリオに適用されます。  
  
 スタイラス入力をリッスンしている WPF を無効にするには、WPF ウィンドウによって追加されたすべてのタブレットサポートを削除します。  
  
## <a name="example"></a>例  
 次のサンプルコードは、リフレクションを使用して既定のタブレットプラットフォームのサポートを削除する方法を示しています。  
  
```csharp  
public static void DisableWPFTabletSupport()  
{  
    // Get a collection of the tablet devices for this window.    
    TabletDeviceCollection devices = System.Windows.Input.Tablet.TabletDevices;  
  
    if (devices.Count > 0)  
    {     
        // Get the Type of InputManager.  
        Type inputManagerType = typeof(System.Windows.Input.InputManager);  
  
        // Call the StylusLogic method on the InputManager.Current instance.  
        object stylusLogic = inputManagerType.InvokeMember("StylusLogic",  
                    BindingFlags.GetProperty | BindingFlags.Instance | BindingFlags.NonPublic,  
                    null, InputManager.Current, null);  
  
        if (stylusLogic != null)  
        {  
            //  Get the type of the stylusLogic returned from the call to StylusLogic.  
            Type stylusLogicType = stylusLogic.GetType();  
  
            // Loop until there are no more devices to remove.  
            while (devices.Count > 0)  
            {  
                // Remove the first tablet device in the devices collection.  
                stylusLogicType.InvokeMember("OnTabletRemoved",  
                        BindingFlags.InvokeMethod | BindingFlags.Instance | BindingFlags.NonPublic,  
                        null, stylusLogic, new object[] { (uint)0 });  
            }                  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>参照

- [スタイラスからの入力のインターセプト](intercepting-input-from-the-stylus.md)
