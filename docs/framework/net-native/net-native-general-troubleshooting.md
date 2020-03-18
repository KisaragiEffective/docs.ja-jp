---
title: .NET ネイティブの一般的なトラブルシューティング
ms.date: 03/30/2017
ms.assetid: ee8c5e17-35ea-48a1-8767-83298caac1e8
ms.openlocfilehash: 2bea81e380fed6c456898e9883658ef874c8dd97
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128241"
---
# <a name="net-native-general-troubleshooting"></a>.NET ネイティブの一般的なトラブルシューティング

このトピックでは、.NET ネイティブでアプリを開発するときに発生する可能性がある問題のトラブルシューティング方法について説明します。

- **問題:** ビルド出力ウィンドウが正しく更新されない。

  **解決方法:** ビルド出力ウィンドウは、ビルドが完了するまで更新されません。 ビルドには数分かかる場合があるため、更新が表示されるまでに遅延が発生することがあります。

- **問題:** ARM 用のアプリの製品ビルド時間が長くなった。

  **解決策:** ARM デバイスにアプリをデプロイすると、.NET ネイティブインフラストラクチャが呼び出されます。 このコンパイルは、リフレクションなどの非静的セマンティクスの実行が継続された状態で、多数の最適化を実行します。 さらに、パフォーマンスの最適化のために、.NET Framework でアプリが使用する部分は静的リンクされるため、コンパイルしてネイティブ コードにも含める必要があります。 このため、コンパイルの時間が長くなります。

  ただし、標準的な開発用コンピューター上で、ほとんどのアプリの標準的なコンパイルにかかる時間は 1 分以内です。  通常、標準的な開発用コンピューターでは、.NET Framework のネイティブ イメージを生成するだけで数分かかります。  生成されるコードを改善するための最適化をすべて行い、.NET Framework を含めても、通常、アプリのビルド時間は 1 ～ 2 分です。

  現在も、マルチスレッド コンパイルやその他の最適化を調査して、コンパイルのパフォーマンスを改善する取り組みが続いています。

- **問題:** アプリが .NET ネイティブを使用してコンパイルされたかどうかはわかりません。

  **解決策:** .NET ネイティブコンパイラが呼び出されると、ビルド時間が長くなり、タスクマネージャーによって、ILC や nutc_driver などのさまざまな .NET ネイティブコンポーネントプロセスが表示されます。

  .NET ネイティブでプロジェクトを正常にビルドすると、obj\\*config*\ *arch*\\*projectname*. ilc\out の下に出力が表示されます。 最終的なネイティブパッケージの内容については、「bin\\*arch*\\*config*\appx」を参照してください。 アプリを配置した場合、最終的なネイティブ パッケージ コンテンツは \bin\\*arch*\\*config*\AppX にあります。

- **問題:** .NET Native を使用してアプリをコンパイルすると、.NET Native を使用せずにコンパイルしたときにはスローされないランタイム例外 (通常は [MissingMetadataException](missingmetadataexception-class-net-native.md) または [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 例外) がスローされる。

  **解決方法:** これらの例外は、リフレクションを介して使用できるはずのメタデータまたは実装コードが .NET Native では提供されなかったためにスローされます (詳細については、「 [.NET ネイティブとコンパイル](net-native-and-compilation.md)」を参照してください)。この例外を回避するには、ランタイム[ディレクティブ (unattend.xml) ファイル](runtime-directives-rd-xml-configuration-file-reference.md)にエントリを追加する必要があります。これにより、.NET ネイティブツールチェーンが実行時にメタデータまたは実装コードを使用できるようになります。 次の 2 つのトラブルシューティング ツールを使用して、ランタイム ディレクティブ ファイルに追加する必要があるエントリを生成できます。

  - [MissingMetadataException トラブルシューティング ツール](https://dotnet.github.io/native/troubleshooter/type.html) (型の場合)。

  - [MissingMetadataException トラブルシューティング ツール](https://dotnet.github.io/native/troubleshooter/method.html) (メソッドの場合)。

  詳細については、「[リフレクションおよび .NET ネイティブ](reflection-and-net-native.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [Windows ストア アプリの .NET ネイティブへの移行](migrating-your-windows-store-app-to-net-native.md)
