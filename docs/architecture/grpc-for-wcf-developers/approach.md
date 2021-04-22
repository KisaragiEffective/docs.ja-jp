---
title: gRPC でいかにして RPC に近づくか - WCF 開発者向け gRPC
description: WCF の主な機能を gRPC と比較します。
ms.date: 09/02/2019
ms.openlocfilehash: b924d2f20b54de2d6476a0b27626d5dd7fd0986f
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711478"
---
# <a name="how-grpc-approaches-rpc"></a>gRPC でいかにして RPC に近づくか

Windows Communication Foundation (WCF) と gRPC はどちらも、*リモート プロシージャ コール* (RPC) パターンの実装です。 このパターンは、クライアント アプリケーションのメソッド呼び出しのように、別のコンピューターまたは別のプロセスで実行されるサービスへの呼び出しをシームレスに行うことを目的としています。 WCF と gRPC の目的は同じですが、実装の詳細はまったく異なります。

次の表では、WCF の主な機能が gRPC とどのように関連しているか、およびさらに詳細な説明が記載されている場所を示します。

| 特徴 | WCF | gRPC |
| -------- | --- | ---- |
| 目的 | ネットワークの実装からビジネス コードを分離します。 | ビジネス コードをインターフェイスの定義とネットワークの実装から分離します。 |
| サービスとメッセージを定義する (3-4 章)  | サービス コントラクト、操作コントラクト、データ コントラクト。 | proto ファイルを使用して、サービスとメッセージを宣言します。 |
| 言語 (3-5 章) | C# または Visual Basic で記述されたコントラクト。 | プロトコル バッファー言語。 |
| ワイヤ形式 (3 章) | 構成可能 (SOAP/XML、Plain XML、JSON、.NET バイナリなど)。 | プロトコル バッファー バイナリ形式 (ただし、他の形式も使用可能)。
| 相互運用性 (4 章) | SOAP over HTTP を使用する場合。 | 公式サポート: .NET、Java、Python、JavaScript、C/C++、Go、Rust、Ruby、Swift、Dart、PHP。 他の言語はコミュニティからの非公式サポート。 |
| ネットワーク (4 章) | 実行時に構成されます。 NetTCP、HTTP、MSMQ を切り替えます。 | HTTP/2 (現時点では、ASP.NET Core gRPC での TCP 経由のみ)。 |
| 方法 (4 章) | 基底クラスでのシリアル化、逆シリアル化、ネットワーク コードの実行時の生成。 | 基底クラスでのシリアル化、逆シリアル化、ネットワーク コードのビルド時の生成。 |
| セキュリティ (6 章) | 認証、WS-Security、メッセージの暗号化。 | 資格情報、ASP.NET Core セキュリティ、TLS ネットワーク。 |

>[!div class="step-by-step"]
>[前へ](grpc-overview.md)
>[次へ](interface-definition-language.md)
