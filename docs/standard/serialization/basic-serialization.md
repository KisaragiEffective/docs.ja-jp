---
title: 基本的なシリアル化
ms.date: 03/30/2017
helpviewer_keywords:
- binary serialization, basic serialization
- serialization, basic serialization
ms.assetid: d899d43c-335a-433e-a589-cd187192984f
dev_langs:
- CSharp
ms.openlocfilehash: a2dde9f795dfe31ff6ef821272a0d5e8d20e8b2f
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159950"
---
# <a name="basic-serialization"></a>基本的なシリアル化

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

クラスをシリアル化できるようにするための最も簡単な方法は、次に示すように <xref:System.SerializableAttribute> でマークすることです。  
  
```csharp  
[Serializable]  
public class MyObject {  
  public int n1 = 0;  
  public int n2 = 0;  
  public String str = null;  
}  
```  
  
このクラスのインスタンスをファイルにシリアル化する方法を、次のコード例に示します。  
  
```csharp  
MyObject obj = new MyObject();  
obj.n1 = 1;  
obj.n2 = 24;  
obj.str = "Some String";  
IFormatter formatter = new BinaryFormatter();  
Stream stream = new FileStream("MyFile.bin", FileMode.Create, FileAccess.Write, FileShare.None);  
formatter.Serialize(stream, obj);  
stream.Close();  
```  
  
この例では、バイナリ フォーマッタを使用してシリアル化します。 必要な作業は、使用するストリームのインスタンスとフォーマッタを作成し、フォーマッタで **Serialize** メソッドを呼び出すことだけです。 ストリームとシリアル化するオブジェクトを、この呼び出しのパラメーターとして指定します。 この例では明示的に示されていませんが、クラスのすべてのメンバー変数は、変数がプライベートとして指定されている場合でもシリアル化されます。 この点で、バイナリ シリアル化は、パブリック フィールドのみをシリアル化する <xref:System.Xml.Serialization.XmlSerializer> クラスとは異なります。 バイナリ シリアル化からメンバー変数を除外する方法については、「[選択的シリアル化](selective-serialization.md)」を参照してください。  
  
オブジェクトを元の状態に復元することも、同じように簡単にできます。 最初に、読み取るストリームと <xref:System.Runtime.Serialization.Formatter> を作成してから、フォーマッタに対してオブジェクトの逆シリアル化を指示します。 この処理を行うコード例を次に示します。  
  
```csharp  
IFormatter formatter = new BinaryFormatter();  
Stream stream = new FileStream("MyFile.bin", FileMode.Open, FileAccess.Read, FileShare.Read);  
MyObject obj = (MyObject) formatter.Deserialize(stream);  
stream.Close();  
  
// Here's the proof.  
Console.WriteLine("n1: {0}", obj.n1);  
Console.WriteLine("n2: {0}", obj.n2);  
Console.WriteLine("str: {0}", obj.str);  
```  
  
前の例で使用している <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> は非常に効率的であり、コンパクトなバイト ストリームを生成します。 このフォーマッタでシリアル化されたすべてのオブジェクトは、同じフォーマッタで逆シリアル化することもできるため、これは .NET Framework で逆シリアル化されるオブジェクトをシリアル化するための最適なツールです。 オブジェクトの逆シリアル化中は、コンストラクターが呼び出されないことに注意してください。 パフォーマンス上の理由から、逆シリアル化に対してこの制約が設けられています。 ただし、この制約はランタイムがオブジェクト作成者と結ぶ通常の協定の一部に違反するため、開発者は、オブジェクトをシリアル化可能としてマークするときに生じる影響を理解する必要があります。  
  
移植性が必要な場合には、代わりに <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> を使用します。 その場合は、コード内の **BinaryFormatter** を **SoapFormatter** に置き換え、前のコードと同様に **Serialize** および **Deserialize** を呼び出します。 前の例では、このフォーマッタの出力は、次のようになります。  
  
```xml  
<SOAP-ENV:Envelope  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"  
  xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"  
  SOAP-ENV:encodingStyle=  
  "http://schemas.microsoft.com/soap/encoding/clr/1.0"  
  "http://schemas.xmlsoap.org/soap/encoding/"  
  xmlns:a1="http://schemas.microsoft.com/clr/assem/ToFile">  
  
  <SOAP-ENV:Body>  
    <a1:MyObject id="ref-1">  
      <n1>1</n1>  
      <n2>24</n2>  
      <str id="ref-3">Some String</str>  
    </a1:MyObject>  
  </SOAP-ENV:Body>  
</SOAP-ENV:Envelope>  
```  
  
[Serializable](xref:System.SerializableAttribute) 属性は継承できないことに注意してください。 `MyObject` から新しいクラスを派生させる場合は、その新しいクラスもこの属性でマークする必要があります。マークしないと、このクラスをシリアル化できません。 たとえば、次に示すクラスのインスタンスをシリアル化しようとすると、<xref:System.Runtime.Serialization.SerializationException> 型がシリアル化可能としてマークされていないことを通知する `MyStuff` が表示されます。  
  
```csharp  
public class MyStuff : MyObject
{  
  public int n3;  
}  
```  
  
 [Serializable](xref:System.SerializableAttribute) 属性を使用すると便利ですが、このような制限事項があります。 シリアル化するクラスをマークするタイミングについては、「[シリアル化のガイドライン](serialization-guidelines.md)」を参照してください。 クラスをコンパイルした後でシリアル化を追加することはできません。  
  
## <a name="see-also"></a>参照

- [バイナリ シリアル化](binary-serialization.md)
- [XML シリアル化および SOAP シリアル化](xml-and-soap-serialization.md)
