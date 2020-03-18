---
title: SendMail カスタム アクティビティ
ms.date: 03/30/2017
ms.assetid: 947a9ae6-379c-43a3-9cd5-87f573a5739f
ms.openlocfilehash: 90b3192d931b216345b50ba49465455427e43a64
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094606"
---
# <a name="sendmail-custom-activity"></a>SendMail カスタム アクティビティ
このサンプルでは、<xref:System.Activities.AsyncCodeActivity> から派生するカスタム アクティビティを作成して、SMTP を使用して電子メールを送信し、ワークフロー アプリケーション内で使用する方法を示します。 カスタムアクティビティでは、<xref:System.Net.Mail.SmtpClient> の機能を使用して、電子メールを非同期的に送信し、認証を使用してメールを送信します。 また、テスト モード、トークン置換、ファイル テンプレート、テスト ドロップ パスなどのエンドユーザーの機能も提供しています。  
  
 次の表で、`SendMail` アクティビティの引数の詳細を説明します。  
  
|Name|種類|[説明]|  
|-|-|-|  
|Host|String|SMTP サーバー ホストのアドレス。|  
|Port|String|ホストの SMTP サービスのポート。|  
|EnableSsl|[bool]|<xref:System.Net.Mail.SmtpClient> が、接続を暗号化するために SSL (Secure Sockets Layer) を使用するかどうかを指定します。|  
|UserName|String|差出人の <xref:System.Net.Mail.SmtpClient.Credentials%2A> プロパティを認証する資格情報を設定するユーザー名。|  
|Password|String|差出人の <xref:System.Net.Mail.SmtpClient.Credentials%2A> プロパティを認証する資格情報を設定するパスワード。|  
|サブジェクト|<xref:System.Activities.InArgument%601>\<文字列 >|メッセージの件名。|  
|Body|<xref:System.Activities.InArgument%601>\<文字列 >|メッセージの本文。|  
|[Attachments]|<xref:System.Activities.InArgument%601>\<文字列 >|この電子メールメッセージに添付されたデータを格納するために使用される添付ファイルのコレクションです。|  
|ソース|<xref:System.Net.Mail.MailAddress>|この電子メールメッセージの差出人アドレス。|  
|ターゲット|<xref:System.Activities.InArgument%601>\<<xref:System.Net.Mail.MailAddressCollection>>|この電子メールメッセージの受信者を含むアドレスコレクション。|  
|CC|<xref:System.Activities.InArgument%601>\<<xref:System.Net.Mail.MailAddressCollection>>|この電子メールメッセージのカーボンコピー (CC) 受信者を格納するアドレスのコレクション。|  
|BCC|<xref:System.Activities.InArgument%601>\<<xref:System.Net.Mail.MailAddressCollection>>|この電子メールメッセージの BCC (ブラインドカーボンコピー) 受信者を含むアドレスコレクション。|  
|トークン|<xref:System.Activities.InArgument%601>< IDictionary\<文字列、文字列 > >|本文で置換するトークン。 この機能を使用すると、本文にいくつかの値を指定した後、このプロパティを使用して提供されるトークンで置換できます。|  
|BodyTemplateFilePath|String|本文のテンプレートのパス。 `SendMail` アクティビティは、このファイルの内容をその body プロパティにコピーします。<br /><br /> テンプレートは、tokens プロパティの内容によって置き換えられるトークンを含めることができます。|  
|TestMailTo|<xref:System.Net.Mail.MailAddress>|このプロパティを設定すると、すべての電子メールが、その中に指定されたアドレスに送信されます。<br /><br /> このプロパティは、ワークフローをテストするときに使用するためのものです。 たとえば、すべての電子メールが実際の受信者に送信されることなく送信されるようにする場合です。|  
|TestDropPath|String|このプロパティが設定されている場合、すべての電子メールも指定したファイルに保存されます。<br /><br /> このプロパティは、ワークフローをテストまたはデバッグするときに使用することを目的としており、送信メールの形式と内容が適切であることを確認します。|  
  
## <a name="solution-contents"></a>ソリューションのコンテンツ  
 ソリューションには、次の 2 つのプロジェクトが含まれています。  
  
|Project|[説明]|重要なファイル|  
|-------------|-----------------|---------------------|  
|SendMail|SendMail アクティビティ|1. SendMail.cs: メインアクティビティの実装<br />2. SendMailDesigner .xaml and SendMailDesigner.xaml.cs: SendMail アクティビティのデザイナー<br />3. MailTemplateBody .htm: 送信する電子メールのテンプレート。|  
|SendMailTestClient|SendMail アクティビティをテストするクライアント。  このプロジェクトでは、SendMail アクティビティを宣言的に起動する方法とプログラムで起動する方法を示します。|1. Sequence1: SendMail アクティビティを呼び出すワークフロー。<br />2. Program.cs: Sequence1 を呼び出し、SendMail を使用するプログラムによってワークフローを作成します。|  
  
## <a name="further-configuration-of-the-sendmail-activity"></a>SendMail アクティビティの追加構成  
 サンプルには表示されませんが、SendMail アクティビティの追加構成を実行できます。 次の 3 つのセクションは、その方法を示しています。  
  
### <a name="sending-an-email-using-tokens-specified-in-the-body"></a>本文で指定されたトークンを使用した電子メールの送信  
 次のコード スニペットでは、本文のトークンを使用して電子メールを送信する方法を示します。 トークンが body プロパティに指定されていることに注目してください。 それらのトークンの値は、tokens プロパティに指定されています。  
  
```csharp  
IDictionary<string, string> tokens = new Dictionary<string, string>();  
tokens.Add("@name", "John Doe");  
tokens.Add("@date", DateTime.Now.ToString());  
tokens.Add("@server", "localhost");  
  
new SendMail  
{  
    From = new LambdaValue<MailAddress>(ctx => new MailAddress("john.doe@contoso.com")),  
    To = new LambdaValue<MailAddressCollection>(  
                    ctx => new MailAddressCollection() { new MailAddress("someone@microsoft.com") }),  
    Subject = "Test email",  
    Body = "Hello @name. This is a test email sent from @server. Current date is @date",  
    Host = "localhost",  
    Port = 25,  
    Tokens = new LambdaValue<IDictionary<string, string>>(ctx => tokens)  
};  
```  
  
### <a name="sending-an-email-using-a-template"></a>テンプレートを使用した電子メールの送信  
 次のスニペットでは、本文のテンプレート トークンを使用して電子メールを送信する方法を示します。 `BodyTemplateFilePath` プロパティを設定するときに、Body プロパティの値を指定する必要がないことに注目してください (テンプレート ファイルのコンテンツは本文にコピーされます)。  
  
```csharp  
new SendMail  
{    
    From = new LambdaValue<MailAddress>(ctx => new MailAddress("john.doe@contoso.com")),  
    To = new LambdaValue<MailAddressCollection>(  
                    ctx => new MailAddressCollection() { new MailAddress("someone@microsoft.com") }),  
    Subject = "Test email",  
    Host = "localhost",  
    Port = 25,  
    Tokens = new LambdaValue<IDictionary<string, string>>(ctx => tokens),  
    BodyTemplateFilePath = @"..\..\..\SendMail\Templates\MailTemplateBody.htm",   
};  
```  
  
### <a name="sending-mails-in-testing-mode"></a>テスト モードでの電子メールの送信  
 このコードスニペットは、2つのテストプロパティを設定する方法を示しています。 `TestMailTo` をに設定すると、すべてのメッセージが `john.doe@contoso.con` に送信されます (To、Cc、Bcc の値には関係ありません)。 TestDropPath を設定すると、送信するすべての電子メールは、指定したパスにも記録されます。 これらのプロパティは、個別に設定できます (関連していません)。  
  
```csharp  
new SendMail  
{    
   From = new LambdaValue<MailAddress>(ctx => new MailAddress("john.doe@contoso.com")),  
   To = new LambdaValue<MailAddressCollection>(  
                    ctx => new MailAddressCollection() { new MailAddress("someone@microsoft.com") }),  
   Subject = "Test email",  
   Host = "localhost",  
   Port = 25,  
   Tokens = new LambdaValue<IDictionary<string, string>>(ctx => tokens),  
   BodyTemplateFilePath = @"..\..\..\SendMail\Templates\MailTemplateBody.htm",  
   TestMailTo= new LambdaValue<MailAddress>(ctx => new MailAddress("john.doe@contoso.com")),  
   TestDropPath = @"c:\Samples\SendMail\TestDropPath\",  
};  
```  
  
## <a name="set-up-instructions"></a>セットアップ手順  
 このサンプルでは SMTP サーバーにアクセスする必要があります。  
  
 SMTP サーバーの設定の詳細については、次のリンクを参照してください。  
  
- [SMTP サービスの構成 (IIS 6.0)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784968(v=ws.10))  
  
- [IIS 7.0: SMTP 電子メールの構成](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772058(v=ws.10))  
  
- [SMTP サービスをインストールする方法](https://docs.microsoft.com/previous-versions/tn-archive/aa997480(v=exchg.65))  
  
 ダウンロードには、サードパーティで提供されている SMTP エミュレーターを使用できます。  
  
##### <a name="to-run-this-sample"></a>このサンプルを実行するには  
  
1. Visual Studio 2010 を使用して、SendMail のソリューションファイルを開きます。  
  
2. 有効な SMTP サーバーへのアクセス権があることを確認してください。 セットアップ手順を参照してください。  
  
3. サーバーアドレス、差出人、および宛先の電子メールアドレスを使用して、プログラムを構成します。  
  
     このサンプルを正しく実行するには、From および To 電子メールアドレスの値と Program.cs の SMTP サーバーのアドレスを構成する必要があります。 プログラムでは電子メールが 2 つの方法で送信されるため、両方の場所でアドレスを変更する必要があります。  
  
4. ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。  
  
5. ソリューションを実行するには、Ctrl キーを押しながら F5 キーを押します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\SendMail`
