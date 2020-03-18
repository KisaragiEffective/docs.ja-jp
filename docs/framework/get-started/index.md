---
title: .NET Framework の概要
ms.custom: updateeachrelease
ms.date: 04/02/2019
helpviewer_keywords:
- .NET Framework, getting started
- getting started [.NET Framework]
ms.assetid: c693fd34-88fe-4d90-b332-19eeadf3b7e7
ms.openlocfilehash: 0fffbb2f1b58d8ba65d9c5ca8b7c8edc583af2da
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397624"
---
# <a name="get-started-with-net-framework"></a>.NET Framework の使用を開始する

.NET Framework は、.NET Framework を対象としたアプリを管理するランタイム実行環境です。 これは、メモリ管理やその他のシステム サービスを提供する共通言語ランタイムと、プログラマがアプリ開発のすべての主要領域で堅牢性と信頼性の高いコードを利用できるようにするための広範なクラス ライブラリから構成されています。

> [!NOTE]
> .NET Framework は、Windows システムでのみ使用できます。 [.NET Core](../../core/index.md) を使用することで、Windows、MacOS、および Linux でアプリを実行できます。

## <a name="what-is-net-framework"></a>.NET Framework とは

.NET Framework は、実行中のアプリにさまざまなサービスを提供する、Windows 向けのマネージド実行環境です。 これは、実行中のアプリを処理する実行エンジンである共通言語ランタイム (CLR) と、開発者が独自のアプリから呼び出すことができる検証済みの再利用可能なコード ライブラリである .NET Framework クラス ライブラリから構成されています。 .NET Framework は実行中のアプリに次のようなサービスを提供します。

- メモリ管理 多くのプログラミング言語では、プログラマがメモリの割り当てと解放およびオブジェクトの有効期間の処理を行う必要があります。 .NET Framework アプリでは、CLR がアプリに代わってこのサービスを提供します。

- 共通型システム。 従来のプログラミング言語では、コンパイラによって基本型が定義されており、このために言語間の相互運用性が複雑になります。 .NET Framework では、基本型は .NET Framework の型システムによって定義され、.NET Framework を対象とするすべての言語に共通です。

- 広範なクラス ライブラリ。 プログラマは、低レベルの共通プログラミング操作を処理するためのコードを大量に記述する代わりに、簡単にアクセスできる .NET Framework クラス ライブラリの型とメンバーのライブラリを使用します。

- 開発フレームワークとテクノロジ。 .NET Framework には、Web アプリ向けの ASP.NET、データ アクセス向けの ADO.NET、サービス指向アプリ向けの Windows Communication Foundation、Windows デスクトップ アプリ向けの Windows Presentation Foundation などの特定領域のアプリ開発のためのライブラリが用意されています。

- 言語の相互運用性。 .NET Framework を対象とする言語コンパイラは、共通中間言語 (CIL) と呼ばれる中間コードを生成し、このコードが共通言語ランタイムによって実行時にコンパイルされます。 この機能を使用すると、1 つの言語で記述されたルーチンが他の言語でもアクセスでき、プログラマは優先言語でのアプリの作成に集中できます。

- バージョンの互換性。 まれなケースを除き、特定のバージョンの .NET Framework を使用して開発されたアプリは、後続バージョンでも変更の必要なく実行されます。

- side-by-side 実行。 .NET Framework では、同じコンピューターに複数バージョンの共通言語ランタイムが共存でき、バージョン競合の解決に役立ちます。 これは、複数バージョンのアプリも共存でき、アプリがビルドされた .NET Framework のバージョンでアプリを実行できることを意味します。 side-by-side 実行は、.NET Framework の 1.0/1.1、2.0/3.0/3.5、および 4/4.5.x/4.6.x/4.7.x/4.8 のバージョン グループに適用されます。

- 複数バージョン対応。 [.NET Standard](../../standard/net-standard.md) に対応することで、開発者は、.NET Standard の特定のバージョンでサポートされる、複数の .NET Framework プラットフォームで機能するクラス ライブラリを作成できます。 たとえば、.NET Standard 2.0 に対応するライブラリは、.NET Framework 4.6.1、.NET Core 2.0、および UWP 10.0.16299 に対応するアプリで使用できます。

<a name="ForUsers"></a>
## <a name="the-net-framework-for-users"></a>ユーザーにとっての .NET Framework

.NET Framework アプリを開発しなくても、使用するユーザーは、.NET Framework やその操作に関する知識を持つ必要はありません。 ほとんどの場合、フレームワークは、ユーザーにとって完全に透過的に機能します。

Windows オペレーティング システムを使用している場合は、.NET Framework がコンピューターに既にインストールされている場合があります。 また、.NET Framework を必要とするアプリをインストールすると、アプリのセットアップ プログラムにより、コンピューターに特定バージョンのフレームワークがインストールされることがあります。 場合によっては、.NET Framework のインストールを求めるダイアログ ボックスが表示されることがあります。 アプリを実行しようとしたときにこのダイアログ ボックスが表示される場合や、コンピューターからインターネットにアクセスできる場合は、必要なバージョンの .NET Framework をインストールするための Web ページにアクセスすることができます。 詳細については、[インストール ガイド](../install/index.md)をご覧ください。

通常は、コンピューターにインストールされている .NET Framework のバージョンはアンインストールしないでください。 これには、次の 2 つの理由があります。

- 使用するアプリが特定のバージョンの .NET Framework に依存している場合、そのバージョンが削除されるとアプリが破損する可能性があります。

- .NET Framework のバージョンの中には、前のバージョンのインプレース更新であるものがあります。 たとえば、.NET Framework 3.5 はバージョン 2.0 のインプレース更新で、.NET Framework 4.8 はバージョン 4 から 4.7.2 のインプレース更新です。 詳細については、「[.NET Framework のバージョンおよび依存関係](../migration-guide/versions-and-dependencies.md)」を参照してください。

Windows 8 よりも前の Windows バージョンで .NET Framework を削除する場合は、必ずコントロール パネルの **[プログラムと機能]** を使用してアンインストールしてください。 .NET Framework のバージョンを手動で削除しないでください。 Windows 8 以降では、.NET Framework はオペレーティング システム コンポーネントとなっているため、個別にアンインストールすることはできません。

1 台のコンピューターで複数バージョンの .NET Framework が同時に共存できます。 これは、後続バージョンをインストールするために前のバージョンをアンインストールする必要がないことを意味します。

## <a name="net-framework-for-developers"></a>開発者にとっての .NET Framework

開発者は、アプリを作成するために .NET Framework をサポートする任意のプログラミング言語を選択できます。 .NET Framework は言語への依存性がなく、相互運用性があるので、開発に使用した言語に関係なく、他の .NET Framework アプリおよびコンポーネントと対話できます。

.NET Framework アプリまたはコンポーネントを開発するには、次の操作を行います。

1. オペレーティング システムにプレインストールされていない場合は、アプリが対象とする .NET Framework のバージョンをインストールします。 最新の製品バージョンは .NET Framework 4.8 です。 これは Windows 10 May 2019 Update にプレインストールされており、以前のバージョンの Windows オペレーティング システムでもダウンロード可能です。 .NET Framework システム要件については、「[.NET Framework システム要件](system-requirements.md)」を参照してください。 その他のバージョンの .NET Framework のインストールの詳細については、[インストール ガイド](../install/guide-for-developers.md)に関するページを参照してください。 .NET Framework の追加パッケージは、アウト オブ バンドでリリースされます。これは、通常のリリース サイクルや定期的なリリース サイクルではなく、ローリング方式でリリースされるということを意味します。 これらのパッケージの詳細については、「[.NET Framework および特別なリリース](the-net-framework-and-out-of-band-releases.md)」を参照してください。

2. .NET Framework のバージョンでサポートされている、アプリ開発に使用する言語を選択します。 Microsoft の [Visual Basic](../../visual-basic/index.yml)、[C#](../../csharp/index.yml)、[F#](../../fsharp/index.yml)、および [C++ と CLI](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) を含む多くの言語から選択できます。 (.NET Framework のアプリを開発できるプログラミング言語は、[共通言語基盤 (CLI) 仕様](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/)に準拠します。)

3. アプリの作成に使用する、選択したプログラミング言語をサポートする開発環境を選択してインストールします。 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) は、.NET Framework アプリ用の Microsoft 統合開発環境 (IDE) です。 これには、いくつかエディションがあります。

.NET Framework を対象にしたアプリの開発の詳細については、[開発ガイド](../development-guide.md)を参照してください。

## <a name="related-articles"></a>関連記事

| Title | 説明 |
| ----- |------------ |
| [概要](overview.md) | .NET Framework を対象としたアプリを作成する開発者向けの詳細情報を提供します。 |
| [インストール ガイド](../install/index.md) | .NET Framework のインストールに関する情報を提供します。 |  
| [.NET Framework および特別なリリース](the-net-framework-and-out-of-band-releases.md) | .NET Framework のアウトオブバンド リリースと、これらをアプリ内で使用する方法について説明します。 |
| [システム要件](system-requirements.md) | .NET Framework を実行するためのハードウェアおよびソフトウェアの要件を示します。 |
| [.NET Core とオープン ソース](net-core-and-open-source.md) | .NET Core と .NET Framework の関係性、およびオープン ソースの .NET Core プロジェクトにアクセスする方法について説明します。 |
| [.NET Core ドキュメント](../../core/index.md) | .NET Core の概念と API リファレンス ドキュメントを提供します。 |
| [.NET Standard](../../standard/net-standard.md) | 統一された API のセットを複数のプラットフォームで使用できるようにするために、個々の .NET 実装がサポートするバージョン管理された仕様である、.NET Standard について説明します。

## <a name="see-also"></a>関連項目

- [.NET Framework ガイド](../index.md)
- [新機能](../whats-new/index.md)
- [.NET API ブラウザー](../../../api/index.md)
- [開発ガイド](../development-guide.md)
