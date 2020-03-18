---
title: <iriParsing> 要素 (Uri 設定)
ms.date: 03/30/2017
ms.assetid: 953d0b53-445e-41f9-b302-77c4030852ce
ms.openlocfilehash: fd617d1b4ac8e532c6f9aeaa01465e9866b059e9
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71698089"
---
# <a name="iriparsing-element-uri-settings"></a>\<iriParsing > 要素 (Uri 設定)
International Resource Identifier (IRI) 解析が、<xref:System.Uri> に適用されるかどうか、および IRI の解析規則が適用されるどうかを指定します。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp;[ **\<uri >** ](uri-element-uri-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp; **\<iriParsing >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<iriParsing  
  enabled="true|false"  
/>  
```  
  
## <a name="attributes-and-elements"></a>属性と要素  
 次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|**要素**|**説明**|  
|-----------------|---------------------|  
|`enabled`|IRI 解析を有効にするかどうかを指定します。 既定値は `false` です。|  
  
### <a name="child-elements"></a>子要素  
 なし  
  
### <a name="parent-elements"></a>親要素  
  
|**要素**|**説明**|  
|-----------------|---------------------|  
|[uri](uri-element-uri-settings.md)|.NET Framework が、uniform resource identifier (Uri) を使用して表された web アドレスを処理する方法を指定する設定が含まれます。|  
  
## <a name="remarks"></a>コメント  
 既存の <xref:System.Uri> クラスは .NET Framework 3.5 で拡張されています。 3.0 SP1 および 2.0 SP1 では、International Resource Identifier (IRI) と国際化ドメイン名 (IDN) のサポートが提供されます。 現在のユーザーには、IRI と IDN のサポートを明示的に有効にしない限り、.NET Framework 2.0 の動作からの変更は表示されません。 これにより、.NET Framework の以前のバージョンとのアプリケーションの互換性を保証します。  
  
 IRI のサポートを有効にするには、次の2つの変更が必要です。  
  
1. Machine.config ファイルの .NET Framework 2.0 ディレクトリの下に次の行を追加します。  
  
    ```xml  
    <section name="uri" type="System.Configuration.UriSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
    ```  
  
2. IRI 解析規則を適用するかどうかを指定します。 これは、machine.config ファイルまたは app.config ファイルで指定できます。  
  
 IRI 解析を有効にする (iriParsing enabled = `true`) と、RFC 3987 の最新の IRI 規則に従って正規化と文字チェックが実行されます。 既定値は `false` であり、RFC 2396 および RFC 3986 (IPv6 リテラルの場合) に従って、正規化と文字チェックを行います。  
  
### <a name="configuration-files"></a>構成ファイル  
 この要素は、アプリケーション構成ファイルまたはマシン構成ファイル (Machine.config) で使用できます。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 次の例は、IRI 解析と IDN 名をサポートするために <xref:System.Uri> クラスによって使用される構成を示しています。  
  
### <a name="code"></a>コード  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All" />  
    <iriParsing enabled="true" />  
  </uri>  
</configuration>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection?displayProperty=nameWithType>
- [ネットワーク設定スキーマ](index.md)
