---
title: Visual Studio で LINQ to DataSet プロジェクトを作成する
ms.date: 08/15/2018
ms.assetid: 49ba6cb0-cdd2-4571-aeaa-25bf0f40e9b3
ms.openlocfilehash: 91032766248b11e51b90aa788b1c64c140347c25
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802027"
---
# <a name="how-to-create-a-linq-to-dataset-project-in-visual-studio"></a>方法: Visual Studio で LINQ to DataSet プロジェクトを作成する

LINQ プロジェクトの種類によっては、特定のアセンブリ参照とインポートされた名前空間C#(Visual Basic) または [using](../../../csharp/language-reference/keywords/using-directive.md) ディレクティブ () が必要になります。 LINQ の最小要件は、<xref:System.Linq>用の*system.servicemodel および `using`* ディレクティブへの参照です。

これらの要件は、Visual Studio 2017 以降のバージョンC#で新しいコンソールアプリプロジェクトを作成した場合に既定で提供されます。 以前のバージョンの Visual Studio からプロジェクトをアップグレードする場合は、これらの LINQ 関連の参照を手動で指定することが必要になる場合があります。

LINQ to DataSet*には、次の 2*つの追加の参照が必要*です。*

> [!NOTE]
> コマンドプロンプトからビルドする場合は、 *%ProgramFiles%\Reference Assemblies\Microsoft\Framework\v3.5*で LINQ 関連の dll を手動で参照する必要があります。

## <a name="to-enable-linq-to-dataset-functionality"></a>LINQ to DataSet 機能を有効にするには

既存のプロジェクトで LINQ to DataSet 機能を有効にするには、次の手順に従います。

1. **System. Core**、system.string、および**datasetextensions** **への参照**を追加します。

   **ソリューションエクスプローラー**で、 **[参照]** ノードを右クリックし、 **[参照の追加]** を選択します。 **[参照マネージャー]** ダイアログボックスで、 **[Core**]、 **[システムデータ]** 、および **[データ拡張子]** を選択します。 **[OK]** を選択します。

1. [Using](../../../csharp/language-reference/keywords/using-directive.md)ディレクティブ (または Visual Basic 内の[Imports ステートメント](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md))**を system.string および system.string**に**追加します。**

   ```csharp
   using System.Data;
   using System.Linq;
   ```

1. 必要に応じて、データベースへの接続方法に応じ**て、`using`** ディレクティブ (または `Imports` のステートメント) を**追加します**。

## <a name="see-also"></a>参照

- [LINQ to DataSet を使ってみる](getting-started-linq-to-dataset.md)
