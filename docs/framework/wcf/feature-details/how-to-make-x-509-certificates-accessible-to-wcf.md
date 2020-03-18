---
title: '方法 : X.509 証明書を WCF からアクセス可能にする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- X.509 certificates [WCF]
- certificates [WCF], making X.509 certificates accessible to WCF
- X.509 certificates [WCF], making accessible to WCF
ms.assetid: a54e407c-c2b5-4319-a648-60e43413664b
ms.openlocfilehash: 71dbf395f43c8028a703a342c032f2b8d022a61c
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921296"
---
# <a name="how-to-make-x509-certificates-accessible-to-wcf"></a>方法 : X.509 証明書を WCF からアクセス可能にする
Windows Communication Foundation (WCF) から x.509 証明書にアクセスできるようにするには、アプリケーションコードで証明書ストアの名前と場所を指定する必要があります。 特定の状況では、X.509 証明書に関連付けられた秘密キーを格納しているファイルにプロセス ID がアクセスできる必要があります。 証明書ストアの x.509 証明書に関連付けられている秘密キーを取得するには、WCF にアクセス許可が必要です。 既定では、所有者と System アカウントだけが証明書の秘密キーにアクセスできます。  
  
### <a name="to-make-x509-certificates-accessible-to-wcf"></a>X.509 証明書を WCF からアクセス可能にするには  
  
1. WCF が実行されているアカウントに、x.509 証明書に関連付けられている秘密キーを含むファイルへの読み取りアクセス権を付与します。  
  
    1. WCF が x.509 証明書の秘密キーへの読み取りアクセスを必要とするかどうかを判断します。  
  
         次の表は、X.509 証明書を使用する際に秘密キーを使用できる必要があるかどうかを示しています。  
  
        |X.509 証明書の使用法|秘密キー|  
        |---------------------------|-----------------|  
        |送信 SOAP メッセージにデジタル署名する。|○|  
        |受信 SOAP メッセージの署名を検証する。|いいえ|  
        |送信 SOAP メッセージを暗号化する。|いいえ|  
        |受信 SOAP メッセージを復号化する。|○|  
  
    2. 証明書が格納されている証明書ストアの場所と名前を決定します。  
  
         証明書が格納されている証明書ストアは、アプリケーション コードまたは構成で指定します。 たとえば、次の例では、証明書が `CurrentUser` という名前の `My` 証明書ストア内に存在することを指定します。  
  
         [!code-csharp[x509Accessible#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/x509accessible/cs/source.cs#1)]
         [!code-vb[x509Accessible#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/x509accessible/vb/source.vb#1)]  
  
    3. [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md)ツールを使用して、証明書の秘密キーがコンピューター上にある場所を確認します。  
  
         [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md)ツールには、証明書ストアの名前、証明書ストアの場所、および証明書を一意に識別するものが必要です。 このツールは、証明書のサブジェクト名または拇印を一意の識別子として受け入れます。 証明書の拇印を確認する方法の詳細については、「[方法: 証明書のサムプリントを取得](../../../../docs/framework/wcf/feature-details/how-to-retrieve-the-thumbprint-of-a-certificate.md)する」を参照してください。  
  
         次のコード例では、 [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md)ツールを使用して、拇印が `46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d`の `CurrentUser` の `My` ストアにある証明書の秘密キーの場所を確認します。  
  
        ```console
        findprivatekey.exe My CurrentUser -t "46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d" -a  
        ```  
  
    4. WCF が実行されているアカウントを確認します。  
  
         次の表は、特定のシナリオで WCF が実行されているアカウントの詳細を示しています。  
  
        |通信の種類|プロセス ID|  
        |--------------|----------------------|  
        |クライアント (コンソールまたは WinForms アプリケーション)|現在ログインしているユーザー|  
        |自己ホスト型のサービス|現在ログインしているユーザー|  
        |IIS 6.0 (Windows Server 2003) または IIS 7.0 (Windows Vista) でホストされるサービス。|NETWORK SERVICE|  
        |IIS 5.x (Windows XP) でホストされているサービス。|Machine.config ファイル内の `<processModel>` 要素によって制御されます。 既定のアカウントは ASPNET です。|  
  
    5. Icacls などのツールを使用して、WCF を実行しているアカウントに、秘密キーを含むファイルへの読み取りアクセス権を付与します。  
  
         次のコード例では、指定されたファイルの随意アクセス制御リスト (DACL) を編集して、ネットワークサービスアカウントにファイルへの読み取り (: R) アクセスを許可します。  
  
        ```console 
        icacls.exe "C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys\8aeda5eb81555f14f8f9960745b5a40d_38f7de48-5ee9-452d-8a5a-92789d7110b1" /grant "NETWORK SERVICE":R  
        ```  
  
## <a name="see-also"></a>関連項目

- [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md)
- [方法 : 証明書のサムプリントを取得する](../../../../docs/framework/wcf/feature-details/how-to-retrieve-the-thumbprint-of-a-certificate.md)
- [証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)
