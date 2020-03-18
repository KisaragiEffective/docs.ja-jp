---
title: '方法: 埋め込みリソースからタイムゾーンを復元する'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], deserializing
- time zones [.NET Framework], restoring
ms.assetid: 6b7b4de9-da07-47e3-8f4c-823f81798ee7
ms.openlocfilehash: cd2119d6d22f3f676b7167ed545aeb058123cfb5
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122203"
---
# <a name="how-to-restore-time-zones-from-an-embedded-resource"></a>方法: 埋め込みリソースからタイムゾーンを復元する

このトピックでは、リソースファイルに保存されているタイムゾーンを復元する方法について説明します。 タイムゾーンの保存に関する情報および手順については、「[方法: 埋め込みリソースにタイムゾーンを保存する](../../../docs/standard/datetime/save-time-zones-to-an-embedded-resource.md)」を参照してください。

### <a name="to-deserialize-a-timezoneinfo-object-from-an-embedded-resource"></a>埋め込みリソースから TimeZoneInfo オブジェクトを逆シリアル化するには

1. 取得するタイムゾーンがカスタムタイムゾーンでない場合は、<xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> メソッドを使用してインスタンス化してみてください。

2. 埋め込みリソースファイルの完全修飾名と、そのリソースファイルを含むアセンブリへの参照を渡すことによって、<xref:System.Resources.ResourceManager> オブジェクトをインスタンス化します。

   埋め込まれたリソースファイルの完全修飾名を特定できない場合は、 [ildasm.exe (IL 逆アセンブラー)](../../../docs/framework/tools/ildasm-exe-il-disassembler.md)を使用して、アセンブリのマニフェストを確認します。 リソースを識別する `.mresource` エントリ。 この例では、リソースの完全修飾名は `SerializeTimeZoneData.SerializedTimeZones`です。

   リソースファイルがタイムゾーンのインスタンス化コードを含む同じアセンブリに埋め込まれている場合は、`static` (Visual Basic では`Shared`) <xref:System.Reflection.Assembly.GetExecutingAssembly%2A> メソッドを呼び出すことによって、そのファイルへの参照を取得できます。

3. <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> メソッドの呼び出しが失敗した場合、またはカスタムタイムゾーンをインスタンス化する場合は、<xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType> メソッドを呼び出して、シリアル化されたタイムゾーンを含む文字列を取得します。

4. <xref:System.TimeZoneInfo.FromSerializedString%2A> メソッドを呼び出してタイムゾーンデータを逆シリアル化します。

## <a name="example"></a>例

次の例では、埋め込み .NET XML リソースファイルに格納されている <xref:System.TimeZoneInfo> オブジェクトを逆シリアル化します。

[!code-csharp[TimeZone2.Serialization#3](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#3)]
[!code-vb[TimeZone2.Serialization#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#3)]

このコードは、アプリケーションに必要な <xref:System.TimeZoneInfo> オブジェクトが存在することを確認するための例外処理を示しています。 まず、<xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> メソッドを使用してレジストリから取得することで、<xref:System.TimeZoneInfo> オブジェクトのインスタンス化を試みます。 タイムゾーンをインスタンス化できない場合、コードは埋め込みリソースファイルからタイムゾーンを取得します。

カスタムタイムゾーン (<xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> メソッドを使用してインスタンス化されたタイムゾーン) のデータはレジストリに格納されないため、コードは、南極のタイムゾーンをインスタンス化するための <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> を呼び出しません。 代わりに、埋め込みリソースファイルをすぐに検索して、<xref:System.TimeZoneInfo.FromSerializedString%2A> メソッドを呼び出す前にタイムゾーンのデータを含む文字列を取得します。

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- このプロジェクトには、System. .dll と system.servicemodel への参照を追加する必要があります。

- 次の名前空間がインポートされます。

  [!code-csharp[TimeZone2.Serialization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#2)]
  [!code-vb[TimeZone2.Serialization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#2)]

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [タイム ゾーンの概要](../../../docs/standard/datetime/time-zone-overview.md)
- [方法: 埋め込みリソースにタイム ゾーンを保存する](../../../docs/standard/datetime/save-time-zones-to-an-embedded-resource.md)
