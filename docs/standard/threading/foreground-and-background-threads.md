---
title: フォアグラウンド スレッドとバックグラウンド スレッド
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], foreground
- threading [.NET Framework], background
- foreground threads
- background threads
ms.assetid: cfe0d632-dd35-47e0-91ad-f742a444005e
ms.openlocfilehash: 9e93f07b3b84264373db0317919b6ee519c8127c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73138051"
---
# <a name="foreground-and-background-threads"></a>フォアグラウンド スレッドとバックグラウンド スレッド
マネージド スレッドは、バックグラウンド スレッドまたはフォアグラウンド スレッドのいずれかです。 バックグラウンド スレッドは、1 つの例外を除き、フォアグラウンド スレッドと同じです。その例外とは、バックグラウンド スレッドではマネージド実行環境を実行させておくことができないことです。 すべてのフォアグラウンド スレッドが (.exe ファイルがマネージド アセンブリである) マネージド プロセスで停止されると、システムはすべてのバックグラウンド スレッドを停止し、シャットダウンします。  
  
> [!NOTE]
> プロセスがシャットダウンしているため、ランタイムがバックグラウンド スレッドを停止した場合、スレッドでは例外がスローされません。 ただし、<xref:System.AppDomain.Unload%2A?displayProperty=nameWithType> メソッドがアプリケーション ドメインをアンロードしたために、スレッドが停止された場合、フォアグラウンド スレッドとバックグラウンド スレッドの両方で <xref:System.Threading.ThreadAbortException> がスローされます。  
  
 スレッドがバックグラウンド スレッドであるか、フォアグラウンド スレッドであるかを判断する場合や、その状態を変更する場合は、<xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType> プロパティを使用します。 スレッドは、<xref:System.Threading.Thread.IsBackground%2A> プロパティを `true` に設定することで、いつでもバックグラウンド スレッドに変更できます。  
  
> [!IMPORTANT]
> スレッドのフォアグラウンドまたはバックグラウンドの状態が、スレッドのハンドルされない例外の結果に影響することはありません。 .NET framework Version 2.0 では、フォアグラウンドまたはバックグラウンド スレッドのハンドルされない例外により、アプリケーションが終了します。 「[マネージド スレッドの例外](../../../docs/standard/threading/exceptions-in-managed-threads.md)」を参照してください。  
  
 マネージド スレッド プールに属するスレッド (つまり、<xref:System.Threading.Thread.IsThreadPoolThread%2A> プロパティが `true` のスレッド) はバックグラウンド スレッドです。 アンマネージド コードからマネージド実行環境に入るすべてのスレッドは、バックグラウンド スレッドとしてマークされます。 新しい <xref:System.Threading.Thread> オブジェクトを作成して開始することで生成されるすべてのスレッドは、既定でフォアグラウンド スレッドとなります。  
  
 スレッドを使用して、ソケット接続などのアクティビティを監視する場合は、その <xref:System.Threading.Thread.IsBackground%2A> プロパティを `true` に設定して、スレッドがプロセスの終了を回避しないようにします。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType>
- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadAbortException>
