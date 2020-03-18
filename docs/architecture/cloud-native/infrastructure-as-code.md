---
title: コードとしてのインフラストラクチャ
description: Azure 向けのクラウドネイティブ .NET アプリの設計 |コードとしてのインフラストラクチャ
ms.date: 06/30/2019
ms.openlocfilehash: 3957da68ac28774f899f49fb181a29c2435902f8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73841781"
---
# <a name="infrastructure-as-code"></a>コードとしてのインフラストラクチャ

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

クラウドネイティブアプリケーションでは、あらゆる種類の優れたサービスとしてのプラットフォーム (PaaS) コンポーネントを使用する傾向があります。 Azure のようなクラウドプラットフォームでは、これらのコンポーネントにストレージ、Service Bus、SignalR サービスなどが含まれる場合があります。 アプリケーションが複雑になるにつれて、使用されているサービスの数が増加する可能性があります。 継続的デリバリーでは、環境への展開の従来のモデルが手動で中断されていたのと同じように、変更の迅速なペースにより、集中管理された IT グループが環境を管理するモデルも破損しています。

環境の構築も自動化することができます。 プロセスを簡単にするための、非常に便利なツールが豊富に用意されています。

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager テンプレート

Azure Resource Manager テンプレートは、Azure でさまざまなリソースを定義するための JSON ベースの言語です。 基本的なスキーマは、図11-10 のようになります。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

**図 11-10** -Resource Manager テンプレートのスキーマ

このテンプレート内では、次のように resources セクション内にストレージコンテナーを定義できます。

```json
"resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
```

**図 11-11** -Resource Manager テンプレートで定義されたストレージアカウントの例

テンプレートをパラメーター化すると、1つのテンプレートをさまざまな設定で再利用して、開発、QA、および運用環境を定義できます。 これにより、低い環境とは異なる方法で設定された、より高い環境に移行するときに予想外の事態を防ぐことができます。 テンプレートで定義されたリソースは、通常、Azure 上の1つのリソースグループ内に作成されます (1 つの resource Manager テンプレートに複数のリソースグループを定義することはできますが、通常はそうではありません)。 これにより、リソースグループ全体を削除するだけで、簡単に環境を削除できます。 コスト分析は、リソースグループレベルで実行することもできます。これにより、各環境のコストをすばやく計算できます。

GitHub の[Azure クイックスタートテンプレート](https://github.com/Azure/azure-quickstart-templates)プロジェクトで定義されているテンプレートの例の多くは、新しいテンプレートを開始するとき、または既存のテンプレートに追加するときに、脚を上げるために用意されています。

Resource Manager テンプレートは、さまざまな方法で実行できます。 おそらく、簡単な方法は、単に Azure portal に貼り付けることです。 試験的なデプロイの場合、この方法は非常に簡単です。 また、Azure DevOps のビルドプロセスまたはリリースプロセスの一部として実行することもできます。 Azure への接続を活用してテンプレートを実行するタスクがあります。 Resource Manager テンプレートへの変更は段階的に適用されます。つまり、新しいリソースを追加するには、テンプレートに追加するだけで済みます。 ツールは、テンプレートで定義されている目的のリソースグループを使用して、現在のリソースグループの比較を処理します。 リソースは、テンプレートで定義されている内容と一致するように作成または変更されます。  

## <a name="terraform"></a>Terraform

Resource Manager テンプレートの欠点は、Azure クラウドに固有のものです。 複数のクラウドのリソースを含むアプリケーションを作成するのは普通ではありませんが、ビジネスが見事なアップタイムに依存している場合は、複数のクラウドをサポートするコストを意味することがあります。 すべてのクラウドで使用できるテンプレート言語が1つある場合は、開発者のスキルをさらに移植しやすくすることもできます。

いくつかのテクノロジが存在します。 この領域で最も成熟したオファリングは[Terraform](https://www.terraform.io/)と呼ばれています。 Terraform は、Azure、Google Cloud Platform、AWS、AliCloud などのすべての主要クラウドプレーヤーをサポートします。また、Heroku や DigitalOcean など、多数の小規模なプレーヤーもサポートしています。 JSON をテンプレート定義言語として使用する代わりに、やや簡潔な YAML を使用します。

前の Resource Manager テンプレートと同様の Terraform ファイルの例 (図 11-11) を図11-12 に示します。

```terraform
provider "azurerm" {
  version = "=1.28.0"
}

resource "azurerm_resource_group" "test" {
  name     = "production"
  location = "West US"
}

resource "azurerm_storage_account" "testsa" {
  name                     = "${var.storageAccountName}"
  resource_group_name      = "${azurerm_resource_group.testrg.name}"
  location                 = "${var.region}"
  account_tier             = "${var.tier}"
  account_replication_type = "${var.replicationType}"

}
```

**図 11-12** -Resource Manager テンプレートの例

Terraform では、テンプレートでエラーが発生したためにリソースをデプロイできない場合に、適切なエラーメッセージを提供することができます。 これは、Resource Manager テンプレートが進行中の課題を抱えている領域です。 また、テンプレートエラーを早期にキャッチするためにビルドフェーズで使用できる、非常に便利な検証タスクもあります。

Resource Manager テンプレートと同様に、Terraform テンプレートのデプロイに使用できるコマンドラインツールがあります。 また、Azure Pipelines には、Terraform テンプレートを検証して適用できるコミュニティによって作成されたタスクもあります。

Terraform テンプレートまたは Resource Manager テンプレートが、新しく作成されたデータベースへの接続文字列などの興味深い値を出力する場合、ビルドパイプラインでキャプチャし、後続のタスクで使用することができます。

>[!div class="step-by-step"]
>[前へ](devops.md)
>[次へ](application-bundles.md)
