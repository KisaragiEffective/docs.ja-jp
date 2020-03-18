---
title: <nameEntry> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#nameEntry
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/mscorlib/cryptographySettings/cryptoNameMapping/nameEntry
helpviewer_keywords:
- <nameEntry> element
- nameEntry element
ms.assetid: 7d7535e9-4b4a-4b8c-82e2-e40dff5a7821
ms.openlocfilehash: a339638587f8b544bbc1b0073553f6232ce09694
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71699780"
---
# <a name="nameentry-element"></a>\<nameEntry > 要素
アルゴリズムの表示名にクラス名をマップして、1 つのクラスが多くの表示名を持つことを許可します。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp;[ **\<mscorlib >** ](mscorlib-element-for-cryptography-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<cryptographySettings >** ](cryptographysettings-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[ **cryptoNameMapping >** ](cryptonamemapping-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<nameEntry >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<nameEntry name="friendly name" Class="class name" />  
```  
  
## <a name="attributes-and-elements"></a>属性と要素  
 次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|**name**|必須の属性です。<br /><br /> 暗号化クラスが実装するアルゴリズムのフレンドリ名を指定します。|  
|**クラス**|必須の属性です。<br /><br /> [\<cryptoClass >](cryptoclass-element.md)要素の**name**属性の値を指定します。|  
  
### <a name="child-elements"></a>子要素  
 [なし]。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`system.web`|ASP.NET 構成セクションのルート要素を指定します。|  
  
## <a name="remarks"></a>コメント  
 **Name**属性には、<xref:System.Security.Cryptography> 名前空間で見つかった抽象クラスの1つの名前を指定できます。 抽象暗号化クラスで**Create**メソッドを呼び出すと、抽象クラス名が <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A> メソッドに渡されます。 **CreateFromName**は、 **class**属性で示される型のインスタンスを返します。 **Name**属性が RSA などの短い名前である場合は、 **CreateFromName**メソッドを呼び出すときにその名前を使用できます。  
  
## <a name="example"></a>例  
 次の例は、 **\<nameEntry >** 要素を使用して、暗号化クラスを参照し、ランタイムを構成する方法を示しています。 その後、文字列 "RSA" を <xref:System.Security.Cryptography.CryptoConfig.CreateFromName%2A?displayProperty=nameWithType> メソッドに渡し、<xref:System.Security.Cryptography.AsymmetricAlgorithm.Create%2A> メソッドを使用して `MyCryptoRSAClass` オブジェクトを返すことができます。  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass   MyCryptoRSA="MyCryptoRSAClass, MyAssembly  
                  Culture=neutral, PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="RSA" class="MyCryptoRSA"/>  
            <nameEntry name="System.Security.Cryptography.AsymmetricAlgorithm"  
                       class="MyCryptoRSA"/>  
         </cryptoNameMapping>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
## <a name="see-also"></a>参照

- [構成ファイル スキーマ](../index.md)
- [暗号化設定スキーマ](index.md)
- [暗号サービス](../../../../standard/security/cryptographic-services.md)
- [暗号化クラスの設定](../../configure-cryptography-classes.md)
