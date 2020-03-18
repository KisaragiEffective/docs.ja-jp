---
title: カスタム永続参加要素を作成する方法
ms.date: 03/30/2017
ms.assetid: 1d9cc47a-8966-4286-94d5-4221403d9c06
ms.openlocfilehash: 0e61395cb59a7d162668445d23241e3ff562d67b
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802545"
---
# <a name="how-to-create-a-custom-persistence-participant"></a>カスタム永続参加要素を作成する方法
次の手順では、永続参加要素を作成します。 永続参加要素の実装例については、永続化のサンプルと[ストアの機能拡張](store-extensibility.md)[に](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd699769(v=vs.100))関するトピックを参照してください。  
  
1. <xref:System.Activities.Persistence.PersistenceParticipant> または <xref:System.Activities.Persistence.PersistenceIOParticipant> クラスから派生するクラスを作成します。 PersistenceIOParticipant クラスは、i/o 操作に参加できるだけでなく、PersistenceParticipant クラスと同じ機能拡張ポイントを提供します。 次のうち、必要な手順を行います。  
  
2. <xref:System.Activities.Persistence.PersistenceParticipant.CollectValues%2A> メソッドを実装します。 **Collectvalues**メソッドには、2つのディクショナリパラメーターがあります。1つは読み取り/書き込み値を格納するためのもので、もう1つは書き込み専用の値を格納するためのもので、後でクエリで使用します。 このメソッドでは、永続参加要素に固有のデータをこれらのディクショナリに設定する必要があります。 各ディクショナリには、値の名前がキーとして格納されているほか、値そのものが <xref:System.Runtime.DurableInstancing.InstanceValue> オブジェクトとして格納されています。  
  
    ReadWriteValues ディクショナリ内の値は、 **Instancevalue**オブジェクトとしてパッケージ化されます。 書き込み専用辞書の値は、InstanceValueOptions を使用して**Instancevalue**オブジェクトとしてパッケージ化されます。省略可能で、InstanceValueOption. WriteOnly set です。 すべての永続参加要素の**Collectvalues**実装によって提供される各**instancevalue**には、一意の名前を付ける必要があります。
  
    ```csharp  
    protected virtual void CollectValues(out IDictionary<XName,Object> readWriteValues, out IDictionary<XName,Object> writeOnlyValues)
    {
    }
    ```  
  
3. <xref:System.Activities.Persistence.PersistenceParticipant.MapValues%2A> メソッドを実装します。 **Mapvalues**メソッドは、 **collectvalues**メソッドが受け取るパラメーターに似た2つのパラメーターを受け取ります。 **Collectvalues**ステージで収集されたすべての値は、これらのディクショナリパラメーターを通じて渡されます。 **Mapvalues**ステージによって追加された新しい値は、書き込み専用の値に追加されます。  書き込み専用のディクショナリが、インスタンスの値に直接関連付けられていない外部ソースにデータを提供するために使用されます。 すべての永続参加要素で**Mapvalues**メソッドの実装によって提供される各値には、一意の名前を付ける必要があります。  
  
    ```csharp  
    protected virtual IDictionary<XName,Object> MapValues(IDictionary<XName,Object> readWriteValues,IDictionary<XName,Object> writeOnlyValues)
    {
    }
    ```  
  
     <xref:System.Activities.Persistence.PersistenceParticipant.MapValues%2A> メソッドは <xref:System.Activities.Persistence.PersistenceParticipant.CollectValues%2A> が提供しない機能を提供し、<xref:System.Activities.Persistence.PersistenceParticipant.CollectValues%2A> が処理しなかった別の永続参加により提供される、別の値への依存関係が許可されます。  
  
4. **Publishvalues**メソッドを実装します。 **Publishvalues**メソッドは、永続化ストアから読み込まれたすべての値を含むディクショナリを受け取ります。  
  
    ```csharp  
    protected virtual void PublishValues(IDictionary<XName,Object> readWriteValues)
    {
    }
    ```  
  
5. 参加要素が永続 i/o 参加要素の場合は、 **Beginonsave**メソッドを実装します。 このメソッドは保存操作中に呼び出されます。 この方法では、ワークフローインスタンスの永続化 (保存) を行うために i/o 補完を実行する必要があります。  ホストが、対応する永続化コマンドのトランザクションを使用している場合、同じトランザクションが Transaction.Current で確立されます。  さらに、PersistenceIOParticipant はトランザクションの一貫性の要件を通知することがあります。この場合、ホストはほかに使用されなければ、永続化のトランザクションを作成します。  
  
    ```csharp  
    protected virtual IAsyncResult BeginOnSave(IDictionary<XName,Object> readWriteValues, IDictionary<XName,Object> writeOnlyValues, TimeSpan timeout, AsyncCallback callback, Object state)
    {
    }
    ```  
  
6. 参加要素が永続 i/o 参加要素の場合は、 **Beginonload**メソッドを実装します。 このメソッドは読み込み操作中に呼び出されます。 このメソッドでは、ワークフローインスタンスの読み込みに対して i/o 補完を実行する必要があります。 ホストが、対応する永続化コマンドのトランザクションを使用している場合、同じトランザクションが Transaction.Current で確立されます。 さらに、永続化 i/o の参加者はトランザクションの一貫性の要件を提供する場合があります。その場合、ホストは永続化のためのトランザクションを作成します (これを使用しない場合)。  
  
    ```csharp  
    protected virtual IAsyncResult BeginOnLoad(IDictionary<XName,Object> readWriteValues, TimeSpan timeout, AsyncCallback callback, Object state)
    {
    }
    ```
