---
title: バージョン トレラントなシリアル化
ms.date: 08/08/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- version tolerant serialization
- serialization, custom serialization
- serialization, version tolerant
- serialization, controlling
- versions and serialization
- BinaryFormatter class, samples
- serialization, attributes
ms.assetid: bea0ffe3-2708-4a16-ac7d-e586ed6b8e8d
ms.openlocfilehash: 9886e2f20ef7954b01ea1f46a9eabdb9ea2cc12d
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348431"
---
# <a name="version-tolerant-serialization"></a>バージョン トレラントなシリアル化

.NET Framework のバージョン 1.0 および 1.1 では、アプリケーションのあるバージョンから次のバージョンに移行しても再利用できる、シリアル化可能な型の作成に問題がありました。 フィールドを追加して型を変更すると、次のような問題が発生していました。

- 以前のバージョンのアプリケーションは、古い型の新しいバージョンを逆シリアル化するように要求すると例外をスローする。
- 新しいバージョンのアプリケーションは、データが欠落している以前のバージョンの型を逆シリアル化すると例外をスローする。

バージョン トレラントなシリアル化 (VTS: Version Tolerant Serialization) は、.NET Framework 2.0 で導入された機能セットで、シリアル化可能な型を、長期にわたって簡単に変更できるようにします。 具体的には、VTS 機能が、ジェネリック型を含め、<xref:System.SerializableAttribute> 属性が適用されているクラスに対して有効です。 VTS を使用すると、他のバージョンの型との互換性を失うことなく、これらのクラスに新しいフィールドを追加できます。 動作するサンプル アプリケーションについては、「[Version Tolerant Serialization Technology Sample](version-tolerant-serialization-technology-sample.md)」(バージョン トレラントなシリアル化テクノロジのサンプル) を参照してください。

VTS 機能は、<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> を使用する場合に有効になります。 また、<xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> を使用する場合は、外部データの複数バージョン対応機能を除くすべての機能が有効になります。 シリアル化でこれらのクラスを使用する方法の詳細については、「[バイナリ シリアル化](binary-serialization.md)」を参照してください。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

## <a name="feature-list"></a>機能の一覧

この機能セットの内容は次のとおりです。

- 外部データまたは予期しないデータの複数バージョン対応。 これにより、型の新しいバージョンから以前のバージョンにデータを送信できます。
- 欠落しているオプション データの複数バージョン対応。 これにより、以前のバージョンから新しいバージョンにデータを送信できます。
- シリアル化のコールバック。 これにより、データが欠落している場合に、既定値の高度な設定ができます。

さらに、オプション フィールドが新たに追加されたときに宣言を生成する機能があります。 これは、<xref:System.Runtime.Serialization.OptionalFieldAttribute.VersionAdded%2A> 属性の <xref:System.Runtime.Serialization.OptionalFieldAttribute> プロパティです。

これらの機能については、以降のセクションで詳しく説明します。

### <a name="tolerance-of-extraneous-or-unexpected-data"></a>外部データまたは予期しないデータの複数バージョン対応

これまで、逆シリアル化中に外部データまたは予期しないデータが検出されると、例外がスローされていました。 VTS ではこのような場合、例外がスローされるのではなく、外部データまたは予期しないデータがすべて無視されます。 これにより、新しいバージョンの型 (フィールドが追加されたバージョン) を使用するアプリケーションが、同じ型の以前のバージョンを必要とするアプリケーションに情報を送ることができます。

次の例では、バージョン 2.0 の `CountryField` クラスの `Address` に含まれる追加データは、以前のアプリケーションで新しいバージョンを逆シリアル化する場合、無視されます。

```csharp  
// Version 1 of the Address class.  
[Serializable]  
public class Address  
{  
    public string Street;  
    public string City;  
}  
// Version 2.0 of the Address class.  
[Serializable]  
public class Address  
{  
    public string Street;  
    public string City;  
    // The older application ignores this data.  
    public string CountryField;  
}  
```  
  
```vb  
' Version 1 of the Address class.  
<Serializable> _  
Public Class Address  
    Public Street As String  
    Public City As String  
End Class  
  
' Version 2.0 of the Address class.  
<Serializable> _  
Public Class Address  
    Public Street As String  
    Public City As String  
    ' The older application ignores this data.  
    Public CountryField As String  
End Class  
```  

### <a name="tolerance-of-missing-data"></a>欠落しているデータの複数バージョン対応

<xref:System.Runtime.Serialization.OptionalFieldAttribute> 属性をフィールドに適用すると、そのフィールドをオプションとしてマークできます。 逆シリアル化中に、オプション データの欠落が検出されても、シリアル化エンジンはデータが存在しないことを無視し、例外をスローしません。 そのため、以前のバージョンの型を必要とするアプリケーションから、同じ型の新しいバージョンを必要とするアプリケーションにデータを送信できます。

バージョン 2.0 の `Address` クラスで、`CountryField` フィールドをオプションとしてマークする方法を次の例に示します。 バージョン 2.0 を必要とする新しいアプリケーションに対して、以前のアプリケーションがバージョン 1 を送信した場合、データが存在しないことは無視されます。

```csharp
[Serializable]
public class Address
{
    public string Street;
    public string City;

    [OptionalField]
    public string CountryField;
}
```

```vb
<Serializable> _
Public Class Address
    Public Street As String
    Public City As String

    <OptionalField> _
    Public CountryField As String
End Class
```
  
### <a name="serialization-callbacks"></a>シリアル化のコールバック

シリアル化のコールバックは、次の 4 つの時点でシリアル化プロセスおよび逆シリアル化プロセスにフックする機構です。

|属性|関連するメソッドが呼び出されるタイミング|一般的な用途|
|---------------|------------------------------------------|-----------------|
|<xref:System.Runtime.Serialization.OnDeserializingAttribute>|逆シリアル化の前。\*|オプション フィールドの既定値を初期化します。|
|<xref:System.Runtime.Serialization.OnDeserializedAttribute>|逆シリアル化の後|他のフィールドの内容に基づいて、オプション フィールドの値を修正します。|
|<xref:System.Runtime.Serialization.OnSerializingAttribute>|シリアル化の前|シリアル化の準備をします。 たとえば、オプションのデータ構造を作成します。|
|<xref:System.Runtime.Serialization.OnSerializedAttribute>|シリアル化の後|シリアル化イベントのログを記録します。|

 \* このコールバックは、逆シリアル化コンストラクターの前に呼び出されます (存在する場合)。

#### <a name="using-callbacks"></a>コールバックの使用

コールバックを使用するには、<xref:System.Runtime.Serialization.StreamingContext> パラメーターを受け取るメソッドに、適切な属性を適用します。 各属性でマークできるのは、クラスごとに 1 つのメソッドだけです。 例:

```csharp
[OnDeserializing]
private void SetCountryRegionDefault(StreamingContext sc)
{
    CountryField = "Japan";
}
```

```vb
<OnDeserializing>
Private Sub SetCountryRegionDefault(sc As StreamingContext)
    CountryField = "Japan"
End Sub
```

これらのメソッドの使用目的は、バージョン管理です。 オプション フィールドにデータがない場合、逆シリアル化の実行中に、このフィールドが正しく初期化されないことがあります。 この問題を解決するには、正しい値を割り当てるメソッドを作成してから、このメソッドに **OnDeserializingAttribute** 属性または **OnDeserializedAttribute** 属性のいずれかを適用します。

型のコンテキストで使用されるメソッドを次に示します。 以前のバージョンのアプリケーションが新しいバージョンのアプリケーションに `Address` クラスのインスタンスを送信すると、`CountryField` フィールドのデータが欠落します。 ただし、逆シリアル化の後、フィールドは既定値の "日本" に設定されます。

```csharp
[Serializable]
public class Address
{
    public string Street;
    public string City;
    [OptionalField]
    public string CountryField;

    [OnDeserializing]
    private void SetCountryRegionDefault(StreamingContext sc)
    {
        CountryField = "Japan";
    }
}
```

```vb
<Serializable> _
Public Class Address
    Public Street As String
    Public City As String
    <OptionalField> _
    Public CountryField As String

    <OnDeserializing> _
    Private Sub SetCountryRegionDefault(sc As StreamingContext)
        CountryField = "Japan"
    End Sub
End Class
```

## <a name="the-versionadded-property"></a>VersionAdded プロパティ

**OptionalFieldAttribute** には **VersionAdded** プロパティがあります。 .NET Framework バージョン 2.0 では、このプロパティは使用されません。 ただし、このプロパティを正しく設定して、型が今後のシリアル化エンジンと互換性を保つようにすることが重要です。

このプロパティは、指定されたフィールドに追加された型のバージョンを示します。 次の例に示すように、このプロパティの値は 2 を開始値として、型が変更されるたびに常に 1 ずつインクリメントする必要があります。

```csharp
// Version 1.0
[Serializable]
public class Person
{
    public string FullName;
}

// Version 2.0
[Serializable]
public class Person
{
    public string FullName;

    [OptionalField(VersionAdded = 2)]
    public string NickName;
    [OptionalField(VersionAdded = 2)]
    public DateTime BirthDate;
}

// Version 3.0
[Serializable]
public class Person
{
    public string FullName;

    [OptionalField(VersionAdded=2)]
    public string NickName;
    [OptionalField(VersionAdded=2)]
    public DateTime BirthDate;

    [OptionalField(VersionAdded=3)]
    public int Weight;
}
```

```vb
' Version 1.0
<Serializable> _
Public Class Person
    Public FullName
End Class

' Version 2.0
<Serializable> _
Public Class Person
    Public FullName As String

    <OptionalField(VersionAdded := 2)> _
    Public NickName As String
    <OptionalField(VersionAdded := 2)> _
    Public BirthDate As DateTime
End Class

' Version 3.0
<Serializable> _
Public Class Person
    Public FullName As String

    <OptionalField(VersionAdded := 2)> _
    Public NickName As String
    <OptionalField(VersionAdded := 2)> _
    Public BirthDate As DateTime

    <OptionalField(VersionAdded := 3)> _
    Public Weight As Integer
End Class
```

## <a name="serializationbinder"></a>SerializationBinder

サーバー上とクライアント上では異なるバージョンのクラスが必要なため、ユーザーによっては、シリアル化するクラスと逆シリアル化するクラスを制御することが必要になる場合があります。 <xref:System.Runtime.Serialization.SerializationBinder> は、シリアル化中および逆シリアル化中に使用される実際の型を制御するために使用される抽象クラスです。 このクラスを使用するには、クラスを <xref:System.Runtime.Serialization.SerializationBinder> から派生させ、<xref:System.Runtime.Serialization.SerializationBinder.BindToName%2A> メソッドと <xref:System.Runtime.Serialization.SerializationBinder.BindToType%2A> メソッドをオーバーライドします。 詳細については、「 [SerializationBinder によるシリアル化と逆シリアル](../../framework/wcf/feature-details/controlling-serialization-and-deserialization-with-serializationbinder.md)化の制御」を参照してください。

## <a name="best-practices"></a>ベスト プラクティス

バージョン管理が正しく行われるように、バージョン間で型を変更するときは次の規則に従ってください。

- シリアル化したフィールドを削除しない。
- 以前のバージョンでフィールドに <xref:System.NonSerializedAttribute> 属性が適用されていない場合、新しいバージョンでもこの属性を適用しない。
- シリアル化したフィールドの名前または型を変更しない。
- 新しいシリアル化フィールドを追加する場合は、**OptionalFieldAttribute** 属性を適用する。
- 以前のバージョンでシリアル化できなかったフィールドから **NonSerializedAttribute** 属性を削除する場合は、**OptionalFieldAttribute** 属性を適用する。
- すべてのオプション フィールドに対して、既定値として 0 または **null** が許容される場合以外は、シリアル化コールバックを使用して意味のある既定値を設定する。

型が今後のシリアル化エンジンと互換性を保つようにするには、次のガイドラインに従ってください。

- **OptionalFieldAttribute** 属性には常に **VersionAdded** を設定する。
- バージョンの分岐は避ける。

## <a name="see-also"></a>関連項目

- <xref:System.SerializableAttribute>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
- <xref:System.Runtime.Serialization.OptionalFieldAttribute.VersionAdded%2A>
- <xref:System.Runtime.Serialization.OptionalFieldAttribute>
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>
- <xref:System.Runtime.Serialization.OnSerializingAttribute>
- <xref:System.Runtime.Serialization.OnSerializedAttribute>
- <xref:System.Runtime.Serialization.StreamingContext>
- <xref:System.NonSerializedAttribute>
- [バイナリ シリアル化](binary-serialization.md)
