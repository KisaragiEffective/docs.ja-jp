---
title: <enforceFIPSPolicy> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- enforceFIPSPolicy element
- FIPS
- <enforceFIPSPolicy> element
- Federal Information Processing Standards (FIPS)
ms.assetid: c35509c4-35cf-43c0-bb47-75e4208aa24e
ms.openlocfilehash: 0d6dd291a24928487a040c0427f058dee80bf836
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73117386"
---
# <a name="enforcefipspolicy-element"></a>\<enforceFIPSPolicy > 要素
暗号化アルゴリズムが連邦情報処理規格 (FIPS: Federal Information Processing Standard) に準拠する必要があるコンピューターの構成要件を強制するかどうかを指定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<enforceFIPSPolicy >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<enforceFIPSPolicy enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|enabled|必須の属性です。<br /><br /> 暗号化アルゴリズムが FIPS に準拠している必要があるコンピューター構成要件の適用を有効にするかどうかを指定します。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|`true`|暗号化アルゴリズムが FIPS に準拠するようにコンピューターが構成されている場合は、その要件が適用されます。 クラスが FIPS に準拠していないアルゴリズムを実装している場合、そのクラスのコンストラクターまたは `Create` メソッドは、そのコンピューターで実行されるときに例外をスローします。 既定値です。|  
|`false`|アプリケーションで使用される暗号化アルゴリズムは、コンピューターの構成に関係なく、FIPS に準拠している必要はありません。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="remarks"></a>Remarks  
 .NET Framework 2.0 以降では、暗号化アルゴリズムを実装するクラスの作成は、コンピューターの構成によって制御されます。 アルゴリズムが FIPS に準拠していることを必要とするようにコンピューターが構成されており、クラスが FIPS に準拠していないアルゴリズムを実装している場合、そのクラスのインスタンスを作成しようとすると、例外がスローされます。 コンストラクターは <xref:System.InvalidOperationException> 例外をスローし、`Create` メソッドは、内部 <xref:System.InvalidOperationException> 例外を使用して <xref:System.Reflection.TargetInvocationException> 例外をスローします。  
  
 構成が FIPS に準拠している必要があり、アプリケーションで fips に準拠していないアルゴリズムが使用されているコンピューターでアプリケーションを実行する場合は、構成ファイルでこの要素を使用して、共通言語ランタイム (CLR) がFIPS 準拠の強制。 この要素は、.NET Framework 2.0 Service Pack 1 で導入されました。  
  
## <a name="example"></a>例  
 次の例は、CLR が FIPS 準拠を強制しないようにする方法を示しています。  
  
```xml  
<configuration>  
    <runtime>  
        <enforceFIPSPolicy enabled="false"/>  
    </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [暗号モデル](../../../../standard/security/cryptography-model.md)
