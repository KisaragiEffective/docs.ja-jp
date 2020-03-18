---
title: カスタムのシリアル化
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- binary serialization, custom serialization
- custom serialization
- binary serialization, controlling
- OptionalFieldAttribute class, custom serialization
- ISerializable interface, custom serialization
- OnDeserializingAttribute class, custom serialization
- OnSerializedAttribute class, custom serialization
- serialization, custom serialization
- serialization, controlling
- OnDeserializedAttribute class, custom serialization
- OnSerializingAttribute class, custom serialization
ms.assetid: 12ed422d-5280-49b8-9b71-a2ed129c0384
ms.openlocfilehash: 60fdc0317975d94433401e3214953b77d0970f60
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75741062"
---
# <a name="custom-serialization"></a>カスタムのシリアル化
カスタムのシリアル化は、型のシリアル化と逆シリアル化を制御するプロセスです。 シリアル化を制御することで、シリアル化の互換性を保証できます。つまり、型のコア機能を損なうことなく、1 つの型の複数のバージョン間でシリアル化および逆シリアル化を行うことができます。 たとえば、最初のバージョンの型では、フィールドが 2 つだけあるとします。 新しいバージョンでは、これにいくつかのフィールドが追加されています。 この場合、2 番目のバージョンのアプリケーションでは、両方の型をシリアル化および逆シリアル化できる必要があります。 以下のセクションでは、シリアル化の制御方法について説明します。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
> [!IMPORTANT]
> .NET Framework 4.0 よりも前のバージョンでは、部分的に信頼されたアセンブリでのカスタム ユーザー データのシリアル化は GetObjectData を使用して実行していました。 バージョン 4.0 以降では、そのメソッドは、部分的に信頼されたアセンブリで実行できないようにする <xref:System.Security.SecurityCriticalAttribute> 属性でマークされています。 この状況に対処するには、<xref:System.Runtime.Serialization.ISafeSerializationData> インターフェイスを実装します。  
  
## <a name="running-custom-methods-during-and-after-serialization"></a>シリアル化時およびシリアル化後のカスタム メソッドの実行  
 ベスト プラクティスかつ最も簡単な方法 (.Net Framework Version 2.0 で導入されました) は、シリアル化時およびシリアル化後にデータを修正するための各メソッドに、以下の属性を適用することです。  
  
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>  
  
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>  
  
- <xref:System.Runtime.Serialization.OnSerializedAttribute>  
  
- <xref:System.Runtime.Serialization.OnSerializingAttribute>  
  
 これらの属性を適用すると、シリアル化プロセスと逆シリアル化プロセスの 4 つのフェーズのうち、いずれか、またはすべてに型を参加させることができます。 これらの属性は、各フェーズで呼び出す必要がある型のメソッドを指定します。 これらのメソッドはシリアル化ストリームにはアクセスしませんが、これらを使用すると、シリアル化の前後、または逆シリアル化の前後にオブジェクトを変更できます。 これらの属性は型の継承階層の全レベルで適用でき、各メソッドは、基本クラスから最派生クラスまで、階層内で呼び出されます。 このしくみでは、最派生実装でシリアル化および逆シリアル化が行われるため、<xref:System.Runtime.Serialization.ISerializable> インターフェイスの実装の複雑性やその実装によって発生する問題が回避されます。 また、フォーマッタは、フィールド値の設定およびシリアル化ストリームからの取得を無視できます。 シリアル化および逆シリアル化の制御の詳細と例については、上記の各リンクをクリックしてください。  
  
 また、既存のシリアル化可能な型に新しいフィールドを追加する場合は、このフィールドに <xref:System.Runtime.Serialization.OptionalFieldAttribute> 属性を適用します。 新しいフィールドが含まれていないストリームを処理する際に、<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> および <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> はこのフィールドの不足を無視します。  
  
## <a name="implementing-the-iserializable-interface"></a>ISerializable インターフェイスの実装  
 シリアル化を制御するもう 1 つの方法は、オブジェクトに <xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装することです。 ただし、シリアル化の制御では、前のセクションで説明した方法がこの方法よりも優先されることに注意してください。  
  
 また、[Serializable](xref:System.SerializableAttribute) 属性を使用してマークされ、クラス レベルまたはクラスのコンストラクターで宣言セキュリティまたは強制セキュリティが設定されたクラスでは、既定のシリアル化を使用しないでください。 代わりに、このようなクラスでは、常に <xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装する必要があります。  
  
 <xref:System.Runtime.Serialization.ISerializable> を実装すると、`GetObjectData` メソッドと、このオブジェクトが逆シリアル化されるときに使用される専用のコンストラクターも実装されます。 前のセクションで使用した <xref:System.Runtime.Serialization.ISerializable> クラスに `MyObject` を実装する方法を次のコード例に示します。  
  
```csharp
[Serializable]
public class MyObject : ISerializable
{
    public int n1;
    public int n2;
    public String str;

    public MyObject()
    {
    }

    protected MyObject(SerializationInfo info, StreamingContext context)
    {
      n1 = info.GetInt32("i");
      n2 = info.GetInt32("j");
      str = info.GetString("k");
    }

    [SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter = true)]
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        info.AddValue("i", n1);
        info.AddValue("j", n2);
        info.AddValue("k", str);
    }
}
```

```vb
<Serializable()>  _
Public Class MyObject
    Implements ISerializable
    Public n1 As Integer
    Public n2 As Integer
    Public str As String

    Public Sub New()
    End Sub

    Protected Sub New(ByVal info As SerializationInfo, ByVal context As StreamingContext)
        n1 = info.GetInt32("i")
        n2 = info.GetInt32("j")
        str = info.GetString("k")
    End Sub 'New

    <SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)> _
    Public Overridable Sub GetObjectData(ByVal info As SerializationInfo, ByVal context As StreamingContext)
        info.AddValue("i", n1)
        info.AddValue("j", n2)
        info.AddValue("k", str)
    End Sub
End Class
```  
  
 シリアル化時に **GetObjectData** を呼び出す場合は、メソッド呼び出しで提供される <xref:System.Runtime.Serialization.SerializationInfo> を設定する必要があります。 シリアル化の対象とする変数は、名前と値のペアとして追加します。 名前には、任意のテキストを使用できます。 逆シリアル化時にオブジェクトを復元するのに十分なデータがシリアル化される場合は、<xref:System.Runtime.Serialization.SerializationInfo> に追加するメンバー変数を自由に決定できます。 基本オブジェクトが <xref:System.Runtime.Serialization.ISerializable> を実装している場合、派生クラスはその基本オブジェクトの **GetObjectData** メソッドを呼び出す必要があります。  
  
 シリアル化によって、他の方法ではアクセスできないオブジェクト インスタンス データを他のコードから参照または変更できるようになります。 したがって、シリアル化を実行するコードには、<xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> フラグが指定された [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) が必要です。 既定のポリシーでは、インターネットからダウンロードしたコードまたはイントラネット コードにはこのアクセス許可は与えられず、ローカル コンピューター上のコードにだけ付与されます。 **GetObjectData** メソッドは、<xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> フラグが指定された [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) を要求するか、特にプライベート データの保護に役立つ他のアクセス許可を要求することによって、明示的に保護する必要があります。  
  
 プライベート フィールドに機密情報が格納されている場合は、**GetObjectData** で適切なアクセス許可を要求してデータを保護してください。 **SerializationFormatter** フラグが指定された [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) を付与されているコードは、プライベート フィールドに格納されているデータを参照および変更できることに注意してください。 この [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) が付与されている悪意のある呼び出し元によって、隠しディレクトリの位置や付与されたアクセス許可などのデータが参照され、コンピューター上のセキュリティの脆弱性が悪用される可能性があります。 指定できるセキュリティ アクセス許可フラグの完全な一覧については、「[SecurityPermissionFlag 列挙体](xref:System.Security.Permissions.SecurityPermissionFlag)」を参照してください。  
  
 <xref:System.Runtime.Serialization.ISerializable> をクラスに追加する場合は、**GetObjectData** と専用のコンストラクターの両方を実装する必要があることに注意してください。 **GetObjectData** がない場合、コンパイラから警告が出力されます。 ただし、コンストラクターの実装を強制することはできないため、コンストラクターが指定されていなくても警告は表示されず、コンストラクターがないクラスの逆シリアル化が試行された時点で例外がスローされます。  
  
 セキュリティやバージョン管理に関して発生する可能性がある問題を回避するために、現在のデザインは <xref:System.Runtime.Serialization.ISerializationSurrogate.SetObjectData%2A> メソッドよりも優先されています。 たとえば、`SetObjectData` メソッドは、インターフェイスの一部として定義された場合にはパブリックである必要があるため、ユーザーは **SetObjectData** メソッドが複数回呼び出されることを防ぐためにコードを記述する必要があります。 そうしないと、操作を実行しているオブジェクトで **SetObjectData** メソッドを呼び出す悪意のあるアプリケーションによって、さまざまな問題が発生する可能性があります。  
  
 逆シリアル化時に、<xref:System.Runtime.Serialization.SerializationInfo> は、これをクラスに渡すために提供されているコンストラクターを使用してクラスに渡されます。 オブジェクトが逆シリアル化されるときには、コンストラクターに対して設定された参照可能範囲の制限は無視されるため、パブリック、プロテクト、内部、またはプライベートとしてクラスをマークできます。 ただし、クラスがシールされている場合を除いて、コンストラクターをプロテクトにすることがベスト プラクティスです。クラスがシールされている場合は、コンストラクターをプライベートとマークする必要があります。 コンストラクターは入力の検証も実行する必要があります。 悪意のあるコードによる不適切な使用を回避するために、コンストラクターは、他のコンストラクターを使用してクラスのインスタンスを取得する場合と同様のセキュリティ チェックおよびアクセス許可を適用する必要があります。 この推奨事項に従わないと、パブリック コンストラクターを使用する標準インスタンスの構築時に適用されるはずのセキュリティが適用されず、悪意のあるコードが、オブジェクトを事前にシリアル化し、<xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> フラグが指定された [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) で制御を取得して、クライアント コンピューター上でオブジェクトを逆シリアル化することが可能になります。  
  
 オブジェクトの状態を復元するには、シリアル化時に使用した名前を使って、<xref:System.Runtime.Serialization.SerializationInfo> から変数の値を取得します。 基本クラスに <xref:System.Runtime.Serialization.ISerializable> が実装されている場合は、基本オブジェクトがその変数を復元できるようにするために、基本コンストラクターを呼び出す必要があります。  
  
 <xref:System.Runtime.Serialization.ISerializable> を実装しているクラスから新しいクラスを派生させるときに、派生クラスにシリアル化する必要がある変数がある場合は、派生クラスでコンストラクターと **GetObjectData** メソッドの両方を実装する必要があります。 上記の `MyObject` クラスを使用してこれを行う方法を次のコード例に示します。  
  
```csharp
[Serializable]
public class ObjectTwo : MyObject
{
    public int num;

    public ObjectTwo()
      : base()
    {
    }

    protected ObjectTwo(SerializationInfo si, StreamingContext context)
      : base(si, context)
    {
        num = si.GetInt32("num");
    }

    [SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter = true)]
    public override void GetObjectData(SerializationInfo si, StreamingContext context)
    {
        base.GetObjectData(si,context);
        si.AddValue("num", num);
    }
}
```

```vb
<Serializable()>  _
Public Class ObjectTwo
    Inherits MyObject
    Public num As Integer

    Public Sub New()

    End Sub

    Protected Sub New(ByVal si As SerializationInfo, _
    ByVal context As StreamingContext)
        MyBase.New(si, context)
        num = si.GetInt32("num")
    End Sub

    <SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)> _
    Public Overrides Sub GetObjectData(ByVal si As SerializationInfo, ByVal context As StreamingContext)
        MyBase.GetObjectData(si, context)
        si.AddValue("num", num)
    End Sub
End Class
```  
  
 必ず、逆シリアル化コンストラクターで基本クラスを呼び出すようにしてください。 そうしないと、基本クラスのコンストラクターが呼び出されず、逆シリアル化後にオブジェクトが完全には構築されません。  
  
 オブジェクトは内側から外側に向かって再構築されるため、逆シリアル化時にメソッドを呼び出すと、望ましくない副作用を引き起こす可能性があります。これは、呼び出されるメソッドが、呼び出しの時点では逆シリアル化されていないオブジェクト参照を参照することがあるためです。 逆シリアル化対象のクラスで <xref:System.Runtime.Serialization.IDeserializationCallback> を実装する場合、オブジェクト グラフ全体が逆シリアル化された時点で <xref:System.Runtime.Serialization.IDeserializationCallback.OnDeserialization%2A> メソッドが自動的に呼び出されます。 この時点で、参照されているすべての子オブジェクトが完全に復元されます。 ハッシュ テーブルは、イベント リスナーを使用せずに逆シリアル化することが困難なクラスの典型的な例です。 逆シリアル化時にキーと値のペアを取得することは簡単ですが、これらのオブジェクトをハッシュ テーブルに戻すと、このハッシュ テーブルから派生したクラスが逆シリアル化されているかどうかわからないため、問題が発生する可能性があります。 したがって、この段階でハッシュ テーブルのメソッドを呼び出すことはお勧めできません。  
  
## <a name="see-also"></a>関連項目

- [バイナリ シリアル化](binary-serialization.md)
- [XML シリアル化および SOAP シリアル化](xml-and-soap-serialization.md)
- [セキュリティとシリアル化](../../../docs/framework/misc/security-and-serialization.md)
