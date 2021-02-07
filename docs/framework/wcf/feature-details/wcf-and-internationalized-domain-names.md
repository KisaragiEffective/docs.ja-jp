---
description: 詳細については、「WCF と国際化ドメイン名」を参照してください。
title: WCF と国際化ドメイン名
ms.date: 03/30/2017
ms.assetid: c8a3e10a-8bc2-4a78-8d86-a562ba6e65fa
ms.openlocfilehash: e7e1ad999d7de749b4f8cf4b3efd823befffba43
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99755994"
---
# <a name="wcf-and-internationalized-domain-names"></a>WCF と国際化ドメイン名

国際化ドメイン名 (IDN) を持つ WCF サービスを許可するためのサポートが追加されました。 国際化ドメイン名とは、非 ASCII 文字を含むドメイン名です。 このサポートには、IDN 名を持つ WCF サービスをホストする機能と、IDN 名を持つ Web サービスとの通信を行う WCF クライアントをホストする機能の両方が含まれます。  
  
## <a name="systemuri-and-idn"></a>System.Uri と IDN  

 <xref:System.Uri> には、<xref:System.Uri.Host%2A> および <xref:System.Uri.DnsSafeHost%2A> という 2 つのプロパティがあります。 これらのプロパティには、IDN 構成設定に応じて Unicode 値または Punycode 値が格納されます。  
  
 IDN をアプリケーションの構成ファイルで有効にするには、次の XML を使用します。  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All/AllExceptIntranet/None" />  
  </uri>  
</configuration>  
```  
  
 要素には、 \<idn> 次のいずれかの値に設定できる enabled 属性が含まれています。  
  
1. "None"  
  
2. "AllExceptIntranet"  
  
3. "All"  
  
 IDN 設定が "None" に設定されている場合、Uri. Host または Uri. DnsSafeHost で変換は実行されません。 IDN 設定が "All" に設定されている場合は uri です。ホストは Unicode と uri のままです。DnsSafeHost は Punycode に変換されます。 IDN 設定が "AllExceptIntranet" に設定されている場合は uri です。DnsSafeHost は、インターネットアドレスの場合は Punycode に変換され、イントラネットアドレスの場合は Unicode のままです。 この設定は、正しい DNS 名解決にとって重要です。 Windows 8 以降のバージョンでは、この設定を構成する必要はありません。  
  
> [!WARNING]
> Punycode を使用してアドレスをハードコーディングしないようにしてください。 アドレスは、適用した構成設定に基づき、WCF によって自動的に変換されます。  
  
> [!WARNING]
> Unicode 文字を applicationHost.exe.config に追加する場合は、UTF-8 エンコードを使用してファイルを保存してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Uri?displayProperty=nameWithType>
