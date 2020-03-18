---
title: Wcf 開発者向けの gRPC-gRPC への WCF ソリューションの移行
description: GRPC で異なる種類の WCF サービスを同等のものに移行する方法について説明します。
ms.date: 09/02/2019
ms.openlocfilehash: 12e724ab46a33547d352da7a604a5a994e617bc2
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628515"
---
# <a name="migrate-a-wcf-solution-to-grpc"></a>WCF ソリューションを gRPC に移行する

この章では、ASP.NET Core 3.0 gRPC プロジェクトを操作する方法と、異なる種類の Windows Communication Foundation (WCF) サービスを同等の gRPC に移行する方法について説明します。

- ASP.NET Core 3.0 gRPC プロジェクトを作成します。
- GRPC 単項 RPC への単純な要求/応答操作。
- GRPC クライアントストリーミング RPC への一方向操作。
- 完全な二重サービスから gRPC 双方向ストリーミング RPC。

また、ストリーミングサービスの使用と、データセットを返すためのフィールドの繰り返しを比較しています。また、章の最後にクライアントライブラリを使用する方法についても説明します。

サンプル WCF アプリケーションは、一連の stock 取引サービスの最小スタブです。 依存関係の挿入のために、Autofac という名前のオープンソースのコントロールの反転 (IoC) コンテナーライブラリを使用します。 これには、WCF サービスの種類ごとに1つずつ、3つのサービスが含まれます。 これらのサービスについては、以降のセクションで詳しく説明します。 ソリューションは、GitHub の[dotnet/grpc-wcf-開発者](https://github.com/dotnet-architecture/grpc-for-wcf-developers)からダウンロードできます。 サービスは、偽のデータを使用して外部の依存関係を最小限に抑えます。

サンプルには、各サービスの WCF および gRPC 実装が含まれています。

>[!div class="step-by-step"]
>[前へ](ws-protocols.md)
>[次へ](create-project.md)
