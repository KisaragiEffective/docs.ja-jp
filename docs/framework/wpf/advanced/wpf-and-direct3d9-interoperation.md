---
title: WPF と Direct3D9 の相互運用
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- WPF [WPF], creating Direct3D9 content
- Direct3D9 [WPF interoperability], creating Direct3D9 content
ms.assetid: 1b14b823-69c4-4e8d-99e4-f6dade58f89a
ms.openlocfilehash: 9ec83c48052e1ef29bb91a6b40b7c76f671bb99f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735185"
---
# <a name="wpf-and-direct3d9-interoperation"></a>WPF と Direct3D9 の相互運用性
Windows Presentation Foundation (WPF) アプリケーションに Direct3D9 コンテンツを含めることができます。 このトピックでは、WPF と効率的に相互運用できるように Direct3D9 コンテンツを作成する方法について説明します。  
  
> [!NOTE]
> WPF で Direct3D9 コンテンツを使用する場合は、パフォーマンスについても考慮する必要があります。 パフォーマンスを最適化する方法の詳細については、「 [Direct3D9 と WPF の相互運用性のパフォーマンスに関する考慮事項](performance-considerations-for-direct3d9-and-wpf-interoperability.md)」を参照してください。  
  
## <a name="display-buffers"></a>バッファーの表示  
 <xref:System.Windows.Interop.D3DImage> クラスは、*バックバッファー*と*フロントバッファー*と呼ばれる2つの表示バッファーを管理します。 バックバッファーは Direct3D9 表面です。 <xref:System.Windows.Interop.D3DImage.Unlock%2A> メソッドを呼び出すと、バックバッファーへの変更がフロントバッファーにコピーされます。  
  
 次の図は、バックバッファーとフロントバッファーの関係を示しています。  
  
 ![D3DImage の表示バッファー](./media/d3dimage-buffers.png "D3DImage_buffers")  
  
## <a name="direct3d9-device-creation"></a>Direct3D9 デバイスの作成  
 Direct3D9 コンテンツをレンダリングするには、Direct3D9 デバイスを作成する必要があります。 デバイスの作成に使用できる Direct3D9 オブジェクトには、`IDirect3D9` と `IDirect3D9Ex`の2つがあります。 これらのオブジェクトを使用して、`IDirect3DDevice9` と `IDirect3DDevice9Ex` デバイスをそれぞれ作成します。  
  
 次のいずれかの方法を呼び出して、デバイスを作成します。  
  
- `IDirect3D9 * Direct3DCreate9(UINT SDKVersion);`  
  
- `HRESULT Direct3DCreate9Ex(UINT SDKVersion, IDirect3D9Ex **ppD3D);`  
  
 Windows Vista 以降のオペレーティングシステムでは、Windows Display Driver Model (WDDM) を使用するように構成された表示で `Direct3DCreate9Ex` メソッドを使用します。 その他のプラットフォームでは、`Direct3DCreate9` メソッドを使用します。  
  
### <a name="availability-of-the-direct3dcreate9ex-method"></a>Direct3DCreate9Ex メソッドの可用性  
 D3d9 には、Windows Vista 以降のオペレーティングシステムでのみ `Direct3DCreate9Ex` 方法があります。 Windows XP で関数を直接リンクすると、アプリケーションの読み込みに失敗します。 `Direct3DCreate9Ex` メソッドがサポートされているかどうかを判断するには、DLL を読み込み、proc アドレスを探します。 次のコードは、`Direct3DCreate9Ex` メソッドをテストする方法を示しています。 完全なコード例については、「[チュートリアル: WPF でホストするための Direct3D9 コンテンツの作成](walkthrough-creating-direct3d9-content-for-hosting-in-wpf.md)」を参照してください。  
  
 [!code-cpp[System.Windows.Interop.D3DImage#RendererManager_EnsureD3DObjects](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderermanager.cpp#renderermanager_ensured3dobjects)]  
  
### <a name="hwnd-creation"></a>HWND の作成  
 デバイスを作成するには HWND が必要です。 一般に、Direct3D9 が使用するダミーの HWND を作成します。 次のコード例は、ダミーの HWND を作成する方法を示しています。  
  
 [!code-cpp[System.Windows.Interop.D3DImage#RendererManager_EnsureHWND](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderermanager.cpp#renderermanager_ensurehwnd)]  
  
### <a name="present-parameters"></a>パラメーターを表示する  
 デバイスを作成するには `D3DPRESENT_PARAMETERS` 構造体も必要ですが、いくつかのパラメーターのみが重要です。 メモリフットプリントを最小限に抑えるために、これらのパラメーターが選択されています。  
  
 `BackBufferHeight` フィールドと `BackBufferWidth` フィールドを1に設定します。 これらを0に設定すると、HWND の大きさに設定されます。  
  
 Direct3D9 によって使用されるメモリが破損しないようにし、Direct3D9 が FPU 設定を変更しないようにするには、常に `D3DCREATE_MULTITHREADED` フラグと `D3DCREATE_FPU_PRESERVE` フラグを設定します。  
  
 次のコードは、`D3DPRESENT_PARAMETERS` 構造体を初期化する方法を示しています。  
  
 [!code-cpp[System.Windows.Interop.D3DImage#Renderer_Init](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderer.cpp#renderer_init)]  
  
## <a name="creating-the-back-buffer-render-target"></a>バックバッファーレンダーターゲットを作成しています  
 <xref:System.Windows.Interop.D3DImage>で Direct3D9 コンテンツを表示するには、Direct3D9 サーフェイスを作成し、<xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> メソッドを呼び出して割り当てます。  
  
### <a name="verifying-adapter-support"></a>アダプターサポートの検証  
 サーフェイスを作成する前に、すべてのアダプターが必要なサーフェイスプロパティをサポートしていることを確認します。 表示するアダプターが1つだけの場合でも、WPF ウィンドウはシステム内のすべてのアダプターに表示されることがあります。 マルチアダプター構成を処理する Direct3D9 コードを作成する必要があります。また、WPF が使用可能なアダプター間でサーフェイスを移動する可能性があるため、すべてのアダプターがサポートされているかどうかを確認する必要があります。  
  
 次のコード例は、システム上のすべてのアダプターの Direct3D9 サポートを確認する方法を示しています。  
  
 [!code-cpp[System.Windows.Interop.D3DImage#RendererManager_TestSurfaceSettings](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderermanager.cpp#renderermanager_testsurfacesettings)]  
  
### <a name="creating-the-surface"></a>サーフェイスの作成  
 Surface を作成する前に、デバイスの機能がターゲットのオペレーティングシステムで良好なパフォーマンスをサポートしていることを確認します。 詳細については、「 [Direct3D9 と WPF の相互運用性のパフォーマンスに関する考慮事項](performance-considerations-for-direct3d9-and-wpf-interoperability.md)」を参照してください。  
  
 デバイスの機能を確認したら、surface を作成できます。 レンダーターゲットを作成する方法を次のコード例に示します。  
  
 [!code-cpp[System.Windows.Interop.D3DImage#Renderer_CreateSurface](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderer.cpp#renderer_createsurface)]  
  
### <a name="wddm"></a>WDDM  
 WDDM を使用するように構成されている Windows Vista 以降のオペレーティングシステムでは、レンダーターゲットテクスチャを作成し、レベル0の画面を <xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> メソッドに渡すことができます。 この方法は Windows XP では推奨されません。ロック可能なレンダーターゲットテクスチャを作成することはできず、パフォーマンスが低下するためです。  
  
## <a name="handling-device-state"></a>デバイスの状態の処理  
 <xref:System.Windows.Interop.D3DImage> クラスは、*バックバッファー*と*フロントバッファー*と呼ばれる2つの表示バッファーを管理します。 バックバッファーは、Direct3D サーフェイスです。  バックバッファーに対する変更は、<xref:System.Windows.Interop.D3DImage.Unlock%2A> メソッドを呼び出したときにフロントバッファーにコピーされ、ハードウェア上に表示されます。 場合によっては、フロントバッファーが使用できなくなることがあります。 この可用性の欠如は、画面のロック、全画面の専用 Direct3D アプリケーション、ユーザー切り替え、またはその他のシステムアクティビティによって発生する可能性があります。 このエラーが発生すると、<xref:System.Windows.Interop.D3DImage.IsFrontBufferAvailableChanged> イベントを処理することによって、WPF アプリケーションに通知されます。  アプリケーションがフロントバッファーを使用できなくなった場合の応答は、WPF がソフトウェアレンダリングにフォールバックすることが有効になっているかどうかによって異なります。 <xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> メソッドには、WPF がソフトウェアレンダリングにフォールバックするかどうかを指定するパラメーターを受け取るオーバーロードがあります。  
  
 <xref:System.Windows.Interop.D3DImage.SetBackBuffer%28System.Windows.Interop.D3DResourceType%2CSystem.IntPtr%29> のオーバーロードを呼び出すか、`enableSoftwareFallback` パラメーターを `false`に設定して <xref:System.Windows.Interop.D3DImage.SetBackBuffer%28System.Windows.Interop.D3DResourceType%2CSystem.IntPtr%2CSystem.Boolean%29> のオーバーロードを呼び出すと、フロントバッファーが使用できなくなり、何も表示されないときに、レンダリングシステムによってバックバッファーへの参照が解放されます。 フロントバッファーが再び使用可能になると、レンダリングシステムによって <xref:System.Windows.Interop.D3DImage.IsFrontBufferAvailableChanged> イベントが発生し、WPF アプリケーションに通知されます。  <xref:System.Windows.Interop.D3DImage.IsFrontBufferAvailableChanged> イベントのイベントハンドラーを作成して、有効な Direct3D サーフェイスで再び描画を再開することができます。 レンダリングを再開するには、<xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A>を呼び出す必要があります。  
  
 `enableSoftwareFallback` パラメーターを `true`に設定して <xref:System.Windows.Interop.D3DImage.SetBackBuffer%28System.Windows.Interop.D3DResourceType%2CSystem.IntPtr%2CSystem.Boolean%29> オーバーロードを呼び出すと、フロントバッファーが使用できなくなったときに、レンダリングシステムはバックバッファーへの参照を保持するため、フロントバッファーが再び使用可能になったときに <xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> を呼び出す必要はありません。  
  
 ソフトウェアレンダリングが有効になっている場合、ユーザーのデバイスが使用できなくなっても、レンダリングシステムが Direct3D サーフェイスへの参照を保持している場合があります。 Direct3D9 デバイスが使用できないかどうかを確認するには、`TestCooperativeLevel` メソッドを呼び出します。 Direct3D9Ex デバイスを確認するには、`CheckDeviceState` メソッドを呼び出します。これは、`TestCooperativeLevel` メソッドが非推奨とされ、常に success が返されるためです。 ユーザーデバイスが使用できなくなった場合は、<xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> を呼び出して、WPF のバックバッファーへの参照を解放します。  デバイスをリセットする必要がある場合は、`backBuffer` パラメーターを `null`に設定して <xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> を呼び出し、`backBuffer` を有効な Direct3D サーフェイスに設定して <xref:System.Windows.Interop.D3DImage.SetBackBuffer%2A> をもう一度呼び出します。  
  
 複数アダプターサポートを実装している場合にのみ、`Reset` メソッドを呼び出して、無効なデバイスから回復します。 それ以外の場合は、すべての Direct3D9 インターフェイスを解放し、それらを完全に再作成します。 アダプターのレイアウトが変更された場合、変更前に作成された Direct3D9 オブジェクトは更新されません。  
  
## <a name="handling-resizing"></a>サイズ変更の処理  
 <xref:System.Windows.Interop.D3DImage> がネイティブサイズ以外の解像度で表示されている場合は、現在の <xref:System.Windows.Media.RenderOptions.BitmapScalingMode%2A>に従ってスケーリングされます。ただし、<xref:System.Windows.Media.Effects.SamplingMode.Bilinear> は <xref:System.Windows.Media.BitmapScalingMode.Fant>の代わりに使用されます。  
  
 再現性を高める必要がある場合は、<xref:System.Windows.Interop.D3DImage> のコンテナーのサイズが変更されたときに新しいサーフェイスを作成する必要があります。  
  
 サイズ変更を処理するには、3つの方法があります。  
  
- レイアウトシステムに参加し、サイズが変化したときに新しいサーフェイスを作成します。 ビデオメモリを消費したり、フラグメント化したりする可能性があるため、あまり多くのサーフェイスを作成しないでください。  
  
- 新しいサーフェイスを作成するために一定期間、サイズ変更イベントが発生するまで待機します。  
  
- コンテナーディメンションを1秒間に何度もチェックする <xref:System.Windows.Threading.DispatcherTimer> を作成します。  
  
## <a name="multi-monitor-optimization"></a>マルチモニターの最適化  
 レンダリングシステムによって <xref:System.Windows.Interop.D3DImage> が別のモニターに移動されると、パフォーマンスが大幅に低下する可能性があります。  
  
 WDDM では、モニターが同じビデオカード上にあり、`Direct3DCreate9Ex`を使用する限り、パフォーマンスが低下することはありません。 モニターが別のビデオカードにある場合は、パフォーマンスが低下します。 Windows XP では、パフォーマンスは常に低下します。  
  
 <xref:System.Windows.Interop.D3DImage> を別のモニターに移動すると、対応するアダプターに新しい画面を作成して、良好なパフォーマンスを復元できます。  
  
 パフォーマンスの低下を回避するには、マルチモニターケース専用のコードを記述します。 次の一覧は、マルチモニターコードを記述する1つの方法を示しています。  
  
1. `Visual.ProjectToScreen` メソッドを使用して、<xref:System.Windows.Interop.D3DImage> のポイントを画面空間で検索します。  
  
2. `MonitorFromPoint` GDI メソッドを使用して、ポイントを表示しているモニターを見つけます。  
  
3. `IDirect3D9::GetAdapterMonitor` メソッドを使用して、モニターがある Direct3D9 アダプターを見つけます。  
  
4. アダプターがバックバッファーを使用するアダプターと同じでない場合は、新しいモニターに新しいバックバッファーを作成し、それを <xref:System.Windows.Interop.D3DImage> バックバッファーに割り当てます。  
  
> [!NOTE]
> <xref:System.Windows.Interop.D3DImage> またがっが監視している場合、同じアダプター上の WDDM と `IDirect3D9Ex` の場合を除き、パフォーマンスは低下します。 このような状況では、パフォーマンスを向上させる方法はありません。  
  
 次のコード例は、現在のモニターを検索する方法を示しています。  
  
 [!code-cpp[System.Windows.Interop.D3DImage#RendererManager_SetAdapter](~/samples/snippets/cpp/VS_Snippets_Wpf/System.Windows.Interop.D3DImage/cpp/renderermanager.cpp#renderermanager_setadapter)]  
  
 <xref:System.Windows.Interop.D3DImage> コンテナーのサイズまたは位置が変更されたときにモニターを更新するか、1秒あたり数回更新される `DispatcherTimer` を使用してモニターを更新します。  
  
## <a name="wpf-software-rendering"></a>WPF のソフトウェアレンダリング  
 WPF は、次のような場合に、ソフトウェアの UI スレッド上で同期的にレンダリングされます。  
  
- 印刷  
  
- <xref:System.Windows.Media.Effects.BitmapEffect>  
  
- <xref:System.Windows.Media.Imaging.RenderTargetBitmap>  
  
 このような状況のいずれかが発生した場合、レンダリングシステムは <xref:System.Windows.Interop.D3DImage.CopyBackBuffer%2A> メソッドを呼び出して、ハードウェアバッファーをソフトウェアにコピーします。 既定の実装では、`GetRenderTargetData` メソッドがサーフェイスで呼び出されます。 この呼び出しはロック/ロック解除パターンの外側で行われるため、失敗する可能性があります。 この場合、`CopyBackBuffer` メソッドは `null` を返し、イメージは表示されません。  
  
 <xref:System.Windows.Interop.D3DImage.CopyBackBuffer%2A> メソッドをオーバーライドし、基本実装を呼び出し、`null`を返す場合は <xref:System.Windows.Media.Imaging.BitmapSource>プレースホルダーを返すことができます。  
  
 基本実装を呼び出す代わりに、独自のソフトウェアレンダリングを実装することもできます。  
  
> [!NOTE]
> WPF が完全にソフトウェアでレンダリングされている場合、WPF にフロントバッファーがないため <xref:System.Windows.Interop.D3DImage> は表示されません。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.D3DImage>
- [Direct3D9 および WPF の相互運用性のパフォーマンスに関する考慮事項](performance-considerations-for-direct3d9-and-wpf-interoperability.md)
- [チュートリアル: WPF でホストするための Direct3D9 コンテンツの作成](walkthrough-creating-direct3d9-content-for-hosting-in-wpf.md)
- [チュートリアル: WPF での Direct3D9 コンテンツのホスト](walkthrough-hosting-direct3d9-content-in-wpf.md)
