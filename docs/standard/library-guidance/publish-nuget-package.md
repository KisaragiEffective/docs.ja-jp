---
title: NuGet パッケージの公開
description: .NET ライブラリを NuGet に公開するためのベスト プラクティスの推奨事項。
ms.date: 10/02/2018
ms.openlocfilehash: 089c660bc51252c6295858b1462ae59bde968564
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76744562"
---
# <a name="publishing-a-nuget-package"></a>NuGet パッケージの公開

NuGet パッケージは、パッケージ リポジトリから公開され、使用されます。 NuGet.org は最も広く知られ、使用されているリポジトリですが、NuGet パッケージを公開するための場所は数多くあります。

* **[NuGet.org](https://www.nuget.org/)** は、NuGet パッケージの主要なオンライン リポジトリです。 NuGet.org にあるパッケージはすべて一般に公開されています。 Visual Studio の既定のパッケージ ソースは NuGet.org です。また、多くの開発者にとって、やり取りする唯一のパッケージ リポジトリは NuGet.org です。 NuGet.org は、コミュニティからのフィードバックが必要な安定版パッケージとプレリリース パッケージを公開するのに最適な場所です。

* **[MyGet](https://myget.org/)** は、オープン ソース プロジェクト用のカスタム パッケージ フィードをサポートしているリポジトリ サービスです。 MyGet のパブリック カスタム フィードは、独自の CI サービスで作成したプレリリース パッケージを公開するのに理想的な場所です。 MyGet には商用のプライベート フィードも用意されています。

* **[ローカル フィード](/nuget/hosting-packages/local-feeds)** を使用すると、フォルダーをパッケージ リポジトリのように扱い、フォルダー内の `*.nupkg` ファイルを NuGet からアクセス可能にすることができます。 ローカル フィードは、NuGet.org に公開する前に NuGet パッケージをテストする場合に便利です。

> [!NOTE]
> NuGet.org では、一度アップロードしたら[パッケージを削除することはできません](/nuget/policies/deleting-packages)。 一般の UI から見えないようにパッケージを一覧から外すことはできますが、`*.nupkg` は引き続き復元でダウンロードできます。 また、nuget.org では、パッケージのバージョンを重複させることはできません。 エラーを含む NuGet パッケージを修正するには、正しくないパッケージを一覧から外し、バージョン番号をインクリメントして新しいバージョンのパッケージを公開する必要があります。

✔️ 実行 コミュニティからのフィードバックが必要な[安定版パッケージとプレリリース パッケージを公開する](/nuget/create-packages/publish-a-package)場合、NuGet.org を選択します。

✔️ 検討 継続的インテグレーションによるビルドから、プレリリース パッケージを MyGet のフィードに公開します。

✔️ 検討 ローカル フィードか MyGet を使用して、開発環境でパッケージをテストします。 パッケージの動作を確認したら、NuGet.org に公開します。

## <a name="nugetorg-security"></a>NuGet.org のセキュリティ

悪意のあるユーザーが自分の NuGet アカウントにアクセスし、悪意のあるバージョンのライブラリをアップロードできないことが重要です。 NuGet.org には、パッケージを公開する際の 2 要素認証と電子メールの通知が用意されています。 NuGet.org にログインした後、 **[Account settings]\(アカウント設定\)** ページでこれらの機能を有効にします。

![代替テキスト](./media/publish-nuget-package/nuget-2fa.png "NuGet アカウントのセキュリティ")

✔️ 実行 Microsoft アカウントを使用して NuGet にサインインします。

✔️ 実行 NuGet へのアクセスに対して 2 要素認証を有効にします。

✔️ 実行 パッケージが公開されたときの電子メール通知を有効にします。

>[!div class="step-by-step"]
>[前へ](sourcelink.md)
>[次へ](versioning.md)
