---
description: 詳細については <add> 、 <filters>
title: <add> の <filters>
ms.date: 03/30/2017
ms.assetid: e3bf437c-dd99-49f3-9792-9a8721e6eaad
ms.openlocfilehash: 546ff41fddfd8a48e14508e27f09236c67c9abc9
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99802347"
---
# <a name="add-of-filters"></a>\<add> の \<filters>

ログに記録するメッセージの種類を指定する XPath フィルター。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<diagnostics>**](diagnostics.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<messageLogging>**](messagelogging.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<filters>**](filters.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>構文  
  
```xml  
<filters>
  <add filter="String" />
</filters>
```  
  
## <a name="attributes-and-elements"></a>属性および要素  

 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|filter|XPath 1.0 の式によって定義される、XML ドキュメントのクエリを指定する文字列。 詳細については、「<xref:System.ServiceModel.Dispatcher.XPathMessageFilter>」を参照してください。|  
  
### <a name="child-elements"></a>子要素  

 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<filters>](filters.md)|ログに記録されるメッセージの種類を制御する XPath フィルターのコレクションを格納します。|  
  
## <a name="remarks"></a>解説  

 フィルターは、`logMessagesAtTransportLevel` を `true` に設定することによって指定されるトランスポート層でのみ適用されます。 サービス レベルおよび形式が正しくないメッセージ ログ記録は、フィルターの影響を受けません。  
  
 コレクションにフィルターを追加するには、`add` を使用します。 1 つ以上のフィルターを定義した場合は、少なくとも 1 つのフィルターと一致するメッセージだけが記録されます。 フィルターを定義しなかった場合は、すべてのメッセージが通過します。  
  
 フィルターは、完全な XPath 構文をサポートし、構成ファイルでの出現順に適用されます。 フィルターが構文的に正しくない場合は、構成例外が発生します。  
  
 SOAP ヘッダー セクションがあるメッセージだけを記録するフィルターの設定方法の例を次に示します。  
  
## <a name="example"></a>例  

 SOAP ヘッダー セクションがあるメッセージだけを記録するフィルターの設定方法の例を次に示します。  
  
```xml  
<messageLogging logEntireMessage="true"
                logMalformedMessages="true"
                logMessagesAtServiceLevel="true"
                logMessagesAtTransportLevel="true"
                maxMessagesToLog="420">
  <filters>
    <add xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
      /soap:Envelope/soap:Headers
    </add>
  </filters>
</messageLogging>
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
- <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement.Filters%2A>
- <xref:System.ServiceModel.Configuration.XPathMessageFilterElement>
- <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>
- [メッセージ ログの構成](../../../wcf/diagnostics/configuring-message-logging.md)
- [\<messageLogging>](messagelogging.md)
