---
title: dateTimeInvalidLocalFormat MDA
ms.date: 03/30/2017
helpviewer_keywords:
- dates [.NET Framework], formatting
- invalid date time local format
- invalid local formats
- MDAs (managed debugging assistants), invalid local formats
- managed debugging assistants (MDAs), invalid local formats
- dateTimeInvalidLocalFormat MDA
- date formatting
- time formatting
- UTC formatting
ms.assetid: c4a942bb-2651-4b65-8718-809f892a0659
ms.openlocfilehash: 2fdace8a9c7bcc090fd801be3bd717e4a2b34a87
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217548"
---
# <a name="datetimeinvalidlocalformat-mda"></a>dateTimeInvalidLocalFormat MDA
`dateTimeInvalidLocalFormat` MDA は、世界協定時刻 (UTC) として格納されている <xref:System.DateTime> インスタンスが、ローカル <xref:System.DateTime> インスタンス専用の形式で書式指定されたときにアクティブになります。 この MDA は、未指定または既定の <xref:System.DateTime> インスタンスに対してはアクティブになりません。  
  
## <a name="symptom"></a>現象  
 アプリケーションで、次のようなローカル形式を使用して UTC <xref:System.DateTime> インスタンスを手動でシリアル化しています。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
Serialize(myDateTime.ToString("yyyy-MM-dd'T'HH:mm:ss.fffffffzzz"));  
```  
  
### <a name="cause"></a>原因  
 <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> メソッドの 'z' 形式には、ローカル タイム ゾーン オフセット (たとえば、シドニー時間の場合は "+10:00") が含まれます。 そのため、<xref:System.DateTime> の値がローカルの場合にのみ、有意な結果が生成されます。 値が UTC 時刻の場合、<xref:System.DateTime.ToString%2A?displayProperty=nameWithType> には、ローカル タイム ゾーン オフセットが含まれますが、タイム ゾーン指定子の表示や調整は行われません。  
  
### <a name="resolution"></a>解決策  
 UTC <xref:System.DateTime> インスタンスが UTC であることを示すように書式設定する必要があります。 UTC 時刻の形式としては、次のように 'Z' を使用して UTC 時刻を示すようにすることをお勧めします。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
Serialize(myDateTime.ToString("yyyy-MM-dd'T'HH:mm:ss.fffffffZ"));  
```  
  
 また、インスタンスがローカル、UTC、未指定のいずれの場合でも正しくシリアル化する <xref:System.DateTime> プロパティを利用して <xref:System.DateTime.Kind%2A> をシリアル化する "o" 形式もあります。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
Serialize(myDateTime.ToString("o"));  
```  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA はランタイムに影響しません。  
  
## <a name="output"></a>出力  
 この MDA がアクティブになっても特別な出力は生成されませんが、呼び出し履歴を使用すると、この MDA をアクティブにした <xref:System.DateTime.ToString%2A> 呼び出しの位置を確認できます。  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dateTimeInvalidLocalFormat />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 次のように、<xref:System.DateTime> または <xref:System.Xml.XmlConvert> クラスを使用して UTC <xref:System.Data.DataSet> 値を間接的にシリアル化するアプリケーションについて考えてみます。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
String serialized = XMLConvert.ToString(myDateTime);  
```  
  
 <xref:System.Xml.XmlConvert> と <xref:System.Data.DataSet> のシリアル化では、既定でシリアル化用のローカル形式を使用します。 UTC など、他の種類の <xref:System.DateTime> 値をシリアル化する場合は、追加のオプションが必要です。  
  
 この特定の例では、`XmlDateTimeSerializationMode.RoundtripKind` での `ToString` 呼び出しに `XmlConvert` を渡します。 これで、データが UTC 時刻としてシリアル化されます。  
  
 <xref:System.Data.DataSet> を使用する場合は、<xref:System.Data.DataColumn.DateTimeMode%2A> オブジェクトの <xref:System.Data.DataColumn> プロパティを <xref:System.Data.DataSetDateTime.Utc> に設定します。  
  
```csharp
DateTime myDateTime = DateTime.UtcNow;  
String serialized = XmlConvert.ToString(myDateTime,   
    XmlDateTimeSerializationMode.RoundtripKind);  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Globalization.DateTimeFormatInfo>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
