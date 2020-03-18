---
title: .NET ネイティブ リフレクション API リファレンス
ms.date: 03/30/2017
ms.assetid: 0429c049-22a3-4ba1-9cc8-f6ee91e31d9c
ms.openlocfilehash: 01678ea6230a53416f213730ae6bb66e6bc057f8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128216"
---
# <a name="net-native-reflection-api-reference"></a>.NET ネイティブ リフレクション API リファレンス
.NET ネイティブには、MissingInteropDataException、 [MissingMetadataException](missingmetadataexception-class-net-native.md)、 [system.runtime.compilerservices](missinginteropdataexception-class-net-native.md)の3つの新しい例外の種類が含まれています。この[例外](missingruntimeartifactexception-class-net-native.md)については、次のとおりです。 3 つの例外型すべてについて、次の点に注意してください。  
  
 これらの型は、内部でのみ使用してください。  
 この3つの例外の種類は、.NET ネイティブツールチェーンのみを使用するためのものです。 例外は、.NET ネイティブツールチェーンが、プログラムの実行を続行できない欠損データを検出した場合にスローされます。  
  
 これらの例外をコード内で処理しないでください。  
 これらの例外は、アプリケーションで必要なメタデータが存在しないこと ( [MissingInteropDataException](missinginteropdataexception-class-net-native.md) 例外と [MissingMetadataException](missingmetadataexception-class-net-native.md) 例外)、またはアプリケーションで必要な実装コードが欠落していること ( [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 例外) のどちらかを示します。 これらの例外状態を修正するには、ランタイム ディレクティブ (.rd.xml) ファイルを変更して必要なメタデータまたは実装コードを実行時に使用できるようにします。 詳細については、「 [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)」を参照してください。 次の 2 つのトラブルシューティング ツールを使用して、 [MissingMetadataException](missingmetadataexception-class-net-native.md) および [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 例外を取り除くための適切なエントリをランタイム ディレクティブ ファイルに提供できます。  
  
- [MissingMetadataException トラブルシューティング ツール](https://dotnet.github.io/native/troubleshooter/type.html) (型の場合)。  
  
- [MissingMetadataException トラブルシューティング ツール](https://dotnet.github.io/native/troubleshooter/method.html) (メソッドの場合)。  
  
> [!NOTE]
> このリファレンスでは、.NET ネイティブに固有の3つの例外の種類について説明します。 .NET Framework コアリフレクション API のリファレンスドキュメントについては、<xref:System.Reflection>、<xref:System.Reflection.Context>、および <xref:System.Reflection.Emit> の名前空間に関する説明を参照してください。 .NET Framework の主要な相互運用 API に関するリファレンス ドキュメントについては、「 <xref:System.Runtime.InteropServices>」を参照してください。  
  
## <a name="systemreflection-namespace"></a>System.Reflection 名前空間  
 <xref:System.Reflection> 名前空間には、.NET Framework でリフレクションに使用される主要な型が含まれています。 .NET ネイティブには、次の2つの新しい例外の種類も含まれています。  
  
|インスタンス|説明|  
|-----------|-----------------|  
|[MissingMetadataException](missingmetadataexception-class-net-native.md)|この例外は、存在しないメタデータを取得するためにリフレクションが使用された場合にスローされます。|  
|[MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md)|この例外は、型または型のメンバーのメタデータは使用可能だが、その実装が削除されている場合にスローされます。|  
  
 この名前空間の他の型に関するドキュメントについては、.NET Framework ドキュメント セットで <xref:System.Reflection> リファレンス ページを参照してください。  
  
## <a name="systemruntimecompilerservices-namespace"></a>System.Runtime.CompilerServices 名前空間  
 <xref:System.Runtime.CompilerServices> 名前空間には、言語コンパイラによって自動的にデザインされた型が含まれています。 .NET ネイティブには、新しい例外の種類も含まれています。  
  
|インスタンス|説明|  
|-----------|-----------------|  
|[MissingInteropDataException](missinginteropdataexception-class-net-native.md)|この例外は、手動マーシャリング メソッドが呼び出されたが、型のメタデータがスタティック分析でも、ランタイム ディレクティブ ファイルにも見つからない場合にスローされます。|  
  
 この名前空間の他の型に関するドキュメントについては、.NET Framework ドキュメント セットで <xref:System.Runtime.CompilerServices> リファレンス ページを参照してください。  
  
## <a name="see-also"></a>関連項目

- [MissingInteropDataException クラス](missinginteropdataexception-class-net-native.md)
- [MissingMetadataException クラス](missingmetadataexception-class-net-native.md)
- [MissingRuntimeArtifactException クラス](missingruntimeartifactexception-class-net-native.md)
- [はじめに](getting-started-with-net-native.md)
