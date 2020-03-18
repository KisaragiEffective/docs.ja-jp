---
title: デリゲートの変性 (C#)
ms.date: 07/20/2015
ms.assetid: 19de89d2-8224-4406-8964-2965b732b890
ms.openlocfilehash: fd1b4824dc3d8f12347e01b804a6e39fe2e086c8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79169715"
---
# <a name="variance-in-delegates-c"></a>デリゲートの変性 (C#)
.NET Framework 3.5 では、C# のすべてのデリゲートで、メソッド シグネチャとデリゲート型を一致させるために変性 (共変性と反変性) のサポートが導入されました。 つまり、シグネチャが一致するメソッドだけでなく、デリゲート型で指定された型よりも強い派生型を返す (共変性) メソッドや、弱い派生型のパラメーターを受け取る (反変性) メソッドを、デリゲートに割り当てることができます。 これには、汎用デリゲートと非汎用デリゲートの両方が含まれます。  
  
 たとえば、次のコードについて考えます。このコードには、2 つのクラスと、汎用と非汎用の 2 つのデリゲートが含まれています。  
  
```csharp  
public class First { }  
public class Second : First { }  
public delegate First SampleDelegate(Second a);  
public delegate R SampleGenericDelegate<A, R>(A a);  
```  
  
 `SampleDelegate` 型または `SampleGenericDelegate<A, R>` 型のデリゲートを作成する場合、そのデリゲートには、次のいずれかのメソッドを割り当てることができます。  
  
```csharp  
// Matching signature.  
public static First ASecondRFirst(Second second)  
{ return new First(); }  
  
// The return type is more derived.  
public static Second ASecondRSecond(Second second)  
{ return new Second(); }  
  
// The argument type is less derived.  
public static First AFirstRFirst(First first)  
{ return new First(); }  
  
// The return type is more derived
// and the argument type is less derived.  
public static Second AFirstRSecond(First first)  
{ return new Second(); }  
```  
  
 次のコード例は、メソッド シグネチャとデリゲート型の間の暗黙的な変換を示しています。  
  
```csharp  
// Assigning a method with a matching signature
// to a non-generic delegate. No conversion is necessary.  
SampleDelegate dNonGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a non-generic delegate.  
// The implicit conversion is used.  
SampleDelegate dNonGenericConversion = AFirstRSecond;  
  
// Assigning a method with a matching signature to a generic delegate.  
// No conversion is necessary.  
SampleGenericDelegate<Second, First> dGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type
// and less derived argument type to a generic delegate.  
// The implicit conversion is used.  
SampleGenericDelegate<Second, First> dGenericConversion = AFirstRSecond;  
```  
  
 その他の例については、「[デリゲートの変性の使用 (C#)](./using-variance-in-delegates.md)」および「[Func および Action 汎用デリゲートでの変性の使用 (C#)](./using-variance-for-func-and-action-generic-delegates.md)」を参照してください。  
  
## <a name="variance-in-generic-type-parameters"></a>ジェネリック型パラメーターの変性  
 .NET Framework 4 以降では、デリゲート間の暗黙的な変換を有効にできるため、ジェネリック型パラメーターによって汎用デリゲートにさまざまな型が指定されていても、型が変性の要件を満たすように相互に継承されていれば、それらの汎用デリゲートは相互に割り当てることができます。  
  
 暗黙的な変換を有効にするには、`in` キーワードまたは `out` キーワードを使用して、デリゲートでジェネリック パラメーターを共変または反変として明示的に宣言する必要があります。  
  
 次のコード例は、共変のジェネリック型パラメーターが指定されたデリゲートを作成する方法を示しています。  
  
```csharp  
// Type T is declared covariant by using the out keyword.  
public delegate T SampleGenericDelegate <out T>();  
  
public static void Test()  
{  
    SampleGenericDelegate <String> dString = () => " ";  
  
    // You can assign delegates to each other,  
    // because the type T is declared covariant.  
    SampleGenericDelegate <Object> dObject = dString;
}  
```  
  
 変性サポートのみを使用してメソッド シグネチャをデリゲート型に一致させ、`in` キーワードと `out` キーワードを使用しない場合、同等のラムダ式かメソッドを使用すれば、デリゲートをインスタンス化できることがありますが、デリゲートを別のデリゲートに割り当てることはできません。  
  
 次のコード例では、`SampleGenericDelegate<String>` が `SampleGenericDelegate<Object>` を継承していますが、`String` を `Object` に明示的に変換することはできません。 この問題を修正するには、ジェネリック パラメーター `T` を `out` キーワードでマークします。  
  
```csharp  
public delegate T SampleGenericDelegate<T>();  
  
public static void Test()  
{  
    SampleGenericDelegate<String> dString = () => " ";  
  
    // You can assign the dObject delegate  
    // to the same lambda expression as dString delegate  
    // because of the variance support for
    // matching method signatures with delegate types.  
    SampleGenericDelegate<Object> dObject = () => " ";  
  
    // The following statement generates a compiler error  
    // because the generic type T is not marked as covariant.  
    // SampleGenericDelegate <Object> dObject = dString;  
  
}  
```  
  
### <a name="generic-delegates-that-have-variant-type-parameters-in-the-net-framework"></a>.NET Framework のバリアント型パラメーターが含まれる汎用デリゲート  
 .NET Framework 4 では、既存の複数の汎用デリゲートで、ジェネリック型パラメーターに対して変性サポートが導入されました。  
  
- `Action` 名前空間の <xref:System> デリゲート。<xref:System.Action%601>、<xref:System.Action%602> など  
  
- `Func` 名前空間の <xref:System> デリゲート。<xref:System.Func%601>、<xref:System.Func%602> など  
  
- <xref:System.Predicate%601> デリゲート  
  
- <xref:System.Comparison%601> デリゲート  
  
- <xref:System.Converter%602> デリゲート  
  
 使用例を含む詳細については、「[Func および Action 汎用デリゲートでの変性の使用 (C#)](./using-variance-for-func-and-action-generic-delegates.md)」を参照してください。  
  
### <a name="declaring-variant-type-parameters-in-generic-delegates"></a>汎用デリゲートのバリアント型パラメーターの宣言  
 汎用デリゲートに共変または反変のジェネリック型パラメーターがある場合、そのデリゲートは "*バリアント汎用デリゲート*" と呼ばれます。  
  
 汎用デリゲートのジェネリック型パラメーターを共変として宣言するには、`out` キーワードを使用します。 共変の型は、メソッドの戻り値の型としてのみ使用できます。メソッド引数の型として使用することはできません。 共変の汎用デリゲートを宣言する方法を次のコード例に示します。  
  
```csharp  
public delegate R DCovariant<out R>();  
```  
  
 汎用デリゲートのジェネリック型パラメーターを反変として宣言するには、`in` キーワードを使用します。 反変の型は、メソッド引数の型としてのみ使用できます。メソッドの戻り値の型として使用することはできません。 反変の汎用デリゲートを宣言する方法を次のコード例に示します。  
  
```csharp  
public delegate void DContravariant<in A>(A a);  
```  
  
> [!IMPORTANT]
> C# の `ref`、`in`、`out` パラメーターを、バリアントとしてマークすることはできません。  
  
 同じデリゲートで、型パラメーターが異なる場合は、変性と共変性の両方をサポートすることもできます。 次の例を参照してください。  
  
```csharp  
public delegate R DVariant<in A, out R>(A a);  
```  
  
### <a name="instantiating-and-invoking-variant-generic-delegates"></a>バリアント汎用デリゲートのインスタンス化と呼び出し  
 バリアント デリゲートのインスタンス化および呼び出しは、インバリアント デリゲートのインスタンス化および呼び出しと同様に行うことができます。 次の例では、ラムダ式によってデリゲートをインスタンス化します。  
  
```csharp  
DVariant<String, String> dvariant = (String str) => str + " ";  
dvariant("test");  
```  
  
### <a name="combining-variant-generic-delegates"></a>バリアント汎用デリゲートの結合  
 バリアント デリゲートの結合はお勧めしません。 <xref:System.Delegate.Combine%2A> メソッドはバリアント デリゲートの変換をサポートしていないため、デリゲートが厳密に同じ型である必要があります。 そのため、次のコード例に示すように、<xref:System.Delegate.Combine%2A> メソッドまたは `+` 演算子を使用してデリゲートを結合すると、実行時例外が発生する可能性があります。  
  
```csharp  
Action<object> actObj = x => Console.WriteLine("object: {0}", x);  
Action<string> actStr = x => Console.WriteLine("string: {0}", x);  
// All of the following statements throw exceptions at run time.  
// Action<string> actCombine = actStr + actObj;  
// actStr += actObj;  
// Delegate.Combine(actStr, actObj);  
```  
  
## <a name="variance-in-generic-type-parameters-for-value-and-reference-types"></a>値型と参照型でのジェネリック型パラメーターの変性  
 ジェネリック型パラメーターの変性がサポートされるのは参照型だけです。 たとえば、整数は値型であるため、`DVariant<int>` を `DVariant<Object>` または `DVariant<long>` に暗黙的に変換することはできません。  
  
 次の例は、値型ではジェネリック型パラメーターの変性がサポートされないことを示しています。  
  
```csharp  
// The type T is covariant.  
public delegate T DVariant<out T>();  
  
// The type T is invariant.  
public delegate T DInvariant<T>();  
  
public static void Test()  
{  
    int i = 0;  
    DInvariant<int> dInt = () => i;  
    DVariant<int> dVariantInt = () => i;  
  
    // All of the following statements generate a compiler error  
    // because type variance in generic parameters is not supported  
    // for value types, even if generic type parameters are declared variant.  
    // DInvariant<Object> dObject = dInt;  
    // DInvariant<long> dLong = dInt;  
    // DVariant<Object> dVariantObject = dVariantInt;  
    // DVariant<long> dVariantLong = dVariantInt;
}  
```  
  
## <a name="see-also"></a>参照

- [ジェネリック](../../../../standard/generics/index.md)
- [Func および Action 汎用デリゲートでの分散の使用 (C#)](./using-variance-for-func-and-action-generic-delegates.md)
- [デリゲートを結合する方法 (マルチキャスト デリゲート)](../../delegates/how-to-combine-delegates-multicast-delegates.md)
