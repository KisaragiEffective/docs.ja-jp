---
title: '方法 : AJAX 対応 ASP.NET Web サービスを WCF に移行する'
ms.date: 03/30/2017
ms.assetid: 1428df4d-b18f-4e6d-bd4d-79ab3dd5147c
ms.openlocfilehash: 60e3088b9075464176c328af1f52676a69c4990f
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976143"
---
# <a name="how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf"></a>方法 : AJAX 対応 ASP.NET Web サービスを WCF に移行する
このトピックでは、基本的な ASP.NET AJAX サービスを同等の AJAX 対応 Windows Communication Foundation (WCF) サービスに移行する手順について説明します。 ここでは、ASP.NET AJAX サービスの機能的に同等の WCF バージョンを作成する方法を示します。 2つのサービスを並行して使用することも、WCF サービスを使用して ASP.NET AJAX サービスを置き換えることもできます。

 既存の ASP.NET AJAX サービスを WCF AJAX サービスに移行すると、次のような利点があります。

- 最小限の追加構成で、AJAX サービスを SOAP サービスとして公開できます。

- トレースなどの WCF 機能を活用できます。

 次の手順では、Visual Studio 2012 を使用していることを前提としています。

 手順に続く例で、ここで説明する手順によって作成されるコードを示します。

 AJAX 対応エンドポイントを使用して WCF サービスを公開する方法の詳細については、「[方法: 構成を使用して ASP.NET AJAX エンドポイントを追加](../../../../docs/framework/wcf/feature-details/how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md)する」を参照してください。

### <a name="to-create-and-test-the-aspnet-web-service-application"></a>ASP.NET Web サービス アプリケーションを作成してテストする

1. Visual Studio 2012 を開きます。

2. **[ファイル]** メニューの **[新規作成]** 、 **[プロジェクト]** 、 **[web]** の順に選択し、 **[ASP.NET web Service Application]** を選択します。

3. プロジェクトに `ASPHello` という名前を指定し、[ **OK]** をクリックします。

4. Service1.asmx.cs ファイルで、`System.Web.Script.Services.ScriptService]` が含まれた行のコメントを解除し、このサービスに対して AJAX を有効にします。

5. **[ビルド]** メニューの **[ソリューションのビルド]** をクリックします。

6. **[デバッグ]** メニューの **[デバッグなしで開始]** をクリックします。

7. 生成された Web ページで、`HelloWorld` 操作を選択します。

8. `HelloWorld` テストページで、 **[起動]** ボタンをクリックします。 次の XML 応答を受信します。

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <string xmlns="http://tempuri.org/">Hello World</string>
    ```

9. この応答は、ASP.NET AJAX サービスが現在機能していること、およびこのサービスが現在 Service1.asmx/HelloWorld でエンドポイントを公開していることを確認するものです。このエンドポイントが HTTP POST 要求に応答し、XML を返します。

     これで、WCF AJAX サービスを使用するようにこのサービスを変換する準備ができました。

### <a name="to-create-an-equivalent-wcf-ajax-service-application"></a>同等の WCF AJAX サービス アプリケーションを作成するには

1. **Asphello**プロジェクトを右クリックし、 **[追加]** 、 **[新しい項目]** 、 **[AJAX 対応 WCF サービス]** の順に選択します。

2. サービスに `WCFHello` 名前を指定し、 **[追加]** をクリックします。

3. WCFHello.svc.cs ファイルを開きます。

4. Service1.asmx.cs から、`HelloWorld` 操作の次の実装をコピーします。

    ```csharp
    public string HelloWorld()
    {
        return "Hello World";
    }
    ```

5. 次のコードの代わりに、コピーした `HelloWorld` 操作の実装を WCFHello.svc.cs ファイルに貼り付けます。

    ```csharp
    public void DoWork()
    {
          // Add your operation implementation here
          return;
    }
    ```

6. <xref:System.ServiceModel.ServiceContractAttribute> の `Namespace` 属性を `WCFHello`として指定します。

    ```csharp
    [ServiceContract(Namespace="WCFHello")]
    [AspNetCompatibilityRequirements(RequirementsMode=AspNetCompatibilityRequirementsMode.Required)]
    public class WCFHello
    { … }
    ```

7. `HelloWorld` 操作に <xref:System.ServiceModel.Web.WebInvokeAttribute> を追加し、<xref:System.ServiceModel.Web.WebMessageFormat.Xml>を返すように <xref:System.ServiceModel.Web.WebInvokeAttribute.ResponseFormat%2A> プロパティを設定します。 この設定を行わない場合、既定の戻り値の型は <xref:System.ServiceModel.Web.WebMessageFormat.Json> です。

    ```csharp
    [OperationContract]
    [WebInvoke(ResponseFormat=WebMessageFormat.Xml)]
    public string HelloWorld()
    {
        return "Hello World";
    }
    ```

8. **[ビルド]** メニューの **[ソリューションのビルド]** をクリックします。

9. Wcfhello.svc ファイルを開き、 **[デバッグ]** メニューの **[デバッグなしで開始]** を選択します。

10. サービスは、HTTP POST 要求に応答する `WCFHello.svc/HelloWorld`でエンドポイントを公開するようになりました。 HTTP POST 要求をブラウザーからテストすることはできませんが、エンドポイントは次の XML を返します。

    ```xml
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Hello World</string>
    ```

11. `WCFHello.svc/HelloWorld` と `Service1.aspx/HelloWorld` エンドポイントが機能的に同等になりました。

## <a name="example"></a>例
 このトピックで説明した手順によって作成されるコードを次の例に示します。

```csharp
//This is the ASP.NET code in the Service1.asmx.cs file.

using System;
using System.Collections;
using System.ComponentModel;
using System.Data;
using System.Linq;
using System.Web;
using System.Web.Services;
using System.Web.Services.Protocols;
using System.Xml.Linq;
using System.Web.Script.Services;

namespace ASPHello
{
    /// <summary>
    /// Summary description for Service1.
    /// </summary>
    [WebService(Namespace = "http://tempuri.org/")]
    [WebServiceBinding(ConformsTo = WsiProfiles.BasicProfile1_1)]
    [ToolboxItem(false)]
    // To allow this Web Service to be called from script, using ASP.NET AJAX, uncomment the following line.
    [System.Web.Script.Services.ScriptService]
    public class Service1 : System.Web.Services.WebService
    {

        [WebMethod]
        public string HelloWorld()
        {
            return "Hello World";
        }
    }
}

//This is the WCF code in the WCFHello.svc.cs file.
using System;
using System.Linq;
using System.Runtime.Serialization;
using System.ServiceModel;
using System.ServiceModel.Activation;
using System.ServiceModel.Web;

namespace ASPHello
{
    [ServiceContract(Namespace = "WCFHello")]
    [AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]
    public class WCFHello
    {
        // Add [WebInvoke] attribute to use HTTP GET.
        [OperationContract]
        [WebInvoke(ResponseFormat=WebMessageFormat.Xml)]
        public string HelloWorld()
        {
            return "Hello World";
        }

        // Add more operations here and mark them with [OperationContract].
    }
}
```

 <xref:System.Xml.XmlDocument> 型は、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> によってシリアル化できないため、<xref:System.Xml.Serialization.XmlSerializer> ではサポートされていません。 <xref:System.Xml.Linq.XDocument> 型を使用するか、<xref:System.Xml.XmlDocument.DocumentElement%2A> をシリアル化します。

 ASMX Web サービスを WCF サービスに対してサイドバイサイドでアップグレードおよび移行する場合は、2つの型をクライアントで同じ名前にマップしないようにしてください。 同じ名前が割り当てられていると、<xref:System.Web.Services.WebMethodAttribute> と <xref:System.ServiceModel.ServiceContractAttribute> で同じ型が使用されている場合に、シリアライザーで次のような例外が発生します。

- WCF サービスが先に追加された場合、ASMX Web サービスでメソッドを呼び出すと <xref:System.Web.UI.ObjectConverter.ConvertValue%28System.Object%2CSystem.Type%2CSystem.String%29> で例外が発生します。これは、プロキシでの順序の WCF スタイル定義が優先されるためです。

- ASMX Web サービスが先に追加された場合、WCF サービスでメソッドを呼び出すと <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> で例外が発生します。これは、プロキシでの順序の Web サービススタイル定義が優先されるためです。

 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> と ASP.NET AJAX <xref:System.Web.Script.Serialization.JavaScriptSerializer> の動作には大きな違いがあります。 たとえば、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> はディクショナリをキーと値のペアの配列として表しますが、ASP.NET AJAX <xref:System.Web.Script.Serialization.JavaScriptSerializer> はディクショナリを実際の JSON オブジェクトとして表します。 したがって、ASP.NET AJAX では、ディクショナリが次のように表されます。

```csharp
Dictionary<string, int> d = new Dictionary<string, int>();
d.Add("one", 1);
d.Add("two", 2);
```

 このディクショナリは、JSON オブジェクトでは次のように表されます。

- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> では [{"Key":"one","Value":1},{"Key":"two","Value":2}] と表され、

- ASP.NET AJAX <xref:System.Web.Script.Serialization.JavaScriptSerializer> による {"one": 1, "two": 2}

 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> は、キーの種類が文字列ではないディクショナリを処理でき、<xref:System.Web.Script.Serialization.JavaScriptSerializer> はできません。この点で前者はより強力と言えます。 しかし、後者の方が JSON で使いやすいと言えます。

 これらのシリアライザーの主な違いを次の表に示します。

|相違点のカテゴリ|DataContractJsonSerializer|ASP.NET AJAX JavaScriptSerializer|
|-----------------------------|--------------------------------|---------------------------------------|
|空きバッファー (新しい byte[0]) の <xref:System.Object> (または <xref:System.Uri>、あるいは他の一部のクラス) への逆シリアル化|SerializationException|null|
|<xref:System.DBNull.Value> のシリアル化|{} (または {"__type": "#System"})|Null|
|[Serializable] 型のプライベート メンバーのシリアル化|できるか|シリアル化できません|
|<xref:System.Runtime.Serialization.ISerializable> 型のパブリック プロパティのシリアル化|シリアル化できません|できるか|
|JSON の「拡張機能」|オブジェクト メンバー名で引用符を必要とする ({"a":"hello"}) JSON 仕様に準拠しています。|引用符のないオブジェクト メンバー名 ({a:"hello"}) をサポートします。|
|<xref:System.DateTime> 協定世界時刻 (UTC)|形式 "\\/日付 (/)\\/" または "\\/日付\\はサポートされません (\d +&#124;(U (\\+\\-[\d{4}]))?\\)\\\\/) "です。|Format "\\/date (/)\\/" および "\\/日付\\(\d + (U&#124;(\\+\\-[\d{4}])) をサポートしていますか?\\)\\\\/) "を DateTime 値として指定します。|
|ディクショナリの表現|KeyValuePair\<K, V > の配列は、文字列ではないキーの種類を処理します。|実際の JSON オブジェクトですが、文字列の種類のキーのみ処理します。|
|エスケープ文字|必ず、エスケープ文字であるスラッシュ (/) を付けます。"\n" などのエスケープされない無効な JSON 文字は使用できません。|DateTime 値には、エスケープ文字スラッシュ (/) を付けます。|

## <a name="see-also"></a>関連項目

- [方法 : 構成を使用して ASP.NET AJAX エンドポイントを追加する](../../../../docs/framework/wcf/feature-details/how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md)
