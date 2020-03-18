---
title: Secure Sockets Layerの使用
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Networking
- SSL
- Secure Sockets Layer
- requesting data from Internet, Secure Sockets Layer
- sending data, Secure Sockets Layer
- Network Resources
- data requests, Secure Sockets Layer
- receiving data, Secure Sockets Layer
- Internet, Secure Sockets Layer
ms.assetid: 6e4289e6-d1b7-4e82-ab0d-e83e3b6063ed
ms.openlocfilehash: ef2abc7574aea1b4f77ff93545ad84678c66ce48
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71046904"
---
# <a name="using-secure-sockets-layer"></a>Secure Sockets Layerの使用
<xref:System.Net> クラスは、Secure Sockets Layer (SSL) を使用して、複数のネットワーク プロトコルの接続を暗号化します。  
  
 HTTP 接続の場合、<xref:System.Net.WebRequest> クラスと <xref:System.Net.WebResponse> クラスは SSL を使用して、SSL をサポートする Web ホストと通信します。 SSL の使用は、指定された URI に基づいて <xref:System.Net.WebRequest> クラスで決定されます。 URI が "https:" で始まる場合は SSL が使用されます。URI が "http:" で始まる場合は、暗号化されていない接続が使用されます。  
  
 ファイル転送プロトコル (FTP) で SSL を使用するには、<xref:System.Net.FtpWebRequest.EnableSsl> プロパティを true に設定してから <xref:System.Net.FtpWebRequest.GetResponse> を呼び出します。 同様に、簡易メール転送プロトコル (SMTP) で SSL を使用するには、<xref:System.Net.Mail.SmtpClient.EnableSsl> プロパティを true に設定してからメールを送信します。  
  
 <xref:System.Net.Security.SslStream> クラスは、SSL のストリームベースの抽象化を提供します。また、SSL ハンドシェイクを構成する方法が多く用意されています。  
  
## <a name="example"></a>例  
  
### <a name="code"></a>コード  
  
```vb  
Dim MyURI As String = "https://www.contoso.com/"  
Dim Wreq As WebRequest = WebRequest.Create(MyURI)  
  
Dim serverUri As String = "ftp://ftp.contoso.com/file.txt"  
Dim request As FtpWebRequest = CType(WebRequest.Create(serverUri), FtpWebRequest)  
request.Method = WebRequestMethods.Ftp.DeleteFile  
request.EnableSsl = True  
Dim response As FtpWebResponse = CType(request.GetResponse(), FtpWebResponse)  
```  
  
```csharp  
String MyURI = "https://www.contoso.com/";  
WebRequest WReq = WebRequest.Create(MyURI);  
  
String serverUri = "ftp://ftp.contoso.com/file.txt"  
FtpWebRequest request = (FtpWebRequest)WebRequest.Create(serverUri);  
request.EnableSsl = true;  
request.Method = WebRequestMethods.Ftp.DeleteFile;  
FtpWebResponse response = (FtpWebResponse)request.GetResponse();  
```  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- **System.Net** 名前空間の参照。  
  
## <a name="see-also"></a>参照

- [ネットワーク プログラミングにおけるセキュリティ](security-in-network-programming.md)
- [.NET Framework のネットワーク プログラミング](index.md)
- [証明書の選択と検証](certificate-selection-and-validation.md)
