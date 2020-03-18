---
title: コンテナーとサーバーレスの手法の組み合わせ
description: コンテナーと Kubernetes をサーバーレスアプローチと組み合わせる
ms.date: 06/30/2019
ms.openlocfilehash: 58aff43adbdd2e629370cc685f32c7b61c25f85e
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "73840593"
---
# <a name="combining-containers-and-serverless-approaches"></a>コンテナーとサーバーレスの手法の組み合わせ

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

多くの場合、マイクロサービスベースのアプリケーションは、ノード間の通信のために、コンテナー、オーケストレーション、およびメッセージパッシングに大きく依存します。 このメッセージングは Azure Functions にとって理想的なタスクですが、Kubernetes と関連ツールを使用して構成および展開されたアプリケーションの残りの部分では、この同じツールセット内で Azure Functions を利用できるようになると便利です。 幸い、Kubernetes ベースのアプリの他の部分と同じプロセスとツールを使用して、Docker コンテナーに Azure Functions をラップしてデプロイすることができます。

## <a name="when-does-it-make-sense-to-use-containers-with-serverless"></a>サーバーレスでコンテナーを使用することは、どのような場合に適していますか。

既定では、サーバーレス関数は、実行されるプラットフォームについての知識がありません。 ただし、場合によっては、コードを実行するコンテナーイメージをカスタマイズするために重要な特定の要件がある場合があります。 関数が特定の言語バージョンに依存しているか、既定のイメージでサポートされていない依存関係または構成要件を持っているため、カスタムイメージを使用することをお勧めします。 このような場合は、独自のコンテナーをカスタマイズして、カスタム Docker コンテナーに関数をデプロイすることが理にかなっています。

## <a name="when-should-you-avoid-using-containers-with-azure-functions"></a>Azure Functions でコンテナーを使用しない場合はどうすればよいでしょうか。

関数の従量課金プランの課金を利用することが予想される場合は、独自のコンテナーを使用している場合は、これを行うことはできません。 さらに、Docker コンテナーを使用するだけでなく、独自の Kubernetes クラスターに関数をデプロイした場合でも、Azure Functions によって提供される組み込みのスケーリングを利用できなくなります。 次に説明する Kubernetes のスケーリング機能を使用する必要があります。

## <a name="how-to-combine-serverless-and-docker-containers"></a>サーバーレスコンテナーと Docker コンテナーを結合する方法

Docker コンテナーで Azure 関数をラップするには、Azure Functions Core Tools をインストールしてから、次のコマンドを実行します。

```console
func init ProjectName --docker
```

次のオプションから、必要なワーカーランタイムを選択します。

- `dotnet` (C#)
- `node` (JavaScript)
- `python`

プロジェクトが作成されると、Dockerfile が含まれます。 ここで、関数をローカルで作成してテストできます。 `docker build` と `docker run` のコマンドを使用してビルドし、実行します。 Docker サポートを使用して Azure Functions の構築を開始する詳細な手順については、「[カスタムイメージを使用した Linux での関数の作成](https://docs.microsoft.com/azure/azure-functions/functions-create-function-linux-custom-image)」チュートリアルを参照してください。

## <a name="how-to-combine-serverless-and-kubernetes-with-keda"></a>サーバーレスと Kubernetes を KEDA と組み合わせる方法

Azure functions は、特定の関数を対象とするイベントの割合に基づいて、需要に合わせて自動的にスケールします。 さらに、Kubernetes を利用して関数をホストし、Kubernetes ベースのイベントドリブン自動スケール (KEDA) を使用することもできます。 イベントが発生していない場合、KEDA は0インスタンスにスケールダウンでき、イベントに応答して、水平ポッドオートスケーラーを使用して要求を満たすためにコンテナーの数をスケールアップできます。 [詳細については、KEDA での Azure functions のスケーリングに関するページを参照して](https://docs.microsoft.com/azure/azure-functions/functions-kubernetes-keda)ください。

## <a name="references"></a>参照

- [Docker コンテナーで Azure Functions を実行する](https://markheath.net/post/azure-functions-docker)
- [カスタムイメージを使用して Linux で関数を作成する](https://docs.microsoft.com/azure/azure-functions/functions-create-function-linux-custom-image)
- [Kubernetes イベントドリブン自動スケールを使用した Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-kubernetes-keda)

>[!div class="step-by-step"]
>[前へ](leverage-serverless-functions.md)
>[次へ](deploy-containers-azure.md)
