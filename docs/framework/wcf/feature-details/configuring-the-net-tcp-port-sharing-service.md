---
title: Net.TCP ポート共有サービスを構成する
ms.date: 03/30/2017
ms.assetid: b6dd81fa-68b7-4e1b-868e-88e5901b7ea0
ms.openlocfilehash: 2ff622dc97e63bd0ee10f00c7515692be8df09a1
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837455"
---
# <a name="configuring-the-nettcp-port-sharing-service"></a>Net.TCP ポート共有サービスを構成する
Net.TCP トランスポートを使用する自己ホスト型サービスは、`ListenBacklog` や `MaxPendingAccepts` など、いくつかの高度な設定を制御できます。これらは、ネットワーク通信で使用される、ベースである TCP ソケットの動作をコンロトールします。 ただし、各ソケットに対するこれらの設定は、トランスポート バインディングでポート共有が無効化されている場合 (既定では有効) に、バインディング レベルでのみ適用されます。  
  
 net.tcp バインディングは、(トランスポート バインド要素で `portSharingEnabled =true` を設定することで) ポート共有が有効化されている場合、外部プロセス (Net.TCP ポート共有サービスをホストする SMSvcHost.exe) に対し、自身の代わりに TCP ソケットを管理することを暗黙的に許可しています。 たとえば、TCP を使用している場合は次のように指定します。  
  
```xml  
<tcpTransport portSharingEnabled="true"  />  
```  
  
 この方法で設定すると、サービスのトランスポート バインド要素に指定したソケット設定は無視され、SMSvcHost.exe で指定されたソケット設定が採用されます。  
  
 SMSvcHost.exe を構成するには、SmSvcHost.exe.config という XML 構成ファイルを作成し、SMSvcHost.exe 実行可能ファイルと同じ物理ディレクトリ (たとえば、C:\Windows\Microsoft.NET\Framework\v4.5) に配置します。  
  
 次の例では、すべての構成可能値の既定値を明示的に指定した SMSvcHost.exe.config のサンプルを示します。  
  
```xml  
<configuration>  
   <system.serviceModel.activation>  
       <net.tcp listenBacklog="16" <!--16 * # of processors -->  
          maxPendingAccepts="4"<!-- 4 * # of processors -->  
          maxPendingConnections="100"  
          receiveTimeout="00:00:30" <!--30 seconds -->  
          teredoEnabled="false">  
          <allowAccounts>  
             <!-- LocalSystem account -->  
             <add securityIdentifier="S-1-5-18"/>  
             <!-- LocalService account -->  
             <add securityIdentifier="S-1-5-19"/>  
             <!-- Administrators account -->  
             <add securityIdentifier="S-1-5-20"/>  
             <!-- Network Service account -->  
             <add securityIdentifier="S-1-5-32-544" />  
             <!-- IIS_IUSRS account (Vista only) -->  
             <add securityIdentifier="S-1-5-32-568"/>  
           </allowAccounts>  
       </net.tcp>  
</configuration>  
```  
  
## <a name="when-to-modify-smsvchostexeconfig"></a>SMSvcHost.exe.config を変更する場合  
 一般的に、SMSvcHost.exe.config ファイルの内容を変更する際には、このファイルに指定するすべての構成設定は、Net.TCP ポート共有サービスを使用するコンピューターのすべてのサービスに影響するため、注意が必要です。 これには、windows プロセスアクティブ化サービス (WAS) の TCP アクティブ化機能を使用する Windows Vista のアプリケーションが含まれます。  
  
 ただし、Net.TCP ポート共有サービスの既定の構成を変更する必要のある状況もあります。 たとえば、`maxPendingAccepts` の既定値は 4 * プロセッサ数です。 ポート共有を使用する多数のサービスをホストするサーバーでは、この値を大きくして、最大のスループットを実現することができます。 `maxPendingConnections` の既定値は 100 です。 サービスを呼び出すクライアントが同時に複数存在し、サービスがクライアント接続を切断する場合も、この値を大きくすることを検討してください。  
  
 SMSvcHost.exe.config には、ポート共有サービスを使用する可能性のあるプロセス ID に関する情報も含まれています。 プロセスが、共有 TCP ポートを使用するためにポート共有サービスに接続すると、そのプロセスのプロセス ID が、ポート共有サービスの使用が許可されている ID のリストに照合されます。 これらの id は、SMSvcHost の .config ファイルの \<allowAccounts > セクションにセキュリティ識別子 (Sid) として指定されます。 既定では、ポート共有サービスへのアクセス許可は、システム アカウント (LocalService、LocalSystem、NetworkService) や管理者グループのメンバーに与えられます。 ポート共有サービスに接続するために別の ID (ユーザー ID など) でプロセスが実行されることを許可しているアプリケーションでは、適切な SID を SMSvcHost.exe.config に明示的に追加する必要があります (これらの変更は SMSvc.exe プロセスが再起動されるまで適用されません)。  
  
> [!NOTE]
> ユーザーアカウント制御 (UAC) が有効になっている Windows Vista システムでは、ローカルユーザーのアカウントが Administrators グループのメンバーであっても、昇格されたアクセス許可が必要です。 これらのユーザーが昇格せずにポート共有サービスを使用できるようにするには、ユーザーの SID (または、ユーザーがメンバーとなっているグループの SID) を SMSvcHost の \<allowAccounts > セクションに明示的に追加する必要があります。  
  
> [!WARNING]
> 既定の SMSvcHost.exe.config ファイルは、カスタムの `etwProviderId` を指定することによって、SMSvcHost.exe のトレースの影響がサービス トレースに及ぶのを防ぎます。  
  
## <a name="see-also"></a>参照

- [\<net.tcp>](../../../../docs/framework/configure-apps/file-schema/wcf/net-tcp.md)
