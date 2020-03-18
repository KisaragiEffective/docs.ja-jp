---
title: XML データ型の変換
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: a2aa99ba-8239-4818-9281-f1d72ee40bde
ms.openlocfilehash: b0cdab8861ca50b40ce2b422fcc1acf16e2f2273
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75711091"
---
# <a name="conversion-of-xml-data-types"></a>XML データ型の変換
**XmlConvert** クラスのメソッドのほとんどは、文字列と厳密に型指定された形式との間のデータ変換に使われます。 これらのメソッドはロケールに依存しません。 つまり、変換の実行時にはロケールの設定は考慮されません。  
  
## <a name="reading-string-as-types"></a>文字列を型として読み込む  
 文字列を読み込んで **DateTime** 型に変換するサンプルを次に示します。  
  
 入力として次の XML を使用します。  
  
 **入力**  
  
```xml  
<Element>2001-02-27T11:13:23</Element>  
```  
  
 次のコードは、文字列を **DateTime** 形式に変換します。  
  
```vb  
reader.ReadStartElement()  
Dim vDateTime As DateTime = XmlConvert.ToDateTime(reader.ReadString())  
Console.WriteLine(vDateTime)  
```  
  
```csharp  
reader.ReadStartElement();  
DateTime vDateTime = XmlConvert.ToDateTime(reader.ReadString());  
Console.WriteLine(vDateTime);  
```  
  
## <a name="writing-strings-as-types"></a>文字列を型として書き込む  
 **Int32** を読み込んで文字列に変換するサンプルを次に示します。  
  
 入力として次の XML を使用します。  
  
 **入力**  
  
```xml  
<TestInt32>-2147483648</TestInt32>  
```  
  
 次のコードは、**Int32** を **String** に変換します。  
  
```vb  
Dim vInt32 As Int32 = -2147483648  
writer.WriteElementString("TestInt32", XmlConvert.ToString(vInt32))  
```  
  
```csharp  
Int32 vInt32=-2147483648;  
writer.WriteElementString("TestInt32",XmlConvert.ToString(vInt32));  
```  
  
## <a name="see-also"></a>関連項目

- [文字列の .NET Framework データ型への変換](../../../../docs/standard/data/xml/converting-strings-to-dotnet-data-types.md)
- [.NET Framework 型の文字列への変換](../../../../docs/standard/data/xml/converting-dotnet-types-to-strings.md)
