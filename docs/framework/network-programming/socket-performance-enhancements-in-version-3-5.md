---
title: バージョン 3.5 のソケット パフォーマンスの強化
ms.date: 03/30/2017
ms.assetid: 225aa5f9-c54b-4620-ab64-5cd100cfd54c
ms.openlocfilehash: 577c033fc5639f9d9f50e413fd2cb55a75d48f2c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047238"
---
# <a name="socket-performance-enhancements-in-version-35"></a>バージョン 3.5 のソケット パフォーマンスの強化
非同期ネットワーク I/O を利用して最高のパフォーマンスを達成するために、バージョン 3.5 で、<xref:System.Net.Sockets.Socket?displayProperty=nameWithType> クラスが機能強化されました。 <xref:System.Net.Sockets.Socket> クラスの機能強化の一環として、一連の新しいクラスが追加されました。これが提供する代替非同期パターンは、目的に特化した高パフォーマンスのソケット アプリケーションで利用できます。 この機能強化は、高いパフォーマンスを必要とするネットワーク サーバー アプリケーションのために設計されたものです。 あるアプリケーションで、機能強化された非同期パターンを排他的に、言い換えると、アプリケーションの高負荷領域 (大量のデータを受け取るときなど) のみで使用できます。  
  
## <a name="class-enhancements"></a>クラスの拡張機能  
 この機能強化の主な特徴は、高ボリュームの非同期ソケット I/O 中、オブジェクトの割り当てと同期が繰り返されることを避けることにあります。 非同期ソケット I/O のために <xref:System.Net.Sockets.Socket> クラスにより現在実装されている開始/終了設計パターンでは、非同期ソケット操作のたびに、<xref:System.IAsyncResult?displayProperty=nameWithType> オブジェクトを割り当てる必要があります。  
  
 新しい <xref:System.Net.Sockets.Socket> クラス機能強化では、非同期ソケット操作は、アプリケーションにより割り当てられ、保守管理される再利用可能 <xref:System.Net.Sockets.SocketAsyncEventArgs?displayProperty=nameWithType> クラス オブジェクトによって記述されます。 高パフォーマンスのソケット アプリケーションは、維持する必要があるオーバーラップしたソケット操作の量を効率的に認識します。 アプリケーションは <xref:System.Net.Sockets.SocketAsyncEventArgs> オブジェクトを必要な数だけ作成できます。 たとえば、サーバー アプリケーションが、入ってくるクライアント接続レートに対応するために、15 のソケット受け取り操作を常に未処理状態にしておく必要がある場合、その目的のために前もって 15 の再利用可能 <xref:System.Net.Sockets.SocketAsyncEventArgs> オブジェクトを割り当てることができます。  
  
 このクラスで非同期ソケット操作を実行するパターンは次の手順で構成されます。  
  
1. 新しい <xref:System.Net.Sockets.SocketAsyncEventArgs> コンテキスト オブジェクトを割り当てるか、アプリケーション プールから空きコンテキスト オブジェクトを入手します。  
  
2. コンテキスト オブジェクトのプロパティをこれから実行する操作 (コールバック デリゲート メソッドやデータ バッファーなど) に設定します。  
  
3. 適切なソケット メソッド (xxxAsync) を呼び出し、非同期操作を開始します。  
  
4. 非同期ソケット メソッド (xxxAsync) がコールバックで true を返した場合、コンテキスト プロパティに完了状態を問い合わせます。  
  
5. 非同期ソケット メソッド (xxxAsync) がコールバックで false を返した場合、操作は非同期で完了しています。 コンテキスト プロパティに操作結果が問い合わされることがあります。  
  
6. 別の操作でコンテキストを再利用するか、プールに戻すか、破棄します。  
  
 新しい非同期ソケット操作コンテキスト オブジェクトの有効期間は、アプリケーション コードの参照と非同期 I/O の参照により決定されます。 非同期ソケット操作メソッドの 1 つにパラメーターとして送信された後、非同期ソケット操作コンテキスト オブジェクトの参照をアプリケーションが維持する必要はありません。 完了コールバックが戻るまで、参照状態が維持されます。 ただし、コンテキスト オブジェクトの参照をアプリケーションが維持することには、将来の非同期ソケット操作で再利用できるため、利便性があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Net.Sockets.Socket?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SendPacketsElement?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SocketAsyncEventArgs?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SocketAsyncOperation?displayProperty=nameWithType>
- [ネットワーク プログラミングのサンプル](network-programming-samples.md)
- [ソケットのコード例](socket-code-examples.md)
