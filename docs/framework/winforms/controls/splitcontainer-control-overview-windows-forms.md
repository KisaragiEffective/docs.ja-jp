---
title: SplitContainer コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- SplitContainer
helpviewer_keywords:
- SplitContainer control [Windows Forms], about SplitContainer control
ms.assetid: 6de5a5f7-97a5-402d-be6d-7e2785483db5
ms.openlocfilehash: 5cb5240ed4ebe3e27c20844681068711c222e9a9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742922"
---
# <a name="splitcontainer-control-overview-windows-forms"></a>SplitContainer コントロールの概要 (Windows フォーム)
Windows フォームの <xref:System.Windows.Forms.SplitContainer> コントロールは複合と考えることができ、移動可能なバーで区切られた 2 つのパネルです。 マウス ポインターがバーの上に移動すると、ポインターの形が変わり、バーが移動可能であることを示します。  
  
> [!IMPORTANT]
> **ツールボックス**では、以前のバージョンの Visual Studio に含まれていた <xref:System.Windows.Forms.Splitter> コントロールが <xref:System.Windows.Forms.SplitContainer> コントロールに置き換えられます。 <xref:System.Windows.Forms.SplitContainer> コントロールは <xref:System.Windows.Forms.Splitter> コントロールより優先されます。 <xref:System.Windows.Forms.Splitter> クラスは、既存のアプリケーションの互換性のために .NET Framework に含まれていますが、新しいプロジェクトでは <xref:System.Windows.Forms.SplitContainer> コントロールを使用することを強くお勧めします。  
  
 <xref:System.Windows.Forms.SplitContainer> コントロールを使用すると、複雑なユーザーインターフェイスを作成できます。多くの場合、1つのパネルで選択すると、他のパネルに表示されるオブジェクトが決まります。 この配置は、情報の表示と参照に対して非常に効果的です。 2つのパネルを使用すると、領域内の情報を集計できます。また、バーまたは "スプリッター" を使用すると、ユーザーがパネルのサイズを簡単に変更できるようになります。  
  
 複数の <xref:System.Windows.Forms.SplitContainer> コントロールを入れ子にすることもできます。2番目の <xref:System.Windows.Forms.SplitContainer> コントロールを水平方向に配置して、上と下のパネルを作成します。  
  
 <xref:System.Windows.Forms.SplitContainer> コントロールは、既定ではキーボードからアクセスできることに注意してください。<xref:System.Windows.Forms.SplitContainer.IsSplitterFixed%2A> プロパティが `false`に設定されている場合、ユーザーは方向キーを押してスプリッターを移動できます。  
  
 <xref:System.Windows.Forms.SplitContainer> コントロールの <xref:System.Windows.Forms.SplitContainer.Orientation%2A> プロパティは、コントロール自体ではなく、スプリッターの方向を決定します。 このため、このプロパティが <xref:System.Windows.Forms.Orientation.Vertical>に設定されている場合、スプリッターは上から下に実行され、左右のパネルが作成されます。  
  
 また、<xref:System.Windows.Forms.SplitContainer.SplitterRectangle%2A> プロパティの値は、<xref:System.Windows.Forms.SplitContainer.Orientation%2A> プロパティの値によって異なることに注意してください。 詳細については、「<xref:System.Windows.Forms.SplitContainer.SplitterRectangle%2A> プロパティ」を参照してください。  
  
 <xref:System.Windows.Forms.SplitContainer> コントロールのサイズと移動を制限することもできます。 <xref:System.Windows.Forms.SplitContainer.FixedPanel%2A> プロパティによって、<xref:System.Windows.Forms.SplitContainer> コントロールのサイズが変更された後もどのパネルが同じサイズであるかが決まります。また、<xref:System.Windows.Forms.SplitContainer.IsSplitterFixed%2A> プロパティによって、分割線をキーボードまたはマウスで移動できるかどうかが決まります。  
  
> [!NOTE]
> <xref:System.Windows.Forms.SplitContainer.IsSplitterFixed%2A> プロパティが `true`に設定されている場合でも、スプリッターはプログラムによって移動される可能性があります。たとえば、<xref:System.Windows.Forms.SplitContainer.SplitterDistance%2A> プロパティを使用します。  
  
 最後に、<xref:System.Windows.Forms.SplitContainer> コントロールの各パネルには、個々のサイズを決定するプロパティがあります。  
  
## <a name="commonly-used-properties-methods-and-events"></a>一般的に使用されるプロパティ、メソッド、およびイベント  
  
|Name|[説明]|  
|----------|-----------------|  
|<xref:System.Windows.Forms.SplitContainer.FixedPanel%2A> プロパティ|<xref:System.Windows.Forms.SplitContainer> コントロールのサイズを変更した後、どのパネルが同じサイズになるかを指定します。|  
|<xref:System.Windows.Forms.SplitContainer.IsSplitterFixed%2A> プロパティ|キーボードまたはマウスを使用してスプリッターを移動できるかどうかを決定します。|  
|<xref:System.Windows.Forms.SplitContainer.Orientation%2A> プロパティ|分割線を垂直方向または水平方向のどちらに配置するかを決定します。|  
|<xref:System.Windows.Forms.SplitContainer.SplitterDistance%2A> プロパティ|左端または上端から移動可能スプリッターバーまでの距離をピクセル単位で決定します。|  
|<xref:System.Windows.Forms.SplitContainer.SplitterIncrement%2A> プロパティ|ユーザーがスプリッターを移動できる最小距離をピクセル単位で指定します。|  
|<xref:System.Windows.Forms.SplitContainer.SplitterWidth%2A> プロパティ|分割線の太さ (ピクセル単位) を決定します。|  
|<xref:System.Windows.Forms.SplitContainer.SplitterMoving> イベント|スプリッターの移動中に発生します。|  
|<xref:System.Windows.Forms.SplitContainer.SplitterMoved> イベント|スプリッターが移動したときに発生します。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.SplitContainer>
- [SplitContainer コントロール](splitcontainer-control-windows-forms.md)
- [SplitContainer コントロールのサンプル](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/0ffz7d1b(v=vs.90))
