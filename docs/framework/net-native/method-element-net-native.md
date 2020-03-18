---
title: <Method> 要素 (.NET ネイティブ)
ms.date: 03/30/2017
ms.assetid: 348b49e5-589d-4eb2-a597-d6ff60ab52d1
ms.openlocfilehash: 7b0e77e6dea29cbd5218ab3f6f992002efd51656
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128335"
---
# <a name="method-element-net-native"></a>\<メソッド > 要素 (.NET ネイティブ)
コンストラクターまたはメソッドにランタイム リフレクション ポリシーを適用します。  
  
## <a name="syntax"></a>構文  
  
```xml  
<Method Name="method_name"  
        Signature="method_signature"  
        Browse="policy_type"  
        Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|属性の型|説明|  
|---------------|--------------------|-----------------|  
|`Name`|全般|必須の属性です。 メソッド名を指定します。|  
|`Signature`|全般|省略可能な属性です。 メソッド シグネチャを指定します。 複数のパラメーターが存在する場合はコンマで区切られます。 たとえば、次の `<Method>` 要素は <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29> メソッドのポリシーを定義します。<br /><br /> `<Type Name="System.DateTime">    <Method Name="ToString" Signature="System.String,System.IFormatProvider"            Dynamic="Required" /> </Type>`<br /><br /> 属性が存在しない場合、ランタイム ディレクティブは、メソッドのすべてのオーバーロードに適用されます。|  
|`Browse`|リフレクション|省略可能な属性です。 メソッドに関する情報の照会やメソッドの列挙を制御しますが、実行時の動的呼び出しは有効にしません。|  
|`Dynamic`|リフレクション|省略可能な属性です。 コンストラクターまたはメソッドへの実行時アクセスを制御して、動的プログラミングを有効にします。 このポリシーにより、実行時にメンバーを動的に呼び出すことができます。|  
  
## <a name="name-attribute"></a>Name 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*method_name*|メソッド名。 メソッドの型は、親の [\<Type>](type-element-net-native.md) 要素または [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 要素により定義されます。|  
  
## <a name="signature-attribute"></a>シグネチャ属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*method_signature*|メソッド シグネチャを形成するパラメーター型です。 複数のパラメーターは、`"System.String,System.Int32,System.Int32)"` のようにコンマで区切ります。 パラメーターの型名は完全修飾されている必要があります。|  
  
## <a name="all-other-attributes"></a>その他すべての属性  
  
|[値]|説明|  
|-----------|-----------------|  
|*policy_setting*|このポリシーの種類に適用する設定です。 指定できる値は、`Auto`、`Excluded`、`Included`、および `Required` です。 詳細については、「[ランタイム ディレクティブのポリシー設定](runtime-directive-policy-settings.md)」を参照してください。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<Parameter>](parameter-element-net-native.md)|メソッドに渡された引数の型にポリシーを適用します。|  
|[\<GenericParameter>](genericparameter-element-net-native.md)|ジェネリック型またはメソッドのパラメーターの型にポリシーを適用します。|  
|[\<ImpliesType>](impliestype-element-net-native.md)|型にポリシーを適用します (含んでいる `<Method>` 要素によって表されるメソッドにそのポリシーが適用されている場合)。|  
|[\<TypeParameter>](typeparameter-element-net-native.md)|メソッドに渡された <xref:System.Type> 引数によって表される型にポリシーを適用します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|型とそのすべてのメンバーにリフレクション ポリシーを適用します。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|構築されたジェネリック型とそのすべてのメンバーにリフレクション ポリシーを適用します。|  
  
## <a name="remarks"></a>Remarks  
 ジェネリック メソッドの `<Method>` 要素は、独自のポリシーを持たないインスタンス化すべてにそのポリシーを適用します。  
  
 `Signature` 属性を使用して、特定のメソッド オーバーロードのポリシーを指定できます。 そうしない場合、`Signature` 属性が存在しないと、メソッドのすべてのオーバーロードにランタイム ディレクティブが適用されます。  
  
 `<Method>` 要素を使用してコンストラクターのランタイム リフレクション ポリシーを定義することはできません。 代わりに、[\<Assembly>](assembly-element-net-native.md) 要素、[\<Namespace>](namespace-element-net-native.md) 要素、[\<Type>](type-element-net-native.md) 要素、または [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 要素の `Activate` 属性を使います。  
  
## <a name="example"></a>例  
 次の例の `Stringify` メソッドは、リフレクションを使用してオブジェクトを文字列形式に変換する汎用書式設定メソッドです。 オブジェクトの既定の `ToString` メソッドを呼び出すことに加えて、このメソッドでは、オブジェクトの `ToString` メソッドに書式文字列、<xref:System.IFormatProvider> 実装、またはその両方を渡して、書式設定された結果文字列を生成できます。 また、数値をバイナリ、16 進数、または 8 進数形式に変換するいずれかの <xref:System.Convert.ToString%2A?displayProperty=nameWithType> オーバーロードを呼び出すこともできます。  
  
 [!code-csharp[ProjectN_Reflection#7](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/method1.cs#7)]  
  
 `Stringify` メソッドは、次のようなコードによって呼び出すことができます。  
  
 [!code-csharp[ProjectN_Reflection#7](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/method1.cs#7)]  
  
 ただし、.NET ネイティブでコンパイルした場合、この例は <xref:System.NullReferenceException> や [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) などの例外を実行時にスローする可能性があります。このことは、`Stringify` メソッドが、主に .NET Framework クラス ライブラリでプリミティブ型の動的な書式設定をサポートするものであるために発生します。 ただし、既定のディレクティブ ファイルではそのメタデータを使用できません。 メタデータが使用できたとしても、適切な `ToString` の実装がネイティブ コードに含まれていないため、この例は [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 例外をスローします。  
  
 これらの例外はすべて、[\<Type>](type-element-net-native.md) 要素を使用してメタデータが存在する必要がある型を定義し、`<Method>` 要素を追加して動的に呼び出せるメソッド オーバーロードの実装も必ず存在するようにすることで排除できます。 これらの例外を排除し、例をエラーなしで実行できるようにした default.rd.xml ファイルを次に示します。  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
  <Application>  
     <Assembly Name="*Application*" Dynamic="Required All" />  
  
     <Type Name = "System.Convert" Browse="Required Public" Dynamic="Required Public" >  
        <Method Name="ToString"    Dynamic ="Required" />  
     </Type>  
     <Type Name="System.Double" Browse="Required Public">  
        <Method Name="ToString" Dynamic="Required" />  
     </Type>  
     <Type Name ="System.Int32" Browse="Required Public" >  
        <Method Name="ToString" Dynamic="Required" />  
     </Type>  
     <Type Name ="System.Int64" Browse="Required Public" >  
        <Method Name="ToString" Dynamic="Required" />  
     </Type>  
     <Namespace Name="System" >  
        <Type Name="Byte" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="DateTime" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Decimal" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Guid" Browse ="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Int16" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="SByte" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Single" Browse="Required Public" >  
          <Method Name="ToString" Dynamic="Required" />           
        </Type>  
        <Type Name="TimeSpan" Browse="Required Public" >  
          <Method Name="ToString" Dynamic="Required" />           
        </Type>  
        <Type Name="UInt16" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="UInt32" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="UInt64" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
     </Namespace>  
  </Application>  
</Directives>  
```  
  
## <a name="see-also"></a>関連項目

- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
- [ランタイム ディレクティブ ポリシーの設定](runtime-directive-policy-settings.md)
- [\<MethodInstantiation> 要素](methodinstantiation-element-net-native.md)
