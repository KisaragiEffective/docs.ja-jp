---
title: <MethodInstantiation> 要素 (.NET ネイティブ)
ms.date: 03/30/2017
ms.assetid: a3355d78-2a88-4109-8521-830d7cae260a
ms.openlocfilehash: f19bd3c20088431bcbbafac298398b82a664bee9
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128320"
---
# <a name="methodinstantiation-element-net-native"></a>\<MethodInstantiation 化 > 要素 (.NET ネイティブ)
構築されたジェネリック メソッドにランタイム リフレクション ポリシーを適用します。  
  
## <a name="syntax"></a>構文  
  
```xml  
<MethodInstantiation Name="method_name"  
                     Signature="method_signature"  
                     Arguments="method_arguments"  
                     Browse="policy_type"  
                     Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|属性の型|説明|  
|---------------|--------------------|-----------------|  
|`Name`|全般|必須の属性です。 メソッド名を指定します。|  
|`Signature`|全般|省略可能な属性です。 メソッドの名前付きパラメーターを指定します。 複数の名前付きパラメーターはコンマで区切られます。 `Signature` 属性は、オーバー ロードされたメソッドを区別するために使用します。|  
|`Arguments`|全般|必須の属性です。 ジェネリック型引数を指定します。 複数の引数が存在する場合は、コンマで区切られます。|  
|`Browse`|リフレクション|省略可能な属性です。 メソッドに関する情報の照会やメソッドの列挙を制御しますが、実行時の動的呼び出しは有効にしません。|  
|`Dynamic`|リフレクション|省略可能な属性です。 コンストラクターまたはメソッドへの実行時アクセスを制御して、動的プログラミングを有効にします。 このポリシーにより、実行時にメンバーを動的に呼び出すことができます。|  
  
## <a name="name-attribute"></a>Name 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*method_name*|メソッド名。 メソッドの型は、親の [\<Type>](type-element-net-native.md) 要素または [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 要素により定義されます。|  
  
## <a name="signature-attribute"></a>シグネチャ属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*method_signature*|メソッドの名前付きパラメーターを指定します。 複数のパラメーターが存在する場合はコンマで区切られます。|  
  
## <a name="arguments-attribute"></a>引数属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*method_arguments*|ジェネリック型引数を指定します。 複数の引数が存在する場合は、コンマで区切られます。 各引数は、完全修飾型名で構成されている必要があります。|  
  
## <a name="all-other-attributes"></a>その他すべての属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*policy_setting*|メソッドのこのポリシーの種類に適用する設定です。 指定できる値は、`Auto`、`Excluded`、`Included`、および `Required` です。 詳細については、「[ランタイム ディレクティブのポリシー設定](runtime-directive-policy-settings.md)」を参照してください。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|型とそのすべてのメンバーにリフレクション ポリシーを適用します。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|構築されたジェネリック型とそのすべてのメンバーにリフレクション ポリシーを適用します。|  
  
## <a name="remarks"></a>Remarks  
 `<MethodInstantiation>` 要素は、対応するオープン ジェネリック メソッドのランタイム リフレクション ポリシーをオーバーライドします。  
  
## <a name="see-also"></a>関連項目

- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
- [ランタイム ディレクティブ ポリシーの設定](runtime-directive-policy-settings.md)
- [\<Method> 要素](method-element-net-native.md)
