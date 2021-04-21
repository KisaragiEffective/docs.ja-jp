---
description: '詳細情報: プロパティと引数'
title: プロパティと引数
ms.date: 03/30/2017
ms.assetid: 14651389-4a49-4cbb-9ddf-c83fdc155df1
ms.openlocfilehash: 34d299889bbe174a236bf9d59b5add932f2e030b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99777607"
---
# <a name="properties-vs-arguments"></a>プロパティと引数

データをアクティビティに渡すために使用可能なオプションはいくつかあります。 <xref:System.Activities.InArgument> を使用する以外に、標準の CLR プロパティまたはパブリック <xref:System.Activities.ActivityAction> プロパティを使用してデータを受け取るアクティビティを開発できます。 このトピックでは適切なメソッド型の選択方法について説明します。  
  
## <a name="using-clr-properties"></a>CLR プロパティの使用  

 データをアクティビティに渡す場合、CLR プロパティ (Get および Set ルーチンを使用してデータを公開するパブリック メソッド) は最も制限が大きいオプションです。 ソリューションをコンパイルする際、CLR プロパティに渡すパラメーターの値がわかっている必要があります。この値はワークフローのすべてのインスタンスで同一です。 このように、CLR プロパティに渡された値はコードで定義された定数に似ています。この値はアクティビティの有効期間を通じて変更されず、アクティビティの異なるインスタンスのために変更できません。 <xref:System.Activities.Expressions.InvokeMethod%601.MethodName%2A> はアクティビティによって公開される CLR プロパティの 1 つです。アクティビティが呼び出すメソッドの名前は、実行時の要件に基づいて変更できず、アクティビティのすべてのインスタンスで同じです。  
  
## <a name="using-arguments"></a>引数の使用  

 引数は、データがアクティビティの有効期間内に 1 回だけ評価されるときに使用する必要があります。つまり、値はアクティビティの有効期間中に変化しません。しかし値はアクティビティの異なるインスタンスごとに異なります。 <xref:System.Activities.Statements.If.Condition%2A> は 1 回評価された値の例です。そのため引数として定義されます。 <xref:System.Activities.Statements.WriteLine.Text%2A> は引数として定義すべきメソッドの別の例です。これはアクティビティの実行中に 1 回のみ評価されますが、アクティビティの異なるインスタンスごとに異なることができます。  
  
## <a name="using-activityaction"></a>ActivityAction の使用  

 アクティビティの実行の有効期間中にデータを複数回評価する必要がある場合は <xref:System.Activities.ActivityAction> を使用してください。 たとえば、<xref:System.Activities.Statements.While.Condition%2A> プロパティは <xref:System.Activities.Statements.While> ループを反復するたびに評価されます。 この目的で <xref:System.Activities.InArgument> を使用した場合、引数は反復のたびに再評価されず、常に同じ結果を返すため、ループは終了しません。
