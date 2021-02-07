---
description: 詳細については、クエリのサポートに関するページを参照してください。
title: クエリのサポート
ms.date: 03/30/2017
ms.assetid: 093c22f5-3294-4642-857a-5252233d6796
ms.openlocfilehash: 418e511e8b845653b9f3ec86d90c6cbfb25d5671
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99755214"
---
# <a name="support-for-queries"></a>クエリのサポート

SQL Workflow Instance Store は、一連の既知のプロパティをストアに記録します。 ユーザーは、これらのプロパティに基づいてインスタンスのクエリを実行できます。 これらの既知のプロパティの一部を次の一覧に示します。  
  
- **サイト名。** サービスが存在する Web サイトの名前。  
  
- **相対アプリケーション パス:** Web サイトを基準としたアプリケーションの相対パス。  
  
- **相対サービス パス:** アプリケーションを基準としたサービスの相対パス。  
  
- **サービス名。** サービスの名前。  
  
- **サービスの名前空間。** サービスが使用する名前空間の名称。  
  
- **現在のコンピューター:**  
  
- **最後のコンピューター**。 ワークフロー サービスのインスタンスが最後に実行されたコンピューター。  
  
> [!NOTE]
> ワークフロー サービス ホストを使用する自己ホスト型のシナリオでは、最後の 4 つのプロパティにのみ値が指定されます。 ワークフロー アプリケーション シナリオでは、最後のプロパティにのみ値が指定されます。  
  
 ワークフロー ランタイムは、最初の 3 つのプロパティの値を指定します。 ワークフローサービスホストは、 **Suspend Reason** プロパティの値を提供します。 SQL Workflow Instance Store 自体は、 **最後に更新されたコンピューター** プロパティの値を提供します。  
  
 また、SQL Workflow Instance Store の機能により、永続性データベースで値を格納したり、クエリで使用したりするカスタム プロパティを指定することができます。 カスタムプロモーションの詳細については、「 [ストアの拡張機能](store-extensibility.md)」を参照してください。  
  
## <a name="views"></a>ビュー  

 インスタンス ストアには、次のビューがあります。 詳細については、「 [永続性データベーススキーマ](persistence-database-schema.md) 」を参照してください。  
  
### <a name="the-instances-view"></a>Instances ビュー  

 Instances ビューには、次のフィールドがあります。  
  
1. **Id**  
  
2. **PendingTimer**  
  
3. **CreationTime**  
  
4. **LastUpdatedTime**  
  
5. **ServiceDeploymentId**  
  
6. **SuspensionExceptionName**  
  
7. **SuspensionReason**  
  
8. **ActiveBookmarks**  
  
9. **CurrentMachine**  
  
10. **LastMachine**  
  
11. **ExecutionStatus**  
  
12. **IsInitialized**  
  
13. **IsSuspended**  
  
14. **IsCompleted**  
  
15. **EncodingOption**  
  
16. **ReadWritePrimitiveDataProperties**  
  
17. **WriteOnlyPrimitiveDataProperties**  
  
18. **ReadWriteComplexDataProperties**  
  
19. **WriteOnlyComplexDataProperties**  
  
### <a name="the-servicedeployments-view"></a>ServiceDeployments ビュー  

 ServiceDeployments ビューには、次のフィールドがあります。  
  
1. **SiteName**  
  
2. **RelativeServicePath**  
  
3. **RelativeApplicationPath**  
  
4. **ServiceName**  
  
5. **ServiceNamespace**  
  
### <a name="the-instancepromotedproperties-view"></a>InstancePromotedProperties ビュー  

 InstancePromotedProperties ビューには、次のフィールドがあります。 昇格させたプロパティの詳細については、 [ストアの機能拡張](store-extensibility.md) に関するトピックを参照してください。  
  
1. **InstanceId**  
  
2. **EncodingOption**  
  
3. **PromotionName**  
  
4. **値 #** ( **Value1** から **Value64** までのフィールドの範囲)。
