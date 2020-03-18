---
title: コントロールでのマルチスレッド
ms.date: 03/30/2017
helpviewer_keywords:
- BackgroundWorker component
- threading [Windows Forms], controls
ms.assetid: c311d652-0f26-45fa-bdcc-b1615d73ce4e
ms.openlocfilehash: 79832e12a10f02c909d2a28270594bcb4ea68656
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742143"
---
# <a name="multithreading-in-windows-forms-controls"></a>Windows フォーム コントロールのマルチスレッド処理
多くのアプリケーションでは、別のスレッドで時間のかかる操作を実行することで、ユーザーインターフェイス (UI) の応答性を高めることができます。 <xref:System.Threading> 名前空間、<xref:System.Windows.Forms.Control.BeginInvoke%2A?displayProperty=nameWithType> メソッド、`BackgroundWorker` コンポーネントなど、Windows フォームコントロールのマルチスレッドには多数のツールが用意されています。  
  
> [!NOTE]
> `BackgroundWorker` コンポーネントによって、<xref:System.Threading> 名前空間と <xref:System.Windows.Forms.Control.BeginInvoke%2A?displayProperty=nameWithType> メソッドに置き換えられ、機能が追加されます。ただし、これらは下位互換性と将来の使用の両方のために保持されます (選択した場合)。 詳細については、「 [BackgroundWorker コンポーネントの概要](backgroundworker-component-overview.md)」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法: Windows フォーム コントロールのスレッド セーフな呼び出しを行う](how-to-make-thread-safe-calls-to-windows-forms-controls.md)  
 Windows フォームコントロールに対してスレッドセーフな呼び出しを行う方法を示します。  
  
 [方法: バックグラウンド スレッドを使用してファイルを検索する](how-to-use-a-background-thread-to-search-for-files.md)  
 <xref:System.Threading> 名前空間と <xref:System.Windows.Forms.Control.BeginInvoke%2A> メソッドを使用してファイルを非同期的に検索する方法について説明します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.ComponentModel.BackgroundWorker>  
 非同期操作のワーカースレッドをカプセル化するコンポーネントについて説明します。  
  
 <xref:System.Media.SoundPlayer.LoadAsync%2A>  
 サウンドを非同期に読み込む方法を説明します。  
  
 <xref:System.Windows.Forms.PictureBox.LoadAsync%2A>  
 画像を非同期に読み込む方法を説明します。  
  
## <a name="related-sections"></a>関連項目  
 [方法: バックグラウンドで操作を実行する](how-to-run-an-operation-in-the-background.md)  
 <xref:System.ComponentModel.BackgroundWorker> コンポーネントで時間のかかる操作を実行する方法について説明します。  
  
 [BackgroundWorker コンポーネントの概要](backgroundworker-component-overview.md)  
 非同期操作に <xref:System.ComponentModel.BackgroundWorker> コンポーネントを使用する方法について説明するトピックを示します。
