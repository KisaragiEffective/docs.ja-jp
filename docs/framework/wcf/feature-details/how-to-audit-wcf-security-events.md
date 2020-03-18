---
title: '方法 : Windows Communication Foundation セキュリティ イベントを監査する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], auditing events
ms.assetid: e71e9587-3336-46a2-9a9e-d72a1743ecec
ms.openlocfilehash: 7071aaf88346ee217226632501ebd6c82cfc1cb8
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346755"
---
# <a name="how-to-audit-windows-communication-foundation-security-events"></a>方法 : Windows Communication Foundation セキュリティ イベントを監査する
Windows Communication Foundation (WCF) を使用すると、windows イベントビューアーを使用して表示できるセキュリティイベントを Windows イベントログに記録できます。 このトピックでは、セキュリティ イベントをログ出力するようにアプリケーションを設定する方法について説明します。 WCF 監査の詳細については、「[監査](../../../../docs/framework/wcf/feature-details/auditing-security-events.md)」を参照してください。  
  
### <a name="to-audit-security-events-in-code"></a>セキュリティ イベントを監査するコードを記述するには  
  
1. 監査ログの場所を指定します。 それには、次のコードに示すように、<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.AuditLogLocation%2A> クラスの <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> プロパティに <xref:System.ServiceModel.AuditLogLocation> 列挙体のいずれかの値を設定します。  
  
     [!code-csharp[AuditingSecurityEvents#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#2)]
     [!code-vb[AuditingSecurityEvents#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#2)]  
  
     <xref:System.ServiceModel.AuditLogLocation> 列挙体には、`Application`、`Security`、`Default`の3つの値があります。 これは、イベント ビューアーを開いたとき、セキュリティ ログとアプリケーション ログのどちらが表示されるかを表します。 `Default` を指定した場合の動作は、アプリケーションが稼働するオペレーティング システムに依存します。 ログの場所を指定せずに監査機能を有効にした場合、セキュリティ ログに書き込み可能なプラットフォームであれば `Security` ログに、そうでなければ `Application` ログに出力するようになります。 既定では、セキュリティログへの書き込みがサポートされているのは、Windows Server 2003 と Windows Vista だけです。  
  
2. イベントの種類を監査対象として設定します。 サービス レベルのイベントとメッセージ レベルの承認イベントを同時に監査できます。 それには、次のコードに示すように、<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.ServiceAuthorizationAuditLevel%2A> プロパティまたは <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.MessageAuthenticationAuditLevel%2A> プロパティに <xref:System.ServiceModel.AuditLevel> 列挙体のいずれかの値を設定します。  
  
     [!code-csharp[AuditingSecurityEvents#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#3)]
     [!code-vb[AuditingSecurityEvents#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#3)]  
  
3. ログ監査イベントに関して、単に無視してアプリケーションの処理を続行するか、それとも失敗を通知するかを設定します。 次のコードに示すように、<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> プロパティに `true` または `false` を設定します。  
  
     [!code-csharp[AuditingSecurityEvents#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#4)]
     [!code-vb[AuditingSecurityEvents#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#4)]  
  
     `SuppressAuditFailure` プロパティの既定値は `true` なので、監査に失敗してもアプリケーションには影響しません。 それ以外の場合は、例外がスローされます。 正常な監査に関するログは、Verbose レベルで出力されます。 異常発生時には、Error レベルでトレースが出力されます。  
  
4. 既存の <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> を、<xref:System.ServiceModel.ServiceHost> の説明にある動作のコレクションから削除します。 動作のコレクションは、<xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> プロパティからアクセスできます。また、<xref:System.ServiceModel.ServiceHostBase.Description%2A> プロパティからもアクセスできます。 その後、次のコードに示すように、新しい <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> を同じコレクションに追加します。  
  
     [!code-csharp[AuditingSecurityEvents#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#5)]
     [!code-vb[AuditingSecurityEvents#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#5)]  
  
### <a name="to-set-up-auditing-in-configuration"></a>構成ファイルで監査を設定するには  
  
1. 構成で監査を設定するには、web.config ファイルの[\<の動作 >](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)セクションに[\<動作 >](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)要素を追加します。 次に、次の例に示すように、 [\<serviceSecurityAudit >](../../../../docs/framework/configure-apps/file-schema/wcf/servicesecurityaudit.md)要素を追加し、さまざまな属性を設定します。  
  
    ```xml  
    <behaviors>  
       <behavior name="myAuditBehavior">  
          <serviceSecurityAudit auditLogLocation="Application"  
                suppressAuditFailure="false"   
                serviceAuthorizationAuditLevel="None"   
                messageAuthenticationAuditLevel="SuccessOrFailure" />  
          </behavior>  
    </behaviors>  
    ```  
  
2. 次の例に示すように、サービスに対して動作を指定する必要があります。  
  
    ```xml  
    <services>  
        <service type="WCS.Samples.Service.Echo"   
        behaviorConfiguration=" myAuditBehavior">  
           <endpoint address=""  
                    binding="wsHttpBinding"  
                    bindingConfiguration="CertificateDefault"   
                    contract="WCS.Samples.Service.IEcho" />  
        </service>  
    </services>  
    ```  
  
## <a name="example"></a>使用例  
 <xref:System.ServiceModel.ServiceHost> クラスのインスタンスを作成し、新しい <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> をその動作のコレクションに追加するコード例を次に示します。  
  
 [!code-csharp[AuditingSecurityEvents#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#1)]
 [!code-vb[AuditingSecurityEvents#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#1)]  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> プロパティを `true` に設定すると、セキュリティ監査を生成する失敗が抑制されます (`false` に設定した場合は、例外がスローされます)。 ただし、次の Windows**ローカルセキュリティ設定**プロパティを有効にすると、監査イベントの生成に失敗すると、windows が直ちにシャットダウンします。  
  
 **監査: セキュリティ監査をログに記録できない場合は、直ちにシステムをシャットダウンします。**  
  
 プロパティを設定するには、 **[ローカルセキュリティの設定]** ダイアログボックスを開きます。 **[セキュリティの設定]** で、 **[ローカルポリシー]** をクリックします。 次に、 **[セキュリティオプション]** をクリックします。  
  
 <xref:System.ServiceModel.AuditLogLocation> プロパティが <xref:System.ServiceModel.AuditLogLocation.Security> に設定されていて、 **[オブジェクトアクセスの監査]** が**ローカルセキュリティポリシー**で設定されていない場合、監査イベントはセキュリティログに書き込まれません。 エラーが返らない場合でも、監査エントリはセキュリティ ログに書き込まれません。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.AuditLogLocation%2A>
- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- <xref:System.ServiceModel.AuditLogLocation>
- [監査](../../../../docs/framework/wcf/feature-details/auditing-security-events.md)
