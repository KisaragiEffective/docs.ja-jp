---
title: ワークフローのセキュリティ
ms.date: 03/30/2017
helpviewer_keywords:
- programming [WF], workflow security
ms.assetid: d712a566-f435-44c0-b8c0-49298e84b114
ms.openlocfilehash: 36d03a2fca8f143b98338050fc9da4490960bda9
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837520"
---
# <a name="workflow-security"></a>ワークフローのセキュリティ
Windows Workflow Foundation (WF) は、Microsoft SQL Server や Windows Communication Foundation (WCF) など、いくつかの異なるテクノロジと統合されています。 これらのテクノロジと相互作用するうえで、不適切に実行された場合にワークフローでセキュリティの問題が発生することがあります。

## <a name="persistence-security-concerns"></a>永続化のセキュリティに関する注意事項

1. <xref:System.Activities.Statements.Delay> アクティビティと永続化を使用するワークフローは、サービスによって再アクティブ化する必要があります。 Windows AppFabric はワークフロー管理サービス (WMS) を使用して、タイマーが期限切れのワークフローを再アクティブ化します。 WMS は <xref:System.ServiceModel.WorkflowServiceHost> を作成して、再アクティブ化されたワークフローをホストします。 WMS サービスが停止した場合、永続化されたワークフローはタイマーが時間切れになっていると再アクティブ化されません。

2. 永続的なインスタンスへのアクセスは、アプリケーション ドメイン外の安全でないエンティティから保護する必要があります。 また、開発者は、悪意のあるコードが永続性インスタンス化コードと同じアプリケーション ドメインで実行できないようにする必要があります。

3. 永続性インスタンス化は、高い (管理者の) アクセス許可で実行しないでください。

4. アプリケーション ドメインの外部で処理されるデータは、保護する必要があります。

5. セキュリティの分離を必要とするアプリケーションは、スキーマ抽象化と同じインスタンスを共有しないようにします。 このようなアプリケーションは、異なるストア プロバイダーを使用するか、別のストアのインスタンス化を使用するように構成されたストア プロバイダーを使用する必要があります。

## <a name="sql-server-security-concerns"></a>SQL サーバーのセキュリティに関する注意事項

- 子アクティビティ、場所、ブックマーク、ホストの拡張機能、またはスコープを多数使用する場合、またはペイロードが非常に大きいブックマークを使用する場合、メモリが不足したり、永続化の実行中に必要以上のディスク容量が割り当てられる可能性があります。 オブジェクト レベルおよびデータベース レベルのセキュリティを使用すると、このような状態を緩和できます。

- <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> を使用する場合、インスタンス ストアをセキュリティで保護する必要があります。

- インスタンス ストアの機密情報は暗号化する必要があります。 詳細については、「 [SQL Server の暗号化](/sql/relational-databases/security/encryption/sql-server-encryption)」を参照してください。

- 多くの場合、データベース接続文字列は構成ファイルに含まれているので、Windows レベルのセキュリティ (ACL) を使用して、構成ファイル (通常は Web.Config) が安全であるように、またログインとパスワードの情報が接続文字列に含まれないようにする必要があります。 Windows 認証をデータベースと Web サーバー間で代わりに使用する必要があります。

## <a name="considerations-for-workflowservicehost"></a>WorkflowServiceHost に関する考慮事項

- ワークフローで使用される Windows Communication Foundation (WCF) エンドポイントはセキュリティで保護する必要があります。 詳細については、「 [WCF セキュリティの概要](../wcf/feature-details/security-overview.md)」を参照してください。

- <xref:System.ServiceModel.ServiceAuthorizationManager> を使用して、ホスト レベルの認証を実装できます。 詳細については、「[方法: サービスのカスタム承認マネージャーを作成](../wcf/extending/how-to-create-a-custom-authorization-manager-for-a-service.md)する」を参照してください。

- 受信メッセージの ServiceSecurityContext は、OperationContext へのアクセスによって、ワーク フロー内からも使用できます。

## <a name="wf-security-pack-ctp"></a>WF Security Pack CTP
 Microsoft WF Security Pack CTP 1 は、 [.NET Framework 4](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/w0x726c2(v=vs.100)) (WF 4) および[Windows IDENTITY Foundation (WIF)](../security/index.md)の[Windows Workflow Foundation](index.md)に基づく一連のアクティビティとその実装の最初の community テクノロジプレビュー (ctp) リリースです。  Microsoft WF Security Pack CTP 1 には、アクティビティおよび、ワークフローを使用してさまざまなセキュリティ関連のシナリオを簡単に有効にする方法を示すそのデザイナーの両方が含まれています。これらには以下のものが含まれます。

1. ワークフローのクライアント ID の偽装

2. PrincipalPermission やクレームの検証などの内部ワークフローの認証

3. ユーザー名/パスワードやセキュリティ トークン サービス (STS) から取得するトークンなど、ワークフローで指定される ClientCredentials を使用した認証されたメッセージ

4. WS-Trust ActAs を使用して、クライアント セキュリティ トークンをバックエンド サービス (クレーム ベースの委任) にフロー

WF Security Pack CTP の詳細とダウンロードについては、「 [Wf Security pack ctp (Wf セキュリティパック ctp](https://archive.codeplex.com/?p=wf) )」を参照してください。
