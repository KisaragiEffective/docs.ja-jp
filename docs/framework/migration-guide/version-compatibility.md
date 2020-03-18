---
title: .NET Framework のバージョンの互換性
ms.custom: updateeachrelease
ms.date: 04/02/2019
helpviewer_keywords:
- .NET Framework, version compatibility
- .NET Framework, compatibility with earlier versions
- .NET Framework versions, compatibility
ms.assetid: 2f25e522-456a-48c3-8a53-e5f39275649f
ms.openlocfilehash: e0de18b5a40875d1fec2633c16688111d8f4b9ee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73974953"
---
# <a name="version-compatibility-in-the-net-framework"></a>.NET Framework のバージョンの互換性

下位互換とは、プラットフォームの特定のバージョンで開発されたアプリが、そのプラットフォームの新しいバージョンでも実行できることを意味します。 .NET Framework では、下位互換性が最大限に高められています。 .NET Framework のあるバージョンで記述されたソース コードは、.NET Framework の新しいバージョンでコンパイルでき、.NET Framework のあるバージョンで実行されるバイナリは、新しいバージョンの .NET Framework でも同じように動作します。

## <a name="Apps"></a> アプリのバージョンの互換性

既定では、アプリは、ビルド対象の .NET Framework のバージョンで実行されます。 そのバージョンが存在せず、アプリの構成ファイルにサポートされるバージョンが定義されていない場合は、.NET Framework 初期化エラーが発生することがあります。 この場合、アプリは実行できません。

アプリを実行する特定のバージョンを定義するには、1 つ以上の [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 要素をアプリの構成ファイルに追加します。 各 `<supportedRuntime>` 要素は、サポートされるランタイムのバージョンを示します。最初の要素で最も優先度の高いバージョンを、最後の要素で最も優先度の低いバージョンを指定します。

```xml
<configuration>
   <startup>
      <supportedRuntime version="v2.0.50727" />
      <supportedRuntime version="v4.0" />
   </startup>
</configuration>
```

詳しくは、[.NET Framework 4 または 4.x をサポートするアプリの構成方法](../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)に関するページをご覧ください。

## <a name="version-compatibility-for-components"></a>コンポーネントのバージョンの互換性

アプリは、実行される .NET Framework のバージョンを制御できますが、コンポーネントは制御できません。 コンポーネントとクラス ライブラリは特定のアプリのコンテキストで読み込まれるので、アプリが実行されるバージョンの .NET Framework 上で自動的に実行されます。

この制限のため、互換性の保証は、コンポーネントにとって特に重要です。 .NET Framework 4 以降、コンポーネントに <xref:System.Runtime.Versioning.ComponentGuaranteesAttribute?displayProperty=nameWithType> 属性を適用することにより、複数のバージョンで想定されるコンポーネントの互換性の程度を指定できるようになりました。 ツールは、この属性を使用して、コンポーネントの将来のバージョンでの互換性保証の潜在的な違反を検出できます。

## <a name="backward-compatibility-and-the-net-framework"></a>下位互換性と .NET Framework

.NET Framework 4.5 およびそれ以降のバージョンは、以前のバージョンの .NET Framework でビルドされたアプリと下位互換性があります。 つまり、旧バージョンの .NET Framework でビルドしたアプリとコンポーネントは、.NET Framework 4.5 およびそれ以降のバージョンを変更せずに動作します。 ただし、既定では、アプリは、開発された共通言語ランタイムのバージョンで動作するため、アプリを .NET Framework 4.5 以降のバージョンで実行するには構成ファイルを提供する必要がある場合があります。 詳細については、この記事の前半に記載した「[アプリのバージョンの互換性](#Apps)」を参照してください。

現実的には、この互換性は、.NET Framework のわずかな変更やプログラミング技法の変化によって損なわれている可能性があります。 たとえば、.NET Framework 4.5 でのパフォーマンスの向上によって、旧バージョンでは発生しなかった競合状態が顕在化する可能性があります。 同様に、.NET Framework アセンブリへのハードコーディングされたパスの使用、.NET Framework の特定のバージョンでの等値比較の実行、およびリフレクションの使用によるプライベート フィールドの値の取得は、下位互換性のない動作です。 さらに、各バージョンの .NET Framework には、バグの修正とセキュリティに関連する変更が含まれており、これらが一部のアプリとコンポーネントの互換性に影響する可能性があります。

アプリまたはコンポーネントが .NET Framework 4.5 (ポイント リリースである .NET Framework 4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2、4.8 を含む) 上で期待どおりに動作しない場合は、次のチェックリストを利用してください。

- アプリが .NET Framework 4.0 以降の任意バージョンの .NET Framework で実行できるように開発されている場合、[アプリケーションの互換性](application-compatibility.md)に関する記事を参照して、ターゲットの .NET Framework バージョンとアプリを実行しているバージョン間の違いの一覧を作成します。

- .NET Framework 3.5 のアプリがある場合、「[.NET Framework 4 の移行に関する問題](../migration-guide/net-framework-4-migration-issues.md)」も参照してください。

- .NET Framework 2.0 のアプリがある場合、「[.NET Framework 3.5 SP1 の変更点](https://docs.microsoft.com/previous-versions/dotnet/articles/dd310284(v=msdn.10))」も参照してください。

- .NET Framework 1.1 アプリがある場合、「[.NET Framework 2.0 の互換性に影響する変更点](https://docs.microsoft.com/previous-versions/aa570326(v=msdn.10))」も参照してください。

- .NET Framework 4.5 またはそのポイント リリース上で実行するために既存のソース コードを再コンパイルする場合、あるいは、.NET Framework 4.5 またはそのポイント リリースをターゲットとするアプリまたはコンポーネントの新しいバージョンを既存のソース コード ベースから開発する場合は、「[クラス ライブラリの互換性のために残されている機能](../whats-new/whats-obsolete.md)」で、古い型とメンバーを確認し、説明されている回避策を適用してください。 (コンパイル済みのコードは、互換性のために残されている旧式の型とメンバーに対して引き続き実行されます)。

- アプリが正常に動作しない原因が .NET Framework 4.5 での変更によるものとわかった場合は、「[ランタイム設定スキーマ](../configure-apps/file-schema/runtime/index.md)」の特に[\<AppContextSwitchOverrides> の要素](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)を確認して、アプリの構成ファイル内のランタイム設定を使用して以前の動作を復元できるかどうかを見極めてください。

- 説明されていない問題が発生した場合は、[.NET の開発者コミュニティ サイト](https://developercommunity.visualstudio.com/spaces/61/index.html)上で問題を提起するか、[Microsoft/dotnet GitHub リポジトリ](https://github.com/microsoft/dotnet/issues)で問題を作成してください。

## <a name="compatibility-and-side-by-side-execution"></a>互換性と side-by-side 実行

問題の適切な解決策が見つからない場合は、.NET Framework 4.5 (またはそのポイント リリースのいずれか) はバージョン 1.1、2.0、3.5 と side-by-side で実行でき、バージョン 4 に置き換わるインプレース更新であることを思い出してください。 Version 1.1、2.0、3.5 を対象とするアプリでは、適切なバージョンの .NET Framework を対象コンピューターにインストールして、アプリを最良の環境で実行できます。 side-by-side 実行について詳しくは、[side-by-side 実行](../deployment/side-by-side-execution.md)に関するページを参照してください。

## <a name="see-also"></a>参照

- [最新情報](../whats-new/index.md)
- [クラス ライブラリ内にある旧版のもの](../whats-new/whats-obsolete.md)
- [アプリケーションの互換性](../migration-guide/application-compatibility.md)
- [.NET Framework の公式サポート ポリシー](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)
- [.NET framework 4 への移行に関する問題](../migration-guide/net-framework-4-migration-issues.md)
