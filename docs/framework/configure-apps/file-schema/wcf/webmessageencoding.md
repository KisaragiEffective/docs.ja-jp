---
title: <webMessageEncoding>
ms.date: 03/30/2017
ms.assetid: 892ca485-e21a-4a44-8e40-633161ef6796
ms.openlocfilehash: 4aa87acaf9080959ba8b53e3ec3216314dc745b6
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73732588"
---
# <a name="webmessageencoding"></a>\<webMessageEncoding >
Windows Communication Foundation (WCF) での使用時に、プレーンテキストの XML、JavaScript Object Notation (JSON) メッセージ エンコード、および "無変換の" バイナリ コンテンツの読み取りおよび書き込みを有効にします。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp; &nbsp;[ **\<system >** ](system-servicemodel.md) \
&nbsp;&nbsp;&nbsp;&nbsp;\<[**バインド**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**customBinding >** ](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**バインド >** \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**webMessageEncoding >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<webMessageEncoding maxReadPoolSize="Integer"
                    maxWritePoolSize="Integer"
                    writeEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding" />
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`maxReadPoolSize`|新しいリーダーを割り当てずに同時に読み取ることができるメッセージの数。 プール サイズを大きくすると、システムでは、比較的大きい作業セットで、アクティビティの急増に対する許容度が高まります。 既定値は、内部エンコーダー (Text、JSON、および raw (生)) ごとに 64 リーダーです。<br /><br /> この数を増やすとメモリの消費量が増えますが、受信メッセージ数の急激な増加にエンコーダーが対処できるようになります。これは、作成済みのリーダーをプールから使用でき、新しいリーダーを作成する必要がないためです。|  
|`maxWritePoolSize`|新しいライターを割り当てずに同時に送信できるメッセージの数。 プール サイズを大きくすると、システムでは、比較的大きい作業セットで、アクティビティの急増に対する許容度が高まります。 既定値は、内部エンコーダー (Text、JSON、および raw (生)) ごとに 64 リーダーです。<br /><br /> この数を増やすとメモリの消費が増えますが、送信メッセージ数の急激な増加にエンコーダーが対処できるようになります。これは、作成済みのライターをプールから使用でき、新しいライターを作成する必要がないためです。|  
|`writeEncoding`|バインドでメッセージの発行に使用される文字セット エンコーディングを指定します。 次の値を指定できます。<br /><br /> -UnicodeFffeTextEncoding: Unicode ビッグエンディアンエンコーディング。<br />-Utf16TextEncoding: Unicode エンコーディング。<br />-Utf8TextEncoding: 8 ビットエンコーディング。<br /><br /> 既定値は Utf8TextEncoding です。 この属性は <xref:System.Text.Encoding> 型です。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[readerQuotas > の \<](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|このバインドを使用して設定されるエンドポイントにより処理可能な、SOAP メッセージの複雑さに対する制約を定義します。 この要素は <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> 型です。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<binding >](bindings.md)|カスタム バインドのすべてのバインド機能を定義します。|  
  
## <a name="remarks"></a>Remarks  
 エンコーディングは、メッセージをバイト シーケンスに変換するプロセスです。 デコードは、その逆のプロセスです。 これらのプロセスでは、文字エンコーディングの指定が必要です。  
  
 `webMessageEncoding` 要素は、プレーンテキストの XML と JSON エンコーディング、および "生" のバイナリ データの処理を、一連の内部エンコーダーに代行させることで機能します。 この委任は、複合メッセージ エンコーダーによって実行されます。  
  
 このバインディング要素と複合エンコーダーは、`webHttpBinding` 要素によって使用される SOAP メッセージを使用しないシナリオでのエンコーディングを制御するために使用されます。 これらのシナリオには、POX ("Plain Old XML")、REST (Representational State Transfer)、RSS (Really Simple Syndication) と Atom 配信、および AJAX (Asynchronous JavaScript and XML) が含まれます。 複合メッセージ エンコーダーは SOAP または WS-Addressing をサポートしません。  
  
 `writeEncoding` 属性を使用すると、バインディング要素に書き込みの文字エンコーディングを構成できます。 指定された <xref:System.Text.Encoding> 値は、JSON およびテキスト形式の XML の場合の書き込みの動作を指定します。 読み取りについては、任意の有効なメッセージ エンコーディングとテキスト エンコーディングが認識されます。  
  
 `maxReadPoolSize` と `maxWritePoolSize` を使用して、割り当てられるリーダーとライターの最大数をそれぞれ設定することもできます。 既定では、64 のリーダーと 16 のライターが割り当てられています。  
  
 既定の複雑さの制約も、 [\<readerQuotas >](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))要素を使用して設定されます。これは、メッセージの複雑さを使用してエンドポイント処理リソースを結び付ける、サービス拒否 (dos) のクラスから保護します。  
  
## <a name="example"></a>例  
  
```xml  
<webMessageEncoding maxReadPoolSize="256"
                    maxWritePoolSize="128"
                    messageVersion="None"
                    textEncoding="utf-8" />
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.WebMessageEncodingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement>
- [メッセージ エンコーディング](message-encoding.md)
- [メッセージ エンコーダーを選択する](../../../wcf/feature-details/choosing-a-message-encoder.md)
- [バインディング](../../../wcf/bindings.md)
- [バインディングの拡張](../../../wcf/extending/extending-bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
