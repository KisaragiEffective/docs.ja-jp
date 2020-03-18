---
title: <TimeSpan_LegacyFormatMode> 要素
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- <TimeSpan_LegacyFormatMode> element
- TimeSpan_LegacyFormatMode element
ms.assetid: 865e7207-d050-4442-b574-57ea29d5e2d6
ms.openlocfilehash: 9d9eedf52f5d711412e4549e39e6ea23abb68ff3
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73968908"
---
# <a name="timespan_legacyformatmode-element"></a>\<TimeSpan_LegacyFormatMode > 要素

<xref:System.TimeSpan?displayProperty=nameWithType> 値を持つ書式設定操作で、ランタイムが従来の動作を保持するかどうかを決定します。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<TimeSpan_LegacyFormatMode >**  

## <a name="syntax"></a>構文

```xml
<TimeSpan_LegacyFormatMode
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br /> ランタイムが <xref:System.TimeSpan?displayProperty=nameWithType> 値に対して従来の書式設定動作を使用するかどうかを指定します。|

## <a name="enabled-attribute"></a>enabled 属性

|[値]|説明|
|-----------|-----------------|
|`false`|ランタイムでは、従来の書式設定動作は復元されません。|
|`true`|ランタイムは、従来の書式設定動作を復元します。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|ランタイム初期化オプションに関する情報を含んでいます。|

## <a name="remarks"></a>Remarks

.NET Framework 4 以降、<xref:System.TimeSpan?displayProperty=nameWithType> 構造体は <xref:System.IFormattable> インターフェイスを実装し、標準およびカスタムの書式指定文字列を使用した書式設定操作をサポートします。 解析メソッドで、サポートされていない書式指定子または書式指定文字列が検出されると、<xref:System.FormatException>がスローされます。

以前のバージョンの .NET Framework では、<xref:System.TimeSpan> 構造体は <xref:System.IFormattable> を実装しておらず、書式指定文字列をサポートしていませんでした。 しかし、多くの開発者は、<xref:System.TimeSpan> が一連の書式指定文字列をサポートし、<xref:System.String.Format%2A?displayProperty=nameWithType>などのメソッドを使用して[複合書式指定操作](../../../../standard/base-types/composite-formatting.md)で使用したことを誤って想定していました。 通常、型が <xref:System.IFormattable> を実装し、書式指定文字列をサポートしている場合、サポートされていない書式指定文字列を使用した書式指定メソッドの呼び出しは、通常、<xref:System.FormatException>をスローします。 ただし、<xref:System.TimeSpan> は <xref:System.IFormattable>を実装しなかったため、ランタイムは書式設定文字列を無視し、代わりに <xref:System.TimeSpan.ToString?displayProperty=nameWithType> メソッドと呼ばれます。 これは、書式指定文字列が書式設定操作に影響を与えないことを意味しますが、その存在は <xref:System.FormatException>になりませんでした。

レガシコードによって複合書式指定メソッドと無効な書式指定文字列が渡され、そのコードを再コンパイルできない場合は、`<TimeSpan_LegacyFormatMode>` 要素を使用して、従来の <xref:System.TimeSpan> の動作を復元できます。 この要素の `enabled` 属性を `true`に設定すると、複合書式指定メソッドによって <xref:System.TimeSpan.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType>ではなく <xref:System.TimeSpan.ToString?displayProperty=nameWithType> が呼び出され、<xref:System.FormatException> はスローされません。

## <a name="example"></a>例

次の例では、<xref:System.TimeSpan> オブジェクトをインスタンス化し、サポートされていない標準書式指定文字列を使用して、<xref:System.String.Format%28System.String%2CSystem.Object%29?displayProperty=nameWithType> メソッドで書式設定しようとしています。

[!code-csharp[TimeSpan.BreakingChanges#1](../../../../../samples/snippets/csharp/VS_Snippets_CLR/timespan.breakingchanges/cs/legacyformatmode1.cs#1)]
[!code-vb[TimeSpan.BreakingChanges#1](../../../../../samples/snippets/visualbasic/VS_Snippets_CLR/timespan.breakingchanges/vb/legacyformatmode1.vb#1)]

.NET Framework 3.5 またはそれ以前のバージョンでこの例を実行すると、次の出力が表示されます。

```console
12:30:45
```

これは、.NET Framework 4 以降のバージョンで例を実行した場合の出力とは大きく異なります。

```console
Invalid Format
```

ただし、次の構成ファイルを例のディレクトリに追加した後、.NET Framework 4 以降のバージョンでこの例を実行すると、出力は、.NET Framework 3.5 で実行されたときに例で生成されたものと同じになります。

```xml
<?xml version ="1.0"?>
<configuration>
   <runtime>
      <TimeSpan_LegacyFormatMode enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
