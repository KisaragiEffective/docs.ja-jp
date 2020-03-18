---
title: .NET ネイティブ アプリでのランタイム例外
ms.date: 03/30/2017
ms.assetid: 5f050181-8fdd-4a4e-9d16-f84c22a88a97
ms.openlocfilehash: 3132e2c9502c91cbfa0b120f664fd0c6f99a2663
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128146"
---
# <a name="runtime-exceptions-in-net-native-apps"></a>.NET ネイティブ アプリでのランタイム例外
デバッグ構成とリリース構成は完全に異なるため、ターゲット プラットフォームでユニバーサル Windows プラットフォーム アプリのリリース ビルドをテストすることは重要です。 既定では、デバッグ構成は .NET Core ランタイムを使用してアプリをコンパイルしますが、リリース構成は .NET ネイティブを使用してアプリをネイティブ コードにコンパイルします。  
  
> [!IMPORTANT]
> アプリのリリースバージョンをテストするときに発生する可能性のある[MissingMetadataException](missingmetadataexception-class-net-native.md)、 [MissingInteropDataException](missinginteropdataexception-class-net-native.md)、および[誤 Singruntimeartifactexception](missingruntimeartifactexception-class-net-native.md)例外の処理については、「手順 4:メタデータの欠落を手動で解決する:[はじめに](getting-started-with-net-native.md)のトピックのほか、[リフレクションと .NET ネイティブ](reflection-and-net-native.md)および[ランタイムディレクティブ (system.xml) 構成ファイルリファレンスも参照](runtime-directives-rd-xml-configuration-file-reference.md)してください。  
  
## <a name="debug-and-release-builds"></a>デバッグ ビルドとリリース ビルド  
 .NET Core ランタイムに対してデバッグ ビルドを実行した場合は、ネイティブ コードにコンパイルされません。 このため、一般にランタイムによって提供されるすべてのサービスをアプリで使用することができます。  
  
 一方、リリース ビルドの場合は、パフォーマンスを最大化するために、ターゲット プラットフォーム用のネイティブ コードにコンパイルされ、外部のランタイムおよびライブラリに対する依存関係のほとんどが削除され、コードが大幅に最適化されます。  
  
 .NET ネイティブを使用してコンパイルされたリリース ビルドをデバッグする場合:  
  
- 通常の .NET デバッグ ツールとは異なる、.NET ネイティブのデバッグ エンジンを使用します。  
  
- 実行可能ファイルのサイズは、最大限削減されます。 .NET ネイティブが実行可能ファイルのサイズを削減する方法の 1 つは、ランタイムの例外メッセージを大幅にトリミングする方法です。これについては、「 [Runtime exception messages](#Messages) 」セクションでトピックとして詳細に説明しています。  
  
- コードは大幅に最適化されます。 つまり、できる限りインライン展開が使用されます。 (インライン展開では、外部ルーチンから呼び出し元ルーチンにコードを移動します)。  .NET ネイティブが特殊なランタイムを提供し、積極的なインライン化を実装すると、デバッグ時に表示される呼び出し履歴に影響します。  詳細については、「 [Runtime call stack](#CallStack) 」を参照してください。  
  
> [!NOTE]
> **[.NET ネイティブ ツール チェーンを使用してコンパイルする]** ボックスをオンまたはオフにすることによって、デバッグ ビルドとリリース ビルドを .NET ネイティブ ツール チェーンでコンパイルするかどうかを制御できます。   ただし、Windows ストアは常に .NET ネイティブ ツールのチェーンを使用してアプリの製品バージョンをコンパイルすることに注意してください。  
  
<a name="Messages"></a>   
## <a name="runtime-exception-messages"></a>Runtime exception messages  
 アプリケーションの実行可能ファイルのサイズを最小限に抑えるために、.NET ネイティブは例外メッセージの全文を組み込みません。 そのため、リリース ビルドでスローされるランタイム例外では、例外メッセージの全文が表示されない場合があります。 代わりに、部分的な文字列を含むテキストが、詳細を示すリンクと共に表示される可能性があります。 たとえば、以下のような例外情報が表示されます。  
  
```output
Exception thrown: '$16_System.AggregateException' in Unknown Module.  
  
Additional information: AggregateException_ctor_DefaultMessage  
  
If there is a handler for this exception, the program may be safely continued.  
```  
  
 完全な例外メッセージが必要な場合は、代わりにデバッグ ビルドを実行します。 たとえば、リリース ビルドからの上記の例外情報は、デバッグ ビルドでは以下のように表示されます。  
  
```output
Exception thrown: 'System.AggregateException' in NativeApp.exe.  
  
Additional information: Value does not fall within the expected range.  
```  
  
<a name="CallStack"></a>   
## <a name="runtime-call-stack"></a>Runtime call stack  
 インライン展開および他の最適化のために、.NET ネイティブ ツール チェーンでコンパイルされたアプリで表示される呼び出し履歴では、ランタイム例外へのパスを明確に識別できない場合があります。  
  
 完全なスタックを取得するには、代わりにデバッグ ビルドを実行します。  
  
## <a name="see-also"></a>関連項目

- [Windows ユニバーサルアプリのデバッグ .NET ネイティブ](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/)
- [はじめに](getting-started-with-net-native.md)
