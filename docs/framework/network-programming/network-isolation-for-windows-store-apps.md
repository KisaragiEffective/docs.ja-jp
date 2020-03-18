---
title: Windows ストア アプリのネットワーク分離
ms.date: 03/30/2017
ms.assetid: b064497c-d956-46b8-838d-7a0223c7e200
ms.openlocfilehash: 390a0281f03b08322cc1bee469b601fd5a1547c4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74802167"
---
# <a name="network-isolation-for-windows-store-apps"></a>Windows ストア アプリのネットワーク分離

名前空間 <xref:System.Net>、<xref:System.Net.Http>、<xref:System.Net.Http.Headers> 内のクラスを使用して、Windows ストア アプリまたはデスクトップ アプリを開発できます。 Windows ストア アプリで使用する場合、これらの名前空間内のクラスは、Windows 8 で使用されるアプリケーション セキュリティ モデルの一部であるネットワーク分離の影響を受けます。 システムでネットワーク アクセスが許可されるように、Windows ストア アプリのアプリ マニフェストで適切なネットワーク機能を有効にする必要があります。  
  
## <a name="checklist-for-network-isolation"></a>ネットワーク分離のチェックリスト  

Windows ストア アプリに対してネットワークの分離が適切に構成されているかどうかを、このチェックリストを使って確認してください。  
  
1. アプリが必要とするネットワーク アクセス要求の方向を確認します。 これには、クライアント側から開始される送信要求と、受信側が送信を要求していない受信要求とがあります。両方の種類のネットワーク要求を組み合わせたものもあります。  
  
2. アプリの通信相手となるネットワーク リソースの種類を確認します。 アプリの通信相手は、ホーム ネットワークまたは職場ネットワーク上の信頼済みリソースである場合があります。 または、インターネット上のリソースと通信することが必要な場合があります。 その両方の種類のネットワーク リソースにアクセスする場合もあります。  
  
3. アプリ マニフェストで、必要最小限のネットワーク分離機能を構成します。  
  
4. アプリを展開して実行し、トラブルシューティングのためのネットワーク分離ツールを使用してテストします。  
  
ネットワーク機能の構成と、ネットワーク分離のトラブルシューティングに使用する分離ツールの詳細については、Windows 8.x Store 開発者向けドキュメントで、[ネットワーク分離の機能を構成する方法](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))に関するページを参照してください。
  
## <a name="see-also"></a>参照

- [Web サービスへの接続](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
- [ネットワーク分離のガイドラインとチェックリスト](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))
- [クイック スタート: HttpClient を使って接続する](https://docs.microsoft.com/previous-versions/windows/apps/hh781239(v=win.10))
- [HttpClient ハンドラーを使う方法](https://docs.microsoft.com/previous-versions/windows/apps/hh781241(v=win.10))
- [HttpClient の接続をセキュリティで保護する方法](https://docs.microsoft.com/previous-versions/windows/apps/hh781240(v=win.10))
- [HttpClient のサンプル](https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664)
