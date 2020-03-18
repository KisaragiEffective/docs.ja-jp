---
title: <UseSmallInternalThreadStacks> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- UseSmallInternalThreadStacks element
- <UseSmallInternalThreadStacks> element
ms.assetid: 1e3f6ec0-1cac-4e1c-9c81-17d948ae5874
ms.openlocfilehash: 2fd776ce8605e6dcf288dcb3852ded16638a1873
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73114918"
---
# <a name="usesmallinternalthreadstacks-element"></a>\<UseSmallInternalThreadStacks > 要素
共通言語ランタイム (CLR) が、内部で使用する特定のスレッドを作成するときに明示的なスタックサイズを指定することによって、メモリの使用量を削減するように要求します。これらのスレッドの既定のスタックサイズは使用されません。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<UseSmallInternalThreadStacks>**  
  
## <a name="syntax"></a>構文  
  
```xml  
<UseSmallInternalThreadStacks enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|enabled|必須の属性です。<br /><br /> 内部で使用する特定のスレッドを作成するときに、CLR が既定のスタックサイズではなく明示的なスタックサイズを使用するように要求するかどうかを指定します。 明示的なスタックサイズは、既定のスタックサイズの 1 MB よりも小さくなっています。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|true|明示的なスタックサイズを要求します。|  
|False|既定のスタックサイズを使用します。 これは .NET Framework 4 の既定値です。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="remarks"></a>Remarks  
 この構成要素は、プロセスでの仮想メモリの使用量の削減を要求するために使用されます。これは、CLR が内部スレッドに使用する明示的なスレッドサイズ (要求が受け入れられた場合) が既定のサイズよりも小さいためです。  
  
> [!IMPORTANT]
> この構成要素は、絶対的な要件ではなく、CLR への要求です。 .NET Framework 4 では、要求は x86 アーキテクチャに対してのみ受け入れられます。 この要素は、CLR の将来のバージョンでは完全に無視される場合もあれば、選択した内部スレッドで常に使用される明示的なスタックサイズに置き換えられる場合もあります。  
  
 この構成要素を指定すると、CLR が要求を受け入れた場合に、より小さなスタックサイズによってスタックオーバーフローが発生する可能性があるため、仮想メモリの使用量が少なくなります。  
  
## <a name="example"></a>例  
 次の例は、CLR が内部で使用する特定のスレッドに対して明示的なスタックサイズを使用するように要求する方法を示しています。  
  
```xml  
<configuration>  
   <runtime>  
      <UseSmallInternalThreadStacks enabled="true" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
