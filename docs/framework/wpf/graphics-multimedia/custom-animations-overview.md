---
title: カスタム アニメーションの概要
ms.date: 03/30/2017
helpviewer_keywords:
- custom classes [WPF], animation
- key frames [WPF], custom
- custom key frames [WPF]
- animation [WPF], custom classes
- custom animation classes [WPF]
ms.assetid: 9be69d50-3384-4938-886f-08ce00e4a7a6
ms.openlocfilehash: 5a9ca51b87f73a24b90d0bd843ee306f8a1b970a
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453092"
---
# <a name="custom-animations-overview"></a>カスタム アニメーションの概要
このトピックでは、カスタム キー フレームやアニメーション クラスを作成して、またはフレームごとのコールバックを使ってバイパスすることにより、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のアニメーション システムを拡張する方法と、それが必要な状況について説明します。  
  
<a name="prerequisites"></a>   
## <a name="prerequisites"></a>前提条件  
 このトピックを理解するには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] によって提供されるさまざまな種類のアニメーションに精通している必要があります。 詳しくは、「From/To/By アニメーションの概要」、「[キー フレーム アニメーションの概要](key-frame-animations-overview.md)」、および「[パス アニメーションの概要](path-animations-overview.md)」をご覧ください。  
  
 アニメーションクラスは <xref:System.Windows.Freezable> クラスから継承するため、<xref:System.Windows.Freezable> オブジェクトと <xref:System.Windows.Freezable>から継承する方法について理解しておく必要があります。 詳しくは、「[Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」をご覧ください。  
  
<a name="extendingtheanimationsystem"></a>   
## <a name="extending-the-animation-system"></a>アニメーション システムの拡張  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のアニメーション システムには、使う組み込み機能のレベルに応じて、さまざまな拡張方法があります。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のアニメーション エンジンには、次の 3 つの主な機能拡張ポイントがあります。  
  
- <xref:System.Windows.Media.Animation.DoubleKeyFrame>などのいずれかの *\<型 >* キーフレームクラスから継承することによって、カスタムキーフレームオブジェクトを作成します。 この方法では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーション エンジンの組み込み機能のほとんどを使います。  
  
- <xref:System.Windows.Media.Animation.AnimationTimeline> から継承するか、または *\<型*の1つである > animation 基底クラスを継承することによって、独自のアニメーションクラスを作成します。  
  
- フレームごとのコールバックを使って、フレームごとにアニメーションを生成します。 この方法は、アニメーションおよびタイミング システムを完全にバイパスします。  
  
 次の表では、いくつかのアニメーション システム拡張シナリオについて説明します。  
  
|目的...|使う方法|  
|-------------------------|-----------------------|  
|対応する *\<Type>* AnimationUsingKeyFrames がある型の値の間の補間をカスタマイズする|カスタム キー フレームを作成します。 詳しくは、「[カスタム キー フレームを作成する](#createacustomkeyframe)」セクションをご覧ください。|  
|対応する *\<Type>* Animation がある型の値の間の補間以外の部分もカスタマイズする|アニメーション化する型に対応する *\<Type>* AnimationBase クラスを継承するカスタム アニメーション クラスを作成します。 詳しくは、「[カスタム アニメーション クラスを作成する](#createacustomanimationtype)」セクションをご覧ください。|  
|対応する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションがない型をアニメーション化する|<xref:System.Windows.Media.Animation.ObjectAnimationUsingKeyFrames> を使用するか、<xref:System.Windows.Media.Animation.AnimationTimeline>から継承するクラスを作成します。 詳しくは、「[カスタム アニメーション クラスを作成する](#createacustomanimationtype)」セクションをご覧ください。|  
|フレームごとに計算され、最後のオブジェクト相互作用セットに基づく値のある、複数のオブジェクトをアニメーション化する|フレームごとのコールバックを使います。 詳しくは、「[フレームごとのコールバックを使用する](#useperframecallback)」セクションをご覧ください。|  
  
<a name="createacustomkeyframe"></a>   
## <a name="create-a-custom-key-frame"></a>カスタム キー フレームを作成する  
 カスタム キー フレーム クラスの作成は、アニメーション システムを拡張する最も簡単な方法です。 キー フレーム アニメーションに対して異なる補間方法が必要なときは、このアプローチを使います。  「[キー フレーム アニメーションの概要](key-frame-animations-overview.md)」で説明されているように、キー フレーム アニメーションはキー フレーム オブジェクトを使って出力値を生成します。 各キー フレーム オブジェクトは 3 つの機能を実行します。  
  
- <xref:System.Windows.Media.Animation.IKeyFrame.Value%2A> プロパティを使用して、対象の値を指定します。  
  
- <xref:System.Windows.Media.Animation.IKeyFrame.KeyTime%2A> プロパティを使用して、その値に達する必要がある時間を指定します。  
  
- InterpolateValueCore メソッドを実装することで、前のキー フレームの値とそれ自体の値の間を補間します。  
  
 **実装の説明**  
  
 *\<Type>* KeyFrame 抽象クラスから派生して、InterpolateValueCore メソッドを実装します。 InterpolateValueCore メソッドは、キー フレームの現在の値を返します。 2 つのパラメーターとして、前のキー フレームの値と、0 から 1 の範囲の進行状況の値を受け取ります。 進行状況0はキーフレームが開始されたことを示し、値1はキーフレームが完了したことを示し、その <xref:System.Windows.Media.Animation.IKeyFrame.Value%2A> プロパティによって指定された値を返します。  
  
 キーフレームクラス *>\<の型*は <xref:System.Windows.Freezable> クラスから継承するため、クラスの新しいインスタンスを返すように <xref:System.Windows.Freezable.CreateInstanceCore%2A> コアもオーバーライドする必要があります。 クラスが依存関係プロパティを使ってデータを保存しない場合、または作成の後で追加の初期化が必要な場合は、他のメソッドのオーバーライドが必要な場合があります。詳しくは、「[Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」をご覧ください。  
  
 カスタム *\<Type>* KeyFrame アニメーションを作成した後は、その型の *\<Type>* AnimationUsingKeyFrames でそれを使うことができます。  
  
<a name="createacustomanimationtype"></a>   
## <a name="create-a-custom-animation-class"></a>カスタム アニメーション クラスを作成する  
 独自のアニメーション型を作成すると、オブジェクトをアニメーション化する方法をいっそう細かく制御できます。 独自のアニメーションの種類を作成するには、次の2つの方法をお勧めします。 <xref:System.Windows.Media.Animation.AnimationTimeline> クラス、または *\<型 >* animation 基底クラスから派生させることができます。 *\<Type>* Animation クラスまたは *\<Type>* AnimationUsingKeyFrames クラスからの派生は推奨されません。  
  
### <a name="derive-from-typeanimationbase"></a>\<Type>AnimationBase から派生する  
 *\<Type>* AnimationBase クラスからの派生は、新しいアニメーション型を作成する最も簡単な方法です。 対応する *\<Type>* AnimationBase クラスが既にある型の新しいアニメーションを作成する場合は、この方法を使います。  
  
 **実装の説明**  
  
 *\<Type>* Animation から派生して、GetCurrentValueCore メソッドを実装します。 GetCurrentValueCore メソッドは、アニメーションの現在の値を返します。 これには、推奨される開始値、提案された終了値、およびアニメーションの進行状況を判断するために使用する <xref:System.Windows.Media.Animation.AnimationClock>という3つのパラメーターがあります。  
  
 > の *\<型*は、<xref:System.Windows.Freezable> クラスから継承するため、クラスの新しいインスタンスを返すように <xref:System.Windows.Freezable.CreateInstanceCore%2A> コアもオーバーライドする必要があります。 クラスが依存関係プロパティを使ってデータを保存しない場合、または作成の後で追加の初期化が必要な場合は、他のメソッドのオーバーライドが必要な場合があります。詳しくは、「[Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」をご覧ください。  
  
 詳しくは、アニメーション化する型の *\<Type>* AnimationBase クラスの GetCurrentValueCore メソッドのドキュメントをご覧ください。 例については、「[Custom Animation Sample](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/CustomAnimation)」(カスタム アニメーションのサンプル) をご覧ください。  
  
 **別の方法**  
  
 アニメーション値を補間する方法を変更したいだけの場合は、いずれかの *\<Type>* KeyFrame クラスからの派生を検討します。 作成したキー フレームを、 *によって提供される対応する \<* Type>[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]AnimationUsingKeyFrames で使うことができます。  
  
### <a name="derive-from-animationtimeline"></a>AnimationTimeline から派生する  
 一致する [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションがまだない型のアニメーションを作成する場合、または、厳密に型指定されていないアニメーションを作成する場合は、<xref:System.Windows.Media.Animation.AnimationTimeline> クラスから派生します。  
  
 **実装の説明**  
  
 <xref:System.Windows.Media.Animation.AnimationTimeline> クラスから派生し、次のメンバーをオーバーライドします。  
  
- <xref:System.Windows.Freezable.CreateInstanceCore%2A> –新しいクラスが具象の場合は、<xref:System.Windows.Freezable.CreateInstanceCore%2A> をオーバーライドして、クラスの新しいインスタンスを返す必要があります。  
  
- <xref:System.Windows.Media.Animation.AnimationTimeline.GetCurrentValue%2A> –アニメーションの現在の値を返すには、このメソッドをオーバーライドします。 この引数には、既定のオリジン値、既定の宛先値、および <xref:System.Windows.Media.Animation.AnimationClock>の3つのパラメーターがあります。 <xref:System.Windows.Media.Animation.AnimationClock> を使用して、アニメーションの現在の時刻または進行状況を取得します。 既定の開始値と終了値を使うかどうかを選択できます。  
  
- <xref:System.Windows.Media.Animation.AnimationTimeline.IsDestinationDefault%2A> –アニメーションで <xref:System.Windows.Media.Animation.AnimationTimeline.GetCurrentValue%2A> メソッドによって指定された既定の送信先値を使用するかどうかを示すために、このプロパティをオーバーライドします。  
  
- <xref:System.Windows.Media.Animation.AnimationTimeline.TargetPropertyType%2A> –アニメーションで生成される出力の <xref:System.Type> を示すために、このプロパティをオーバーライドします。  
  
 クラスが依存関係プロパティを使ってデータを保存しない場合、または作成の後で追加の初期化が必要な場合は、他のメソッドのオーバーライドが必要な場合があります。詳しくは、「[Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)」をご覧ください。  
  
 推奨される ([!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーションによって使われる) パラダイムは、2 つの継承レベルを使うことです。  
  
1. <xref:System.Windows.Media.Animation.AnimationTimeline>から派生する、> の抽象基本クラスの抽象 *\<型*を作成します。 このクラスは、<xref:System.Windows.Media.Animation.AnimationTimeline.TargetPropertyType%2A> メソッドをオーバーライドする必要があります。 また、新しい抽象メソッド、GetCurrentValueCore、およびオーバーライド <xref:System.Windows.Media.Animation.AnimationTimeline.GetCurrentValue%2A> を導入して、既定の元の値と既定の宛先の値のパラメーターの型を検証し、GetCurrentValueCore を呼び出します。  
  
2. 新しい *\<Type >* GetCurrentValueCore 基底クラスを継承する別のクラスを作成し、<xref:System.Windows.Freezable.CreateInstanceCore%2A> メソッド、導入したメソッド、および <xref:System.Windows.Media.Animation.AnimationTimeline.IsDestinationDefault%2A> プロパティをオーバーライドします。  
  
 **別の方法**  
  
 対応する From/To/By アニメーションまたはキーフレームアニメーションがない型をアニメーション化する場合は、<xref:System.Windows.Media.Animation.ObjectAnimationUsingKeyFrames>を使用することを検討してください。 厳密に型指定されていないため、<xref:System.Windows.Media.Animation.ObjectAnimationUsingKeyFrames> は任意の型の値をアニメーション化できます。 この方法の欠点は、<xref:System.Windows.Media.Animation.ObjectAnimationUsingKeyFrames> が離散補間のみをサポートすることです。  
  
<a name="useperframecallback"></a>   
## <a name="use-per-frame-callback"></a>フレームごとのコールバックを使用する  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アニメーション システムを完全にバイパスする必要があるときは、この方法を使います。 この方法の 1 つのシナリオは、各アニメーション ステップでオブジェクトの最後の一連のやり取りに基づいてアニメーション化されるオブジェクトの新しい向きまたは位置を再計算する必要がある物理アニメーションです。  
  
 **実装の説明**  
  
 この概要で説明されている他の方法とは異なり、フレームごとのコールバックを使うために、カスタム アニメーション クラスまたはカスタム キー フレーム クラスを作成する必要はありません。  
  
 代わりに、アニメーション化するオブジェクトを含むオブジェクトの <xref:System.Windows.Media.CompositionTarget.Rendering> イベントに登録します。 このイベント ハンドラー メソッドは、フレームごとに 1 回呼び出されます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] がビジュアル ツリーの永続化されたレンダリング データを構成ツリーにマーシャリングするたびに、イベント ハンドラー メソッドが呼び出されます。  
  
 イベント ハンドラーでは、アニメーション効果に必要なあらゆる計算を実行し、これらの値を使用してアニメーション化するオブジェクトのプロパティを設定します。  
  
 現在のフレームのプレゼンテーション時間を取得するには、このイベントに関連付けられている <xref:System.EventArgs> を <xref:System.Windows.Media.RenderingEventArgs>としてキャストします。これにより、現在のフレームの表示時間を取得するために使用できる <xref:System.Windows.Media.RenderingEventArgs.RenderingTime%2A> プロパティが提供されます。  
  
 詳細については、<xref:System.Windows.Media.CompositionTarget.Rendering> のページを参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Media.Animation.AnimationTimeline>
- <xref:System.Windows.Media.Animation.IKeyFrame>
- [プロパティ アニメーションの手法の概要](property-animation-techniques-overview.md)
- [Freezable オブジェクトの概要](../advanced/freezable-objects-overview.md)
- [キー フレーム アニメーションの概要](key-frame-animations-overview.md)
- [パス アニメーションの概要](path-animations-overview.md)
- [アニメーションの概要](animation-overview.md)
- [アニメーションとタイミング システムの概要](animation-and-timing-system-overview.md)
- [カスタム アニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/CustomAnimation)
