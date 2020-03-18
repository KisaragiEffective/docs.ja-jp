---
title: <EnableAmPmParseAdjustment> 要素
ms.date: 03/30/2017
ms.assetid: fda998a5-f538-4f8b-a18c-ee7f35e16938
ms.openlocfilehash: 8920e51fcaaca5cb78b80a99ea321163c9b5240f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73117366"
---
# <a name="enableampmparseadjustment-element"></a>\<EnableAmPmParseAdjustment > 要素
日付と時刻の解析メソッドが、日、月、時、および午前/午後の指定子を含む日付文字列を解析するために調整されたルールセットを使用するかどうかを決定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<EnableAmPmParseAdjustment >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<EnableAmPmParseAdjustment enabled="0"|"1" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`enabled`|必須の属性です。<br /><br /> 日付と時刻の解析メソッドが、日、月、時、および午前/午後の指定子のみを含む日付文字列を解析するために調整されたルールセットを使用するかどうかを指定します。|  
  
### <a name="enabled-attribute"></a>enabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|0|日付と時刻の解析メソッドでは、日、月、時、および AM/PM 指定子のみを含む日付文字列を解析するために調整された規則は使用されません。|  
|1|日付と時刻の解析メソッドでは、日、月、時、および AM/PM 指定子のみを含む日付文字列を解析するための調整された規則が使用されます。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|ランタイム初期化オプションに関する情報を含んでいます。|  
  
## <a name="remarks"></a>Remarks  
 `<EnableAmPmParseAdjustment>` 要素は、次のメソッドが日付文字列を解析する方法を制御します。次のメソッドでは、数値の日と月の後に1時間、AM/PM の指定子 ("4/10 6 AM" など) が含まれています。  
  
- <xref:System.DateTime.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTime.TryParse%2A?displayProperty=nameWithType>  
  
- <xref:System.DateTimeOffset.TryParse%2A?displayProperty=nameWithType>  
  
- <xref:System.Convert.ToDateTime%2A?displayProperty=nameWithType>  
  
 その他のパターンは影響を受けません。  
  
 `<EnableAmPmParseAdjustment>` 要素は、<xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType>、<xref:System.DateTime.TryParseExact%2A?displayProperty=nameWithType>、<xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType>、および <xref:System.DateTimeOffset.TryParseExact%2A?displayProperty=nameWithType> の各メソッドには影響しません。  
  
> [!IMPORTANT]
> .NET Core と .NET ネイティブでは、調整された AM/PM 解析規則は既定で有効になっています。  
  
 解析調整規則が有効になっていない場合、文字列の最初の桁は12時間形式の時刻として解釈され、AM/PM 指定子を除く文字列の残りの部分は無視されます。 解析メソッドによって返される日付と時刻は、現在の日付と、日付文字列から抽出された日の時刻で構成されます。  
  
 解析の調整規則が有効になっている場合、解析メソッドは、日付と月を現在の年に属するものとして解釈し、時刻を12時間形式の時間として解釈します。  
  
 次の表は、<xref:System.DateTime.Parse%28System.String%29?displayProperty=nameWithType> メソッドを使用して、`<EnableAmPmParseAdjustment>` 要素の `enabled` プロパティが "0" または "1" に設定されている文字列 "" 4/10 6 AM "を解析する場合の <xref:System.DateTime> 値の違いを示しています。 今日の日付が2017年1月5日であると想定し、指定したカルチャの "G" 書式設定文字列を使用して書式設定されているかのように日付を表示します。  
  
|カルチャ名|enabled = "0"|enabled = "1"|  
|------------------|------------------|------------------|  
|en-US|1/5/2017 4:00:00 AM|4/10/2017 6:00:00 AM|  
|en-GB|5/1/2017 6:00:00|10/4/2017 6:00:00|  
  
## <a name="see-also"></a>関連項目

- [\<ランタイム > 要素](runtime-element.md)
- [\<configuration> 要素](../configuration-element.md)
