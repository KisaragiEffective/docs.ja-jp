---
title: リフレクションに依存する API
ms.date: 03/30/2017
ms.assetid: f9532629-6594-4a41-909f-d083f30a42f3
ms.openlocfilehash: 7329ac339912042fc5d2fb335faa3bf74ed03b8d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128538"
---
# <a name="apis-that-rely-on-reflection"></a>リフレクションに依存する API
場合によっては、コードでのリフレクションの使用は明確ではないため、.NET ネイティブツールチェーンは実行時に必要なメタデータを保持しません。 このトピックでは、リフレクション API の一部であるとは見なされないが、正常に実行するためにリフレクションを必要とする、一般的な API と一般的なプログラミング パターンについて説明します。 これらをソース コードで使用している場合、これらに関する情報をランタイム ディレクティブ (.rd.xml) ファイルに追加して、これらの API を呼び出しても [MissingMetadataException](missingmetadataexception-class-net-native.md) 例外やその他の例外が実行時にスローされないようにできます。  
  
## <a name="typemakegenerictype-method"></a>Type.MakeGenericType メソッド  
 ジェネリック型 `AppClass<T>` を動的にインスタンス化するには、次のようなコードを使用して <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> メソッドを呼び出します。  
  
 [!code-csharp[ProjectN#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/type_makegenerictype1.cs#1)]  
  
 このコードが実行時に成功するためには、いくつかのメタデータ項目が必要です。 1 つ目は、インスタンス化されていないジェネリック型 `Browse` の `AppClass<T>` メタデータです。  
  
```xml  
<Type Name="App1.AppClass`1" Browse="Required PublicAndInternal" />  
```  
  
 これにより、<xref:System.Type.GetType%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> メソッド呼び出しが成功し、有効な <xref:System.Type> オブジェクトが返されます。  
  
 ただし、インスタンス化されていないジェネリック型のメタデータを追加した場合でも、<xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> メソッドを呼び出すと、次のような [MissingMetadataException](missingmetadataexception-class-net-native.md) 例外がスローされます。  
  
パフォーマンス上の理由から、次の種類のメタデータが削除されたため、この操作を実行できません:  
  
`App1.AppClass`1 < system.string > ' です。  
  
 次のランタイム ディレクティブをランタイム ディレクティブ ファイルに追加すると、`Activate` の `AppClass<T>` に対する特定のインスタンス化の <xref:System.Int32?displayProperty=nameWithType> メタデータを追加できます。  
  
```xml  
<TypeInstantiation Name="App1.AppClass" Arguments="System.Int32"   
                   Activate="Required Public" />  
```  
  
 `AppClass<T>` に対するそれぞれ異なるインスタンス化は、<xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> メソッドにより作成され、静的に使用されていない場合、個別のディレクティブを必要とします。  
  
## <a name="methodinfomakegenericmethod-method"></a>MethodInfo.MakeGenericMethod メソッド  
 ジェネリック メソッド `Class1` を持つクラス `GetMethod<T>(T t)` があるとすると、`GetMethod` は、次のようなコードを使用してリフレクションにより呼び出すことができます。  
  
 [!code-csharp[ProjectN#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/makegenericmethod1.cs#2)]  
  
 このコードを正常に実行するには、次のようないくつかのメタデータ項目が必要です。  
  
- 呼び出すメソッドを持つ型の `Browse` メタデータ。  
  
- 呼び出すメソッドの `Browse` メタデータ。  パブリック メソッドの場合、それを含む型のパブリック `Browse` メタデータを追加しても、メソッドが含められます。  
  
- リフレクション呼び出しデリゲートが .NET ネイティブツールチェーンによって削除されないようにするために、呼び出すメソッドの動的メタデータ。 メソッドの動的メタデータが欠落している場合、<xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType> メソッドを呼び出すと、次の例外がスローされます。  
  
    ```output
    MakeGenericMethod() cannot create this generic method instantiation because the instantiation was not metadata-enabled: 'App1.Class1.GenMethod<Int32>(Int32)'.  
    ```  
  
 次のランタイム ディレクティブは、必要なすべてのメタデータが必ず使用可能であるようにします。  
  
```xml  
<Type Name="App1.Class1" Browse="Required PublicAndInternal">  
   <MethodInstantiation Name="GenMethod" Arguments="System.Int32" Dynamic="Required"/>  
</Type>  
```  
  
 `MethodInstantiation` ディレクティブは、動的に呼び出されるメソッドの異なるインスタンス化ごとに必要です。`Arguments` 要素は、それぞれ異なるインスタンス化引数を反映するために更新されます。  
  
## <a name="arraycreateinstance-and-typemaketypearray-methods"></a>Array.CreateInstance メソッドと Type.MakeTypeArray メソッド  
 次の例は、<xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType> 型で <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> メソッドと `Class1` メソッドを呼び出します。  
  
 [!code-csharp[ProjectN#3](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/array1.cs#3)]  
  
 配列メタデータが存在しない場合、次のエラーが発生します。  
  
```output
This operation cannot be carried out as metadata for the following type was removed for performance reasons:  
  
App1.Class1[]  
  
Unfortunately, no further information is available.  
```  
  
 配列型の `Browse` メタデータは、配列を動的にインスタンス化するために必要です。  次のランタイム ディレクティブにより、`Class1[]` を動的にインスタンス化できます。  
  
```xml  
<Type Name="App1.Class1[]" Browse="Required Public" />  
```  
  
## <a name="see-also"></a>関連項目

- [はじめに](getting-started-with-net-native.md)
- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
