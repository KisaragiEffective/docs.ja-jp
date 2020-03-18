---
title: GRPC が WCF 開発者向けの RPC-gRPC にアプローチする方法
description: WCF の主な機能を gRPC と比較します。
ms.date: 09/02/2019
ms.openlocfilehash: b924d2f20b54de2d6476a0b27626d5dd7fd0986f
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711478"
---
# <a name="how-grpc-approaches-rpc"></a>gRPC でいかにして RPC に近づくか

Windows Communication Foundation (WCF) と gRPC はどちらも、*リモートプロシージャコール*(RPC) パターンの実装です。 このパターンは、クライアントアプリケーションのメソッド呼び出しのように、別のコンピューターまたは別のプロセスで実行されるサービスへの呼び出しをシームレスに行うことを目的としています。 WCF と gRPC の目的は同じですが、実装の詳細はまったく異なります。

次の表は、WCF の主な機能が gRPC とどのように関連しているか、さらに詳細な説明が記載されている場所を示しています。

| フィーチャー | WCF | gRPC |
| -------- | --- | ---- |
| 目標 | ネットワーク実装からビジネスコードを分離します。 | インターフェイス定義とネットワーク実装からビジネスコードを分離します。 |
| サービスとメッセージを定義する (章 3-4)  | サービスコントラクト、操作コントラクト、およびデータコントラクト。 | では、プロトコルファイルを使用して、サービスとメッセージを宣言します。 |
| 言語 (章 3-5) | または Visual Basic C#で記述されたコントラクト。 | プロトコルバッファー言語。 |
| ワイヤ形式 (第3章) | SOAP/XML、Plain XML、JSON、および .NET バイナリを含む構成可能。 | プロトコルバッファーバイナリ形式 (他の形式を使用することもできます)。
| 相互運用性 (第4章) | SOAP over HTTP を使用する場合。 | 公式サポート: .NET、Java、Python、JavaScript、C/C++、ゴー、Rust、Ruby、Swift、DART、PHP。 コミュニティの他の言語を非公式にサポートします。 |
| ネットワーク (第4章) | 実行時に構成されます。 NetTCP、HTTP、MSMQ を切り替えます。 | HTTP/2 (現時点では ASP.NET Core gRPC を使用した TCP のみ)。 |
| 方法 (第4章) | 基本クラスのシリアル化、逆シリアル化、およびネットワークコードのランタイム生成。 | 基本クラスでのシリアル化、逆シリアル化、およびネットワークコードのビルド時生成。 |
| セキュリティ (第6章) | 認証、WS-SECURITY、メッセージの暗号化。 | 資格情報、ASP.NET Core セキュリティ、TLS ネットワーク。 |

>[!div class="step-by-step"]
>[前へ](grpc-overview.md)
>[次へ](interface-definition-language.md)
