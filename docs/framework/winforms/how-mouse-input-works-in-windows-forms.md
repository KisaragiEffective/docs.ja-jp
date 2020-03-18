---
title: マウス入力のしくみ
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, mouse input
- mouse [Windows Forms], input
ms.assetid: 48fc5240-75a6-44bf-9fce-6aa21b49705a
ms.openlocfilehash: 1164974e65ca1e96c0650569626ad4140baf004e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739627"
---
# <a name="how-mouse-input-works-in-windows-forms"></a>Windows フォームにおけるマウス入力のしくみ
マウス入力の受信と処理は、すべての Windows アプリケーションの重要な部分です。 マウスイベントを処理してアプリケーションでアクションを実行したり、マウスの位置情報を使用してヒットテストやその他のアクションを実行したりすることができます。 また、アプリケーション内のコントロールがマウス入力を処理する方法を変更することもできます。 このトピックでは、これらのマウスイベントの詳細と、マウスのシステム設定を取得および変更する方法について説明します。 マウスイベントによって提供されるデータと、マウスクリックイベントが発生する順序の詳細については、「 [Windows フォームのマウスイベント](mouse-events-in-windows-forms.md)」を参照してください。  
  
## <a name="mouse-location-and-hit-testing"></a>マウスの位置とヒットテスト  
 ユーザーがマウスを動かすと、オペレーティングシステムはマウスポインターを移動します。 マウスポインターには、ホットスポットと呼ばれる1つのピクセルが含まれています。これは、オペレーティングシステムがポインターの位置として追跡し、認識します。 ユーザーがマウスを動かすか、マウスボタンを押すと、<xref:System.Windows.Forms.Cursor.HotSpot%2A> を含む <xref:System.Windows.Forms.Control> によって適切なマウスイベントが発生します。 マウスイベントを処理するとき、または <xref:System.Windows.Forms.Cursor> クラスの <xref:System.Windows.Forms.Cursor.Position%2A> プロパティを使用して、<xref:System.Windows.Forms.MouseEventArgs> の <xref:System.Windows.Forms.MouseEventArgs.Location%2A> プロパティを使用して、現在のマウス位置を取得できます。 その後、マウスの位置情報を使用してヒットテストを実行し、マウスの位置に基づいてアクションを実行できます。 ヒットテスト機能は、<xref:System.Windows.Forms.ListView>、<xref:System.Windows.Forms.TreeView>、<xref:System.Windows.Forms.MonthCalendar>、<xref:System.Windows.Forms.DataGridView> コントロールなどの Windows フォームのいくつかのコントロールに組み込まれています。 適切なマウスイベントと共に使用されます。たとえば、ヒットテストは、アプリケーションが特定のアクションを実行する必要があるかどうかを判断するのに非常に役立ちます。 <xref:System.Windows.Forms.Control.MouseHover>。  
  
## <a name="mouse-events"></a>マウス イベント  
 マウス入力に応答する主な方法は、マウスイベントを処理することです。 次の表は、マウスイベントと、そのイベントが発生したときの説明を示しています。  
  
|マウス イベント|[説明]|  
|-----------------|-----------------|  
|<xref:System.Windows.Forms.Control.Click>|このイベントは、マウスボタンが離されたとき (通常は <xref:System.Windows.Forms.Control.MouseUp> イベントの前) に発生します。 このイベントのハンドラーは、型 <xref:System.EventArgs> の引数を受け取ります。 このイベントは、クリックがいつ発生するかを判断する必要がある場合にのみ処理します。|  
|<xref:System.Windows.Forms.Control.MouseClick>|このイベントは、ユーザーがマウスでコントロールをクリックしたときに発生します。 このイベントのハンドラーは、型 <xref:System.Windows.Forms.MouseEventArgs> の引数を受け取ります。 クリックが発生したときにマウスに関する情報を取得する必要がある場合に、このイベントを処理します。|  
|<xref:System.Windows.Forms.Control.DoubleClick>|このイベントは、コントロールがダブルクリックされたときに発生します。 このイベントのハンドラーは、型 <xref:System.EventArgs> の引数を受け取ります。 このイベントは、ダブルクリックがいつ発生するかを判断する必要がある場合にのみ処理します。|  
|<xref:System.Windows.Forms.Control.MouseDoubleClick>|このイベントは、ユーザーがマウスでコントロールをダブルクリックしたときに発生します。 このイベントのハンドラーは、型 <xref:System.Windows.Forms.MouseEventArgs> の引数を受け取ります。 ダブルクリックが発生したときにマウスに関する情報を取得する必要がある場合に、このイベントを処理します。|  
|<xref:System.Windows.Forms.Control.MouseDown>|このイベントは、マウスポインターがコントロール上にあり、ユーザーがマウスボタンを押すと発生します。 このイベントのハンドラーは、型 <xref:System.Windows.Forms.MouseEventArgs> の引数を受け取ります。|  
|<xref:System.Windows.Forms.Control.MouseEnter>|このイベントは、コントロールの種類に応じて、マウスポインターがコントロールの境界線またはクライアント領域に入ったときに発生します。 このイベントのハンドラーは、型 <xref:System.EventArgs> の引数を受け取ります。|  
|<xref:System.Windows.Forms.Control.MouseHover>|このイベントは、マウスポインターがコントロールの上に置かれたときに発生します。 このイベントのハンドラーは、型 <xref:System.EventArgs> の引数を受け取ります。|  
|<xref:System.Windows.Forms.Control.MouseLeave>|このイベントは、コントロールの種類に応じて、マウスポインターがコントロールの境界線またはクライアント領域から離れると発生します。 このイベントのハンドラーは、型 <xref:System.EventArgs> の引数を受け取ります。|  
|<xref:System.Windows.Forms.Control.MouseMove>|このイベントは、マウスポインターがコントロール上に移動したときに発生します。 このイベントのハンドラーは、型 <xref:System.Windows.Forms.MouseEventArgs> の引数を受け取ります。|  
|<xref:System.Windows.Forms.Control.MouseUp>|このイベントは、マウスポインターがコントロール上にあり、ユーザーがマウスボタンを離すと発生します。 このイベントのハンドラーは、型 <xref:System.Windows.Forms.MouseEventArgs> の引数を受け取ります。|  
|<xref:System.Windows.Forms.Control.MouseWheel>|このイベントは、コントロールにフォーカスがあるときにユーザーがマウスホイールを回転させたときに発生します。 このイベントのハンドラーは、型 <xref:System.Windows.Forms.MouseEventArgs> の引数を受け取ります。 <xref:System.Windows.Forms.MouseEventArgs> の <xref:System.Windows.Forms.MouseEventArgs.Delta%2A> プロパティを使用して、マウスのスクロール距離を決定できます。|  
  
## <a name="changing-mouse-input-and-detecting-system-settings"></a>マウス入力の変更とシステム設定の検出  
 コントロールから派生させ、<xref:System.Windows.Forms.Control.GetStyle%2A> および <xref:System.Windows.Forms.Control.SetStyle%2A> メソッドを使用して、コントロールがマウス入力を処理する方法を検出および変更できます。 <xref:System.Windows.Forms.Control.SetStyle%2A> メソッドは、<xref:System.Windows.Forms.ControlStyles> 値のビットごとの組み合わせを取得して、コントロールが標準のクリックまたはダブルクリックの動作を持つかどうか、またはコントロールが独自のマウス処理を処理するかどうかを判断します。 また、<xref:System.Windows.Forms.SystemInformation> クラスには、マウスの機能を説明するプロパティが含まれており、マウスがオペレーティングシステムとどのように対話するかを指定します。 次の表は、これらのプロパティをまとめたものです。  
  
|プロパティ|[説明]|  
|--------------|-----------------|  
|<xref:System.Windows.Forms.SystemInformation.DoubleClickSize%2A>|2回のクリックがダブルクリックであることをオペレーティングシステムが考慮するために、ユーザーが2回クリックする必要がある領域のサイズ (ピクセル単位) を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.DoubleClickTime%2A>|オペレーティングシステムがマウス操作をダブルクリックすることを考慮するために、最初のクリックから2回目のクリックまでに経過する最大時間 (ミリ秒単位) を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseButtons%2A>|マウスのボタンの数を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseButtonsSwapped%2A>|左右のマウス ボタンの機能が入れ替わっているかどうかを示す値を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseHoverSize%2A>|マウス静止メッセージが生成されるためにマウス静止時間が経過するまでマウス ポインターをとどめておく必要がある四角形の領域のサイズ (ピクセル単位) を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseHoverTime%2A>|マウス静止メッセージが生成されるために静止領域内にマウス ポインターをとどめておく必要がある時間 (ミリ秒単位) を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MousePresent%2A>|マウスが取り付けられているかどうかを示す値を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseSpeed%2A>|現在のマウスの速度 (1 ~ 20) を示す値を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseWheelPresent%2A>|マウス ホイール付きのマウスが取り付けられているかどうかを示す値を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseWheelScrollDelta%2A>|1つのマウスホイールの回転の増分値の量を取得します。|  
|<xref:System.Windows.Forms.SystemInformation.MouseWheelScrollLines%2A>|マウス ホイールを回転したときにスクロールする行数を取得します。|  
  
## <a name="see-also"></a>参照

- [Windows フォーム アプリケーションにおけるマウス入力](mouse-input-in-a-windows-forms-application.md)
- [Windows フォームにおけるマウスのキャプチャ](mouse-capture-in-windows-forms.md)
- [Windows フォームにおけるマウス ポインター](mouse-pointers-in-windows-forms.md)
