---
title: メッセージ ログ記録のセキュリティの考慮事項
ms.date: 03/30/2017
ms.assetid: 21f513f2-815b-47f3-85a6-03c008510038
ms.openlocfilehash: 679975be44244f10232b805a6cc2776b48ed6058
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935767"
---
# <a name="security-concerns-for-message-logging"></a>メッセージ ログ記録のセキュリティの考慮事項
ここでは、メッセージ ログに表示される機密データだけでなく、メッセージ ログによって生成されるイベントを保護する方法についても説明します。  
  
## <a name="security-concerns"></a>セキュリティに関する注意事項  
  
### <a name="logging-sensitive-information"></a>機密情報のログ記録  
 Windows Communication Foundation (WCF) では、アプリケーション固有のヘッダーと本文のデータは一切変更されません。 また、WCF は、アプリケーション固有のヘッダーまたは本文データにある個人情報を追跡しません。  
  
 メッセージのログ記録が有効になっていると、アプリケーション固有ヘッダー内にある個人情報 (クエリ文字列など)、および本文情報 (クレジット カード番号) がログ内で確認できるようになります。 アプリケーションを配置するユーザーは、構成ファイルとログ ファイルに対するアクセス制御を実施する必要があります。 この種の情報を表示しないようにするには、ログ記録を無効にするか、ログを共有する場合にこの種のデータにフィルターをかけて除外します。  
  
 ログ ファイルの内容が意図せず公開されることを防ぐためには、次のヒントに従います。  
  
- Web ホストおよび自己ホストの両方のシナリオにおいて、ログ ファイルがアクセス制御リスト (ACL) によって保護されていることを確認します。  
  
- Web 要求を使用して簡単に処理できないファイル拡張子を選択します。 たとえば、.xml ファイルの拡張子を選ぶのは安全ではありません。 インターネット インフォメーション サービス (IIS) の管理ガイドを参照して、処理できる拡張子のリストを確認します。  
  
- ログ ファイルのある場所への絶対パスを指定します。この場所は、部外者が Web ブラウザーを使用してアクセスできないように、Web ホストの vroot パブリック ディレクトリの外にします。  
  
 既定では、キーおよびユーザー名やパスワードなどの個人を特定できる情報 (PII) は、トレースおよびログに記録するメッセージではありません。 しかし、コンピューターの管理者は、Machine.config ファイルの `enableLoggingKnownPII` 要素にある `machineSettings` 属性を使用して、コンピューター上で実行されているアプリケーションに対し、既知の PII (個人を特定できる情報) のログ記録を許可できます。 次の構成では、この設定方法について示します。  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <machineSettings enableLoggingKnownPii="true"/>  
   </system.serviceModel>  
</configuration>   
```  
  
 アプリケーションを配置するユーザーが App.config ファイルか Web.config ファイルのいずれかで `logKnownPii` 属性を使用することで 、PII ログを可能にする方法を次に示します。  
  
```xml  
<system.diagnostics>  
  <sources>  
      <source name="System.ServiceModel.MessageLogging"  
        logKnownPii="true">  
        <listeners>  
                 <add name="messages"  
                 type="System.Diagnostics.XmlWriterTraceListener"  
                 initializeData="c:\logs\messages.svclog" />  
          </listeners>  
      </source>  
    </sources>  
</system.diagnostics>  
```  
  
 両方の設定が `true` の場合にのみ、PII はログに記録されます。 2 種類のスイッチを組み合わせることで、各アプリケーションで既知の PII のログを柔軟に記録できるようになります。  
  
> [!IMPORTANT]
> [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] では、PII ログ記録を有効にするには、Web.config ファイルまたは App.config ファイルで `logEntireMessage` フラグと `logKnownPii` フラグを `true` に設定する必要があります。`<system.serviceModel><messageLogging logEntireMessage="true" logKnownPii="true" …` のように設定します。  
  
 構成ファイルで 2 種類以上のカスタム ソースを指定しても、最初のソースの属性のみが読み込まれることに注意してください。 他の属性は無視されます。 つまり、2 番目以降の App.config ファイルでは、PII はいずれのソースでもログに記録されなくなります。これは 2 番目のソースで PII のログ記録を明示的に有効にした場合でも同様です。  
  
```xml  
<system.diagnostics>  
   <sources>  
      <source name="System.ServiceModel.MessageLogging"  
              logKnownPii="false">  
              <listeners>  
                 <add name="messages"  
                      type="System.Diagnostics.XmlWriterTraceListener"  
                      initializeData="c:\logs\messages.svclog" />  
              </listeners>  
            </source>  
      <source name="System.ServiceModel"   
              logKnownPii="true">  
              <listeners>  
                 <add name="traces"  
                      type="System.Diagnostics.XmlWriterTraceListener"  
                      initializeData="c:\logs\traces.svclog" />  
              </listeners>  
      </source>  
   </sources>  
</system.diagnostics>  
```  
  
 `<machineSettings enableLoggingKnownPii="Boolean"/>` の要素が Machine.config ファイルに含まれていない場合、システムは <xref:System.Configuration.ConfigurationErrorsException> をスローします。  
  
 変更点はアプリケーションが開始されるか、再起動されるまで、反映されません。 両方の属性も `true` に設定されている場合は、イベントは開始時にログに記録されます。 また、`logKnownPii` が `true` に設定され、`enableLoggingKnownPii` が `false` に設定されている場合にも、イベントはログに記録されます。  
  
 コンピューターの管理者およびアプリケーションを配置するユーザーは、これらの 2 種類のスイッチを使用する場合に注意する必要があります。 PII のログ記録が有効になっている場合は、セキュリティ キーと PII がログに記録されます。 ログ記録を無効にしても、機密情報およびアプリケーション固有のデータは、依然としてメッセージのヘッダーと本体に記録されています。 プライバシーの詳細および PII の公開を防止する方法については、「[ユーザーのプライバシー](https://docs.microsoft.com/previous-versions/dotnet/articles/aa480490(v=msdn.10))」を参照してください。  
  
> [!CAUTION]
> PII は無効なメッセージでは非表示になりません。 このようなメッセージは、変更されずにそのままログに記録されます。 上に示した属性は、このことに影響を与えません。  
  
### <a name="custom-trace-listener"></a>カスタム トレース リスナー  
 カスタム トレース リスナーをメッセージ ログのトレース ソースに追加する権限は、管理者だけに付与する必要があります。 これは、機密情報の漏洩につながるメッセージをリモートで送信するように、悪意のあるカスタム リスナーを構成することが可能なためです。 また、ネットワークを介してリモート データベースなどにメッセージを送信するようにカスタム リスナーを構成している場合は、リモート コンピューター上のメッセージ ログに対する適切なアクセス制御を実施する必要があります。  
  
## <a name="events-triggered-by-message-logging"></a>メッセージ ログ記録でトリガーされるイベント  
 メッセージ ログ記録で発生するすべてのイベントを以下に示します。  
  
- Message logging on : このイベントは、メッセージ ログ記録が構成内で、または WMI を介して有効になっている場合に発生します。 イベントの内容は "メッセージのログ記録が有効になりました。 機密情報は、通信回線上で暗号化されていた場合でも平文で記録される可能性があります (メッセージ本文など)" となります。  
  
- Message logging off : このイベントは、メッセージ ログ記録が WMI を介して無効になっている場合に発生します。 イベントの内容は "メッセージのログ記録が無効になりました" となります。  
  
- Log Known PII On : このイベントは、既知の PII のログ記録が有効になっている場合に発生します。 これは、machine.config ファイルの `machineSettings` 要素の `enableLoggingKnownPii` 属性が `true`に設定されていて、App.config ファイルまたは Web.config ファイルの `source` 要素の `logKnownPii` 属性が `true`に設定されている場合に発生します。  
  
- Log Known PII Not Allowed : このイベントは既知の PII のログ記録が許可されていない場合に発生します。 これは、App.config ファイルまたは web.config ファイルの `source` 要素の `logKnownPii` 属性が `true`に設定されていても、machine.config ファイルの `machineSettings` 要素内の `enableLoggingKnownPii` 属性が `false`に設定されている場合に発生します。 例外をスローすることはありません。  
  
 これらのイベントは、Windows に付属するイベント ビューアー ツールを使用して表示できます。 詳細については、「[イベントログ](./event-logging/index.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [メッセージ ログ](message-logging.md)
- [トレースに関するセキュリティの考慮事項と役立つヒント](./tracing/security-concerns-and-useful-tips-for-tracing.md)
