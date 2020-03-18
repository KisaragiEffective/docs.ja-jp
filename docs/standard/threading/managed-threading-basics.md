---
title: マネージド スレッド処理の基本
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- multiple threads
- threading [.NET Framework], multiple threads
- threading [.NET Framework], about threading
- managed threading
ms.assetid: b2944911-0e8f-427d-a8bb-077550618935
ms.openlocfilehash: bec769043ab630b37609bed12302ceff5b90474a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139229"
---
# <a name="managed-threading-basics"></a>マネージド スレッド処理の基本

このセクションの最初の 5 つのトピックは、マネージド スレッド処理を使用するタイミングを判断するのに役立つように設計されており、また、いくつかの基本的な機能について説明するためのものです。 その他の機能を提供するクラスについては、「[スレッド処理オブジェクトと機能](../../../docs/standard/threading/threading-objects-and-features.md)」と「[同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)」を参照してください。  
  
 このセクションの残りのトピックには、マネージド スレッド処理と Windows オペレーティング システムとの相互作用など、高度なトピックが含まれます。  
  
> [!NOTE]
> .NET Framework 4 では、タスク並列ライブラリと PLINQ によって、マルチスレッド プログラムでのタスクとデータの並列処理のための API が提供されます。 詳細については、「[並列プログラミング](../../../docs/standard/parallel-programming/index.md)」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容

 [スレッドおよびスレッド処理](../../../docs/standard/threading/threads-and-threading.md)  
 複数のスレッドの利点と欠点について説明し、スレッドを作成またはスレッド プール スレッドを使用する可能性のあるシナリオの概要を示します。  
  
 [マネージド スレッドの例外](../../../docs/standard/threading/exceptions-in-managed-threads.md)  
 さまざまなバージョンの .NET Framework のスレッドでのハンドルされていない例外の動作、特に、アプリケーションの終了の原因となる状況について説明します。  
  
 [マルチスレッド処理のためのデータの同期](../../../docs/standard/threading/synchronizing-data-for-multithreading.md)  
 複数のスレッドで使用されるクラスのデータを同期する方法について説明します。  
  
 [フォアグラウンド スレッドとバックグラウンド スレッド](../../../docs/standard/threading/foreground-and-background-threads.md)  
 フォアグラウンド スレッドとバック グラウンド スレッドの違いについて説明します。  
  
 [Windows でのマネージド スレッド処理とアンマネージド スレッド処理](../../../docs/standard/threading/managed-and-unmanaged-threading-in-windows.md)  
 マネージド スレッド処理とアンマネージド スレッド処理の関係について説明し、Windows のスレッド処理 API に相当するマネージド API をリストし、COM アパートメントとマネージド スレッドの相互作用について説明します。  
  
 [スレッド ローカル ストレージ: スレッド相対静的フィールドとデータ スロット](../../../docs/standard/threading/thread-local-storage-thread-relative-static-fields-and-data-slots.md)  
 スレッド相対ストレージ メカニズムについて説明します。  
  
## <a name="reference"></a>リファレンス

 <xref:System.Threading.Thread>  
 **Thread** クラスのリファレンス ドキュメントです。このクラスは、アンマネージド コードから作成されたか、マネージド アプリケーションで作成されたかにかかわらず、マネージド スレッドを表します。  
  
 <xref:System.ComponentModel.BackgroundWorker>  
 ユーザー インターフェイス オブジェクトと共にマルチスレッドを実装するための安全な方法を提供します。  
  
## <a name="related-sections"></a>関連項目

 [同期プリミティブの概要](../../../docs/standard/threading/overview-of-synchronization-primitives.md)  
 複数のスレッドのアクティビティを同期するために使用されるマネージド クラスについて説明します。  
  
 [マネージド スレッド処理の実施](../../../docs/standard/threading/managed-threading-best-practices.md)  
 マルチスレッドに関する一般的な問題と、問題を回避するための方法について説明します。  
  
 [並列プログラミング](../../../docs/standard/parallel-programming/index.md)  
 非同期およびマルチスレッドの .NET Framework アプリケーションを作成する作業を大幅に簡素化する、タスク並列ライブラリと PLINQ について説明します。
