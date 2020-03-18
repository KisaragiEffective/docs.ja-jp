---
ms.openlocfilehash: 2872c5909b382e01fdd231019a12970caa3b77d2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "72526758"
---
## <a name="installation-instructions---visual-studio-installer"></a>インストール手順 - Visual Studio インストーラー

**Visual Studio インストーラー**で **.NET Compiler Platform SDK** を見つけるには、以下の 2 つの異なる方法があります。

### <a name="install-using-the-visual-studio-installer---workloads-view"></a>Visual Studio インストーラーを使用したインストール - ワークロード ビュー

.NET Compiler Platform SDK は、Visual Studio 拡張機能の開発ワークロードの一部として自動的に選択されません。 省略可能なコンポーネントとして選択する必要があります。

1. **Visual Studio インストーラー**を実行します。
1. **[変更]** を選択します。
1. **Visual Studio 拡張機能の開発**ワークロードを確認します。
1. 概要ツリーの **[Visual Studio 拡張機能の開発]** ノードを開きます。
1. **[.NET Compiler Platform SDK]** のチェック ボックスをオンにします。 省略可能なコンポーネントの最後に表示されます。

また、必要に応じて、**DGML エディター**のビジュアライザーでグラフを表示します。

1. 概要ツリーの **[個別のコンポーネント]** ノードを開きます。
1. **[DGML エディター]** のチェック ボックスをオンにします。

### <a name="install-using-the-visual-studio-installer---individual-components-tab"></a>Visual Studio インストーラーを使用したインストール - [個別のコンポーネント] タブ

1. **Visual Studio インストーラー**を実行します。
1. **[変更]** を選択します。
1. **[個別のコンポーネント]** タブを選択します。
1. **[.NET Compiler Platform SDK]** のチェック ボックスをオンにします。 **[コンパイラ、ビルド ツール、およびランタイム]** セクションの上部に表示されます。

また、必要に応じて、**DGML エディター**のビジュアライザーでグラフを表示します。

1. **[DGML エディター]** チェック ボックスをオンにします。 **[コード ツール]** セクションに表示されます。
