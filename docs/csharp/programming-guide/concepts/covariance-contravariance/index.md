---
title: 共変性と反変性 (C#)
ms.date: 07/20/2015
ms.assetid: 066d9a3c-aab7-4ea6-826d-0b1a85399c74
ms.openlocfilehash: 80b4d703bb88d0bf1f7f48236c21b7698017e7f8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79169871"
---
# <a name="covariance-and-contravariance-c"></a>共変性と反変性 (C#)
C# では、共変性と反変性により、配列型、デリゲート型、およびジェネリック型引数の暗黙の参照変換が可能になります。 共変性は代入互換性を維持し、反変性はこれを反転させます。  
  
 次のコードでは、代入互換性、共変性、および反変性の違いについて説明します。  
  
```csharp  
// Assignment compatibility.
string str = "test";  
// An object of a more derived type is assigned to an object of a less derived type.
object obj = str;  
  
// Covariance.
IEnumerable<string> strings = new List<string>();  
// An object that is instantiated with a more derived type argument
// is assigned to an object instantiated with a less derived type argument.
// Assignment compatibility is preserved.
IEnumerable<object> objects = strings;  
  
// Contravariance.
// Assume that the following method is in the class:
// static void SetObject(object o) { }
Action<object> actObject = SetObject;  
// An object that is instantiated with a less derived type argument
// is assigned to an object instantiated with a more derived type argument.
// Assignment compatibility is reversed.
Action<string> actString = actObject;  
```  
  
 配列の共変性により、強い派生型の配列から弱い派生型の配列への暗黙の型変換が可能になります。 ただし、次のコード例に示すように、この操作はタイプ セーフではありません。  
  
```csharp  
object[] array = new String[10];  
// The following statement produces a run-time exception.  
// array[0] = 10;  
```  
  
 メソッド グループの共変性と反変性のサポートにより、メソッド シグネチャをデリゲート型と一致させることができます。 これにより、一致するシグネチャを持つメソッドだけでなく、デリゲート型で指定された型よりも強い派生型 (共変性) を返すメソッドや、弱い派生型 (反変性) のパラメーターを受け取るメソッドを、デリゲートに割り当てることができます。 詳細については、「[デリゲートの変性 (C#)](./variance-in-delegates.md)」および「[デリゲートの変性の使用 (C#)](./using-variance-in-delegates.md)」を参照してください。  
  
 次のコード例は、メソッド グループでの共変性と反変性のサポートを示しています。  
  
```csharp  
static object GetObject() { return null; }  
static void SetObject(object obj) { }  
  
static string GetString() { return ""; }  
static void SetString(string str) { }  
  
static void Test()  
{  
    // Covariance. A delegate specifies a return type as object,  
    // but you can assign a method that returns a string.  
    Func<object> del = GetString;  
  
    // Contravariance. A delegate specifies a parameter type as string,  
    // but you can assign a method that takes an object.  
    Action<string> del2 = SetObject;  
}  
```  
  
 .NET Framework 4 以降では、C# でジェネリック インターフェイスと汎用デリゲートでの共変性と反変性がサポートされ、ジェネリック型パラメーターの暗黙の型変換が可能になっています。 詳細については、「[ジェネリック インターフェイスの変性 (C#)](./variance-in-generic-interfaces.md)」および「[デリゲートの変性 (C#)](./variance-in-delegates.md)」を参照してください。  
  
 次のコード例は、ジェネリック インターフェイスの暗黙の参照変換を示しています。  
  
```csharp  
IEnumerable<String> strings = new List<String>();  
IEnumerable<Object> objects = strings;  
```  
  
 ジェネリック インターフェイスや汎用デリゲートは、そのジェネリック パラメーターが共変または反変として宣言されている場合、*バリアント*と呼ばれます。 C# では、独自のバリアント インターフェイスおよびデリゲートを作成できます。 詳細については、「[バリアント ジェネリック インターフェイスの作成 (C#)](./creating-variant-generic-interfaces.md)」および「[デリゲートの分散 (C#)](./variance-in-delegates.md)」を参照してください。  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[ジェネリック インターフェイスの変性 (C#)](./variance-in-generic-interfaces.md)|ジェネリック インターフェイスでの共変性と反変性について説明し、.NET Framework でのバリアント ジェネリック インターフェイスの一覧を示します。|  
|[バリアント ジェネリック インターフェイスの作成 (C#)](./creating-variant-generic-interfaces.md)|カスタムのバリアント インターフェイスを作成する方法を示します。|  
|[ジェネリック コレクションに対するインターフェイスでの変性の使用 (C#)](./using-variance-in-interfaces-for-generic-collections.md)|<xref:System.Collections.Generic.IEnumerable%601> および <xref:System.IComparable%601> インターフェイスでの共変性と反変性のサポートがコードの再利用にどのように役立つかを示します。|  
|[デリゲートの変性 (C#)](./variance-in-delegates.md)|汎用および非汎用デリゲートでの共変性と反変性について説明し、.NET Framework でのバリアント汎用デリゲートの一覧を示します。|  
|[デリゲートの変性の使用 (C#)](./using-variance-in-delegates.md)|非汎用デリゲートでの共変性と反変性のサポートを使用して、メソッド シグネチャをデリゲート型に一致させる方法について説明します。|  
|[Func および Action 汎用デリゲートでの分散の使用 (C#)](./using-variance-for-func-and-action-generic-delegates.md)|`Func` および `Action` デリゲートでの共変性と反変性のサポートがコードの再利用にどのように役立つかを示します。|
