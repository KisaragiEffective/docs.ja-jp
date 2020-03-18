---
title: 変数と引数の追跡
ms.date: 03/30/2017
ms.assetid: 8f3d9d30-d899-49aa-b7ce-a8d0d32c4ff0
ms.openlocfilehash: c5d3fe6626c22184edd83de6aedad8589ab2ef35
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837546"
---
# <a name="variable-and-argument-tracking"></a>変数と引数の追跡
ワークフローの実行を追跡するときは、データを抽出すると便利です。 これにより、実行後に追跡レコードにアクセスするときにコンテキストが追加されます。 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] では、追跡を使用して、ワークフローのアクティビティのスコープ内の参照可能な変数や引数を抽出できます。 プロファイルを追跡すると、データを簡単に抽出できるようになります。  
  
## <a name="variables-and-arguments"></a>変数と引数  
 変数および引数が抽出されるのは、アクティビティが ActivityStateRecord を生成したときです。  変数は、アクティビティのスコープ内にある場合にのみ抽出の対象となります。 アクティビティ内に抽出する変数は、次のように指定します。  
  
- 変数名によって変数が指定されている場合、追跡されている現在のアクティビティと親アクティビティ内の変数が追跡で検索されます。 変数は、現在のアクティビティのスコープと親スコープ内で検索されます。  
  
- 抽出する変数が name = "\*" を使用して指定されている場合、追跡されている現在のアクティビティ内のすべての変数が抽出されます。 この場合、スコープ内の変数でも、親アクティビティで定義されているものは抽出されません。  
  
 引数の抽出時、抽出される引数はアクティビティの状態によって異なります。 アクティビティの状態が Executing である場合、`InArguments` のみが抽出に使用できます。 他のアクティビティ状態 (Closed、Faulted、Canceled) については、すべての引数、InArguments と OutArguments の両方が抽出に使用できます。  
  
 次の例は、アクティビティの `Closed` 追跡レコードが生成されたときに変数と引数を抽出するアクティビティ状態クエリを示しています。 変数と引数は、<xref:System.Activities.Tracking.ActivityStateRecord> でのみ抽出できるため、<xref:System.Activities.Tracking.ActivityStateQuery> を使用して追跡プロファイル内で定期受信されます。  
  
```xml  
<activityStateQuery activityName="SendEmailActivity">  
  <states>  
    <state name="Closed"/>  
  </states>  
  <variables>  
    <variable name="FromAddress"/>  
  </variables>  
  <arguments>  
    <argument name="Result"/>  
  </arguments>  
</activityStateQuery>  
```  
  
## <a name="protecting-information-stored-within-variables-and-arguments"></a>変数および引数に格納される情報の保護  
 追跡される変数や引数は、既定では WF ランタイムによって参照可能になります。 ワークフローの開発者は、次のような手順を行うことで、追跡対象の変数や引数へのアクセスを防止できます。  
  
1. 変数の値を暗号化します。  
  
2. 追跡プロファイルの作成を管理して、変数や引数の抽出を防止します。  
  
3. カスタムの追跡参加要素については、変数や引数に格納されている機密情報が WF コードによって公開されないようにします。  
  
## <a name="see-also"></a>参照

- [Windows Server App Fabric の監視](https://docs.microsoft.com/previous-versions/appfabric/ee677251(v=azure.10))
- [App Fabric を使用したアプリケーションの監視](https://docs.microsoft.com/previous-versions/appfabric/ee677276(v=azure.10))
