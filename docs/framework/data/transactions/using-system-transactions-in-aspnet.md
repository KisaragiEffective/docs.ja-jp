---
title: ASP.NET での System.Transactions の使用
ms.date: 03/30/2017
ms.assetid: 1982c300-7ea6-4242-95ed-dc28ccfacac9
ms.openlocfilehash: 2bddebc13897408839e18f42b17a9fbaefdc5b87
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73040410"
---
# <a name="using-systemtransactions-in-aspnet"></a>ASP.NET での System.Transactions の使用
ここでは、ASP.NET アプリケーション内で <xref:System.Transactions> を正しく使用する方法について説明します。

## <a name="enable-distributedtransactionpermission-in-aspnet"></a>ASP.NET での DistributedTransactionPermission の有効化
 <xref:System.Transactions> は部分的に信頼された呼び出し元をサポートし、`AllowPartiallyTrustedCallers` 属性 (APTCA) でマークされます。 <xref:System.Transactions> の信頼レベルは、<xref:System.Transactions> 公開しているリソースの種類 (システムメモリ、共有プロセス全体のリソース、システム全体のリソース、その他のリソースなど) に基づいて定義され、それらにアクセスするために必要な信頼レベルに基づいて定義されます。参考. 部分的に信頼された環境では、 <xref:System.Transactions.DistributedTransactionPermission>が付与されていない限り、完全に信頼されていないアセンブリは、アプリケーション ドメイン内でのみトランザクションを使用できます (この場合、保護されている唯一のリソースはシステム メモリです)。

 Microsoft 分散トランザクション コーディネーター (MSDTC) で管理されるようトランザクション管理がエスカレートされるたびに、<xref:System.Transactions.DistributedTransactionPermission> が要求されます。 この種のシナリオでは、プロセス全体のリソースと特にグローバルなリソースを使用します。グローバルなリソースは、MSDTC ログで予約されたスペースです。 この使用方法の例として、データベース、または供給するサービスの一部としてデータベースを使用するアプリケーションへの Web フロントエンドがあります。

 ASP.NET は、独自の信頼レベル セットを持ち、ポリシー ファイルを介して特定のアクセス許可セットをこれらの信頼レベルに関連付けます。 詳細については、「 [ASP.NET Trust Levels And Policy Files](https://docs.microsoft.com/previous-versions/aspnet/wyts434y(v=vs.100))」を参照してください。 Windows SDK を最初にインストールしたときに、既定の ASP.NET ポリシーファイルは、<xref:System.Transactions.DistributedTransactionPermission>に関連付けられていません。 そのため、ASP.NET アプリケーションのトランザクションが MSDTC によって管理されるようエスカレートされると、<xref:System.Security.SecurityException><xref:System.Transactions.DistributedTransactionPermission>を要求した時点で、 によりエスカレーションが失敗します。 部分的に信頼された ASP.NET の環境でトランザクションのエスカレーションを有効にするには、<xref:System.Transactions.DistributedTransactionPermission><xref:System.Data.SqlClient.SqlClientPermission>と同じ既定の信頼レベルで、 を付与する必要があります。 これをサポートする独自のカスタム信頼レベルとポリシー ファイルを構成するか、既定のポリシー ファイルである **Web_hightrust.config** および **Web_mediumtrust.config**を変更できます。

 ポリシーファイルを変更するには、`DistributedTransactionPermission` の `SecurityClass` 要素を `PolicyLevel` 要素の下の `SecurityClasses` 要素に追加し、ASP.NET `IPermission` の下に、対応する `NamedPermissionSet` 要素を追加します。 次の構成ファイルに、この例を示します。

```xml
<SecurityClasses>
   <SecurityClass Name="DistributedTransactionPermission" Description="System.Transactions.DistributedTransactionPermission, System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
...
</SecurityClasses>

<PermissionSet
  class="NamedPermissionSet"
  version="1"
  Name="ASP.Net">
     <IPermission
        class="System.Transactions.DistributedTransactionPermission, System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
        version="1"
        Unrestricted="true"
     />
...
</PermissionSet>
```

 ASP.NET セキュリティポリシーの詳細については、「 [Ws-securitypolicy 要素 (ASP.NET Settings Schema)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/zhs35b56(v=vs.100))」を参照してください。

## <a name="dynamic-compilation"></a>動的コンパイル
 アクセス時に動的にコンパイルされる ASP.NET アプリケーションで <xref:System.Transactions> をインポートして使用する場合、構成ファイルに <xref:System.Transactions> アセンブリへの参照を配置する必要があります。 具体的には、参照は、既定**のルートの web.config 構成**ファイルの `compilation/assemblies` セクション、または特定の web アプリケーションの構成ファイルの下に追加する必要があります。 次に例を示します。

```xml
<configuration>
   <system.web>
      <compilation>
         <assemblies>
      <add assembly="System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
         </assemblies>
      </compilation>
   </system.web>
</configuration>
```

 詳細については、「[コンパイルのためのアセンブリの追加要素 (ASP.NET 設定スキーマ)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/37e2zyhb(v=vs.100))」を参照してください。

## <a name="see-also"></a>関連項目

- [ASP.NET の信頼レベルとポリシーファイル](https://docs.microsoft.com/previous-versions/aspnet/wyts434y(v=vs.100))
- [Ws-securitypolicy 要素 (ASP.NET Settings スキーマ)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/zhs35b56(v=vs.100))
- [トランザクション管理のエスカレーション](transaction-management-escalation.md)
