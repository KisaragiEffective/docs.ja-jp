---
title: '方法: DataContractJsonSerializer を使用する'
ms.date: 03/25/2019
ms.assetid: 88abc1fb-8196-4ee3-a23b-c6934144d1dd
ms.openlocfilehash: 3cf8cc52587a64e7273ab9e0de0b1751d00827cf
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901211"
---
# <a name="how-to-use-datacontractjsonserializer"></a>DataContractJsonSerializer の使用方法

> [!NOTE]
> この記事では、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>について説明します。 JSON のシリアル化と逆シリアル化を含むほとんどのシナリオでは、Api を使用することをお勧めし[ます。](../../../standard/serialization/system-text-json-overview.md)

JSON (JavaScript Object Notation) は、クライアント ブラウザーと AJAX 対応の Web サービスとの間で、少量のデータを高速に交換できる効率的なデータ エンコード形式です。

この記事では、.NET 型オブジェクトを JSON エンコードされたデータにシリアル化し、JSON 形式のデータを .NET 型のインスタンスに逆シリアル化する方法について説明します。 この例では、データコントラクトを使用して、ユーザー定義 `Person` 型のシリアル化と逆シリアル化を示し、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>を使用します。

通常、AJAX 対応のエンドポイントで公開されるサービス操作でデータコントラクト型を使用する場合、JSON のシリアル化と逆シリアル化は、Windows Communication Foundation (WCF) によって自動的に処理されます。 ただし、場合によっては、JSON データを直接操作する必要があります。

この記事は、 [DataContractJsonSerializer サンプル](../samples/json-serialization.md)を基にしています。

## <a name="to-define-the-data-contract-for-a-person-type"></a>個人の種類のデータコントラクトを定義するには

1. クラスに `Person` をアタッチし、シリアル化するメンバーに <xref:System.Runtime.Serialization.DataContractAttribute> 属性をアタッチすることで、<xref:System.Runtime.Serialization.DataMemberAttribute> のデータ コントラクトを定義します。 データコントラクトの詳細については、「[サービスコントラクトの設計](../designing-service-contracts.md)」を参照してください。

    ```csharp
    [DataContract]
    internal class Person
    {
        [DataMember]
        internal string name;

        [DataMember]
        internal int age;
    }
    ```

## <a name="to-serialize-an-instance-of-type-person-to-json"></a>Person 型のインスタンスを JSON にシリアル化するには

> [!NOTE]
> サーバー上の送信応答のシリアル化中または他の何らかの理由でエラーが発生した場合、エラーとしてクライアントに返されないことがあります。

1. `Person` 型のインスタンスを作成します。

    ```csharp
    var p = new Person();
    p.name = "John";
    p.age = 42;
    ```

2. <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>を使用して、`Person` オブジェクトをメモリストリームにシリアル化します。

    ```csharp
    var stream1 = new MemoryStream();
    var ser = new DataContractJsonSerializer(typeof(Person));
    ```

3. JSON データをストリームに書き込むには、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject%2A> メソッドを使用します。

    ```csharp
    ser.WriteObject(stream1, p);
    ```

4. JSON の出力を表示します。

    ```csharp
    stream1.Position = 0;
    var sr = new StreamReader(stream1);
    Console.Write("JSON form of Person object: ");
    Console.WriteLine(sr.ReadToEnd());
    ```

## <a name="to-deserialize-an-instance-of-type-person-from-json"></a>JSON から Person 型のインスタンスに逆シリアル化するには

1. `Person` の <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.ReadObject%2A> メソッドを使用して、JSON エンコードされたデータを <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> の新しいインスタンスに逆シリアル化します。

    ```csharp
    stream1.Position = 0;
    var p2 = (Person)ser.ReadObject(stream1);
    ```

2. 結果を表示します。

    ```csharp
    Console.WriteLine($"Deserialized back, got name={p2.name}, age={p2.age}");
    ```

## <a name="example"></a>使用例

```csharp
// Create a User object and serialize it to a JSON stream.
public static string WriteFromObject()
{
    // Create User object.
    var user = new User("Bob", 42);

    // Create a stream to serialize the object to.
    var ms = new MemoryStream();

    // Serializer the User object to the stream.
    var ser = new DataContractJsonSerializer(typeof(User));
    ser.WriteObject(ms, user);
    byte[] json = ms.ToArray();
    ms.Close();
    return Encoding.UTF8.GetString(json, 0, json.Length);
}

// Deserialize a JSON stream to a User object.
public static User ReadToObject(string json)
{
    var deserializedUser = new User();
    var ms = new MemoryStream(Encoding.UTF8.GetBytes(json));
    var ser = new DataContractJsonSerializer(deserializedUser.GetType());
    deserializedUser = ser.ReadObject(ms) as User;
    ms.Close();
    return deserializedUser;
}
```

> [!NOTE]
> JSON シリアライザーは、次のサンプル コードに示すように、データ コントラクターの複数のメンバーが同じ名前である場合、シリアル化例外をスローします。

```csharp
[DataContract]
public class TestDuplicateDataBase
{
    [DataMember]
    public int field1 = 123;
}

[DataContract]
public class TestDuplicateDataDerived : TestDuplicateDataBase
{
    [DataMember]
    public new int field1 = 999;
}
```

## <a name="see-also"></a>関連項目

- [.NET での JSON のシリアル化](../../../standard/serialization/system-text-json-overview.md)
