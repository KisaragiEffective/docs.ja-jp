---
title: <Application> 要素 (.NET ネイティブ)
ms.date: 03/30/2017
ms.assetid: b4e9b37a-059b-4076-8f56-cb3f9cef0cd9
ms.openlocfilehash: e26826b3d8674b536ab0897182da58bc02cfd00b
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128521"
---
# <a name="application-element-net-native"></a>\<Application > 要素 (.NET ネイティブ)
実行時にリフレクションに使用可能なメタデータを持つアプリケーション全体の型と型のメンバーのコンテナーとして機能し、アプリ内のすべてのプログラム要素にランタイム リフレクション ポリシーを適用します。  
  
 \<Directives> 要素  
\<Application> 要素 (rd.xml)  
  
## <a name="syntax"></a>構文  
  
```xml  
<Application Activate="policy_setting"  
             Browse="policy_setting"  
             Dynamic="policy_setting"  
             Serialize="policy_setting"  
             DataContractSerializer="policy_setting"  
             DataContractJsonSerializer="policy_setting"  
             XmlSerializer="policy_setting"  
             MarshalObject="policy_setting"  
             MarshalDelegate="policy_setting"  
             MarshalStructure="policy_setting" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。 「子要素」の表では、ポリシーは実行時に特定のプログラム要素で使用可能になるメタデータの種類を示します。  
  
### <a name="attributes"></a>属性  
  
|属性|属性の型|説明|  
|---------------|--------------------|-----------------|  
|`Activate`|リフレクション|省略可能な属性です。 コンストラクターへの実行時アクセスを制御して、インスタンスのアクティブ化を有効にします。|  
|`Browse`|リフレクション|省略可能な属性です。 型に関する情報の照会や型の列挙を制御しますが、実行時の動的アクセスは有効にしません。|  
|`Dynamic`|リフレクション|省略可能な属性です。 コンストラクター、メソッド、フィールド、プロパティ、およびイベントを含むすべての型のメンバーへの実行時アクセスを制御して、動的プログラミングを有効にします。|  
|`Serialize`|シリアル化|省略可能な属性です。 コンストラクター、フィールド、およびプロパティへの実行時アクセスを制御し、Newtonsoft の JSON シリアライザーなどのライブラリによって型インスタンスをシリアル化および逆シリアル化できるようにします。|  
|`DataContractSerializer`|シリアル化|省略可能な属性です。 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> クラスを使用するシリアル化のポリシーを制御します。|  
|`DataContractJsonSerializer`|シリアル化|省略可能な属性です。 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType> クラスを使用する JSON シリアル化のポリシーを制御します。|  
|`XmlSerializer`|シリアル化|省略可能な属性です。 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> クラスを使用する XML シリアル化のポリシーを制御します。|  
|`MarshalObject`|Interop|省略可能な属性です。 Windows ランタイムと COM に参照型をマーシャリングするためのポリシーを制御します。|  
|`MarshalDelegate`|Interop|省略可能な属性です。 ネイティブ コードへの関数ポインターとしてデリゲート型をマーシャリングするためのポリシーを制御します。|  
|`MarshalStructure`|Interop|省略可能な属性です。 ネイティブ コードに構造体をマーシャリングするためのポリシーを制御します。|  
  
## <a name="all-attributes"></a>すべての属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*policy_setting*|アプリで型に適用する、このポリシーの設定です。 指定できる値は、`All`、`Auto`、`Excluded`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal`、および `Required All` です。 詳細については、「[ランタイム ディレクティブのポリシー設定](runtime-directive-policy-settings.md)」を参照してください。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<Assembly>](assembly-element-net-native.md)|特定のアセンブリ内のすべての型にポリシーを適用します。|  
|[\<Namespace>](namespace-element-net-native.md)|特定の名前空間内のすべての型にポリシーを適用します。|  
|[\<Type>](type-element-net-native.md)|クラスや構造体などの特定の型にポリシーを適用します。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|構築されたジェネリック型にポリシーを適用します。 たとえば、[\<TypeInstantiation>](typeinstantiation-element-net-native.md) 要素を使用して `List<String>` 型のポリシーを定義できます。|  
|[\<Method>](method-element-net-native.md)|特定の型のメソッドにポリシーを適用します。|  
|[\<MethodInstantiation>](methodinstantiation-element-net-native.md)|構築されたジェネリック メソッドにポリシーを適用します。|  
|[\<Property>](property-element-net-native.md)|特定の型のプロパティにポリシーを適用します。|  
|[\<Field>](field-element-net-native.md)|特定の型のフィールドにポリシーを適用します。|  
|[\<Event>](event-element-net-native.md)|特定の型のイベントにポリシーを適用します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<Directives>](directives-element-net-native.md)|ランタイム ディレクティブ ファイルのルート要素です。|  
  
## <a name="remarks"></a>Remarks  
 [\<Directives>](directives-element-net-native.md) 要素には、0 または 1 個の `<Application>` 要素を含めることができます。 1 つのリフレクション ディレクティブ ファイルに複数の `<Application>` 要素を含めることはサポートされていません。  
  
 `<Application>` 要素は、次の 2 とおりの方法で使用できます。  
  
- 実行時に必要なメタデータを持つプログラム要素を定義するためのコンテナーとして。 この場合、`<Application>` 要素に属性は必要ありません。 コンパイル時に、コンパイラ ツールは、.NET Framework コア ライブラリを含むすべてのライブラリで、`<Application>` 要素の子要素により示されるプログラム要素を検索します。 一方、[\<Library>](library-element-net-native.md) の子要素により示されるプログラム要素を検索する場合、コンパイラ ツールは [\<Library>](library-element-net-native.md) 要素により指定されたライブラリのみを検索します。  
  
- リフレクション、シリアル化、および相互運用に関するアプリケーション全体のポリシーを設定する要素として。 `<Application>` 要素の属性はアプリケーション全体のポリシーを定義します。このポリシーは、`<Application>` 要素または [\<Library>](library-element-net-native.md) 要素により定義される子要素によってオーバーライドできます。  
  
## <a name="see-also"></a>関連項目

- [\<ライブラリ > 要素](library-element-net-native.md)
- [\<ディレクティブ > 要素](directives-element-net-native.md)
- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
