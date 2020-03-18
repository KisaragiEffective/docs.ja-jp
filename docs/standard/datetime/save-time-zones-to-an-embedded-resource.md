---
title: '方法: 埋め込みリソースにタイムゾーンを保存する'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], saving
- time zone objects [.NET Framework], serializing
- time zone objects [.NET Framework], saving
ms.assetid: 3c96d83a-a057-4496-abb0-8f4b12712558
ms.openlocfilehash: aaee4e82d09e8b604d06dadb5a5eefe8d2e1f307
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123768"
---
# <a name="how-to-save-time-zones-to-an-embedded-resource"></a>方法: 埋め込みリソースにタイムゾーンを保存する

タイムゾーンに対応するアプリケーションでは、多くの場合、特定のタイムゾーンが存在する必要があります。 ただし、個々の <xref:System.TimeZoneInfo> オブジェクトの可用性は、ローカルシステムのレジストリに格納されている情報に依存しているため、通常に使用可能なタイムゾーンも存在しない可能性があります。 また、<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドを使用してインスタンス化されたカスタムタイムゾーンに関する情報は、レジストリ内の他のタイムゾーン情報と共に格納されません。 これらのタイムゾーンが必要なときに確実に使用できるようにするには、それらをシリアル化して保存し、後でそれらを逆シリアル化して復元します。

通常、<xref:System.TimeZoneInfo> オブジェクトのシリアル化は、タイムゾーン対応アプリケーションとは別に行われます。 シリアル化された <xref:System.TimeZoneInfo> オブジェクトの保持に使用されるデータストアによっては、タイムゾーンデータがセットアップまたはインストールルーチンの一部としてシリアル化されることがあります (たとえば、データがレジストリのアプリケーションキーに格納されている場合)。または、の前に実行するユーティリティルーチンの一部として、最終的なアプリケーションがコンパイルされます (たとえば、シリアル化されたデータが .NET XML リソース (.resx) ファイルに格納されている場合)。

アプリケーションと共にコンパイルされるリソースファイルに加えて、他のいくつかのデータストアをタイムゾーン情報に使用できます。 次に例を示します。

- レジストリ。 アプリケーションでは、HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones のサブキーを使用するのではなく、独自のアプリケーションキーのサブキーを使用して、カスタムタイムゾーンデータを格納する必要があることに注意してください。

- 構成ファイル。

- その他のシステムファイル。

### <a name="to-save-a-time-zone-by-serializing-it-to-a-resx-file"></a>.Resx ファイルにシリアル化してタイムゾーンを保存するには

1. 既存のタイムゾーンを取得するか、新しいタイムゾーンを作成します。

   既存のタイムゾーンを取得する方法については、「[方法: 定義済みの UTC オブジェクトおよびローカルタイムゾーンオブジェクトにアクセス](../../../docs/standard/datetime/access-utc-and-local.md)する」および「[方法: TimeZoneInfo オブジェクトをインスタンス化](../../../docs/standard/datetime/instantiate-time-zone-info.md)する」を参照してください。

   新しいタイムゾーンを作成するには、<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドのいずれかのオーバーロードを呼び出します。 詳細については、「[方法: 調整規則のないタイムゾーンを作成](../../../docs/standard/datetime/create-time-zones-without-adjustment-rules.md)する」および「[方法: 調整規則を使用してタイムゾーンを作成する](../../../docs/standard/datetime/create-time-zones-with-adjustment-rules.md)」を参照してください。

2. <xref:System.TimeZoneInfo.ToSerializedString%2A> メソッドを呼び出して、タイムゾーンのデータを含む文字列を作成します。

3. 名前を指定し、必要に応じて、.resx ファイルのパスを <xref:System.IO.StreamWriter> クラスコンストラクターに渡して、<xref:System.IO.StreamWriter> オブジェクトをインスタンス化します。

4. <xref:System.IO.StreamWriter> オブジェクトを <xref:System.Resources.ResXResourceWriter> クラスコンストラクターに渡すことによって、<xref:System.Resources.ResXResourceWriter> オブジェクトをインスタンス化します。

5. タイムゾーンのシリアル化された文字列を <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> メソッドに渡します。

6. <xref:System.Resources.ResXResourceWriter.Generate%2A?displayProperty=nameWithType> メソッドを呼び出します。

7. <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> メソッドを呼び出します。

8. <xref:System.IO.StreamWriter.Close%2A> メソッドを呼び出して、<xref:System.IO.StreamWriter> オブジェクトを閉じます。

9. 生成された .resx ファイルをアプリケーションの Visual Studio プロジェクトに追加します。

10. Visual Studio の **[プロパティ]** ウィンドウを使用して、.resx ファイルの **[ビルドアクション]** プロパティが **[埋め込みリソース]** に設定されていることを確認します。

## <a name="example"></a>例

次の例では、中部標準時を表す <xref:System.TimeZoneInfo> オブジェクトと、SerializedTimeZones という名前の .NET XML リソースファイルへの南極 Time を表す <xref:System.TimeZoneInfo> オブジェクトをシリアル化します。 通常、中部標準時はレジストリで定義されています。パーマーステーション、南極はカスタムタイムゾーンです。

[!code-csharp[TimeZone2.Serialization#1](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#1)]
[!code-vb[TimeZone2.Serialization#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#1)]

この例では <xref:System.TimeZoneInfo> オブジェクトをシリアル化して、コンパイル時にリソースファイルで使用できるようにします。

<xref:System.Resources.ResXResourceWriter.Generate%2A?displayProperty=nameWithType> メソッドでは、完全なヘッダー情報が .NET XML リソースファイルに追加されるため、既存のファイルにリソースを追加するために使用することはできません。 この例では、SerializedTimeZones ファイルを確認し、存在する場合は、2つのシリアル化されたタイムゾーン以外のすべてのリソースを汎用 <xref:System.Collections.Generic.Dictionary%602> オブジェクトに格納することで、この処理を行います。 その後、既存のファイルが削除され、既存のリソースが新しい SerializedTimeZones ファイルに追加されます。 シリアル化されたタイムゾーンデータもこのファイルに追加されます。

リソースのキー (または**名前**) フィールドには、空白を埋め込むことはできません。 <xref:System.String.Replace%28System.String%2CSystem.String%29> メソッドは、リソースファイルに割り当てられる前に、タイムゾーン識別子内のすべての埋め込みスペースを削除するために呼び出されます。

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- このプロジェクトには、System. .dll と system.servicemodel への参照を追加する必要があります。

- 次の名前空間がインポートされます。

  [!code-csharp[TimeZone2.Serialization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#2)]
  [!code-vb[TimeZone2.Serialization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#2)]

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [タイム ゾーンの概要](../../../docs/standard/datetime/time-zone-overview.md)
- [方法: 埋め込みリソースからタイム ゾーンを復元する](../../../docs/standard/datetime/restore-time-zones-from-an-embedded-resource.md)
