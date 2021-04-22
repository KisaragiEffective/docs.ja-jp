---
title: Flowchart のワークフロー
description: この記事では、通常、Workflow Foundation においてシーケンシャルでないワークフローを実装するために使用されるフローチャート アクティビティについて説明します。
ms.date: 03/30/2017
ms.assetid: b0a3475c-d22f-49eb-8912-973c960aebf5
ms.openlocfilehash: ce0661653a1d50b3f7264246b810faabbd12bf5f
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2020
ms.locfileid: "83419916"
---
# <a name="flowchart-workflows"></a>Flowchart のワークフロー

フローチャートは、プログラムを診断するための既知のパラダイムです。 通常、Flowchart アクティビティはシーケンシャル以外のワークフローを実装するために使用されますが、`FlowDecision` ノードが使用されない場合は、シーケンシャル ワークフローにも使用できます。

## <a name="flowchart-workflow-structure"></a>フローチャート ワークフローの構造

 Flowchart アクティビティは、実行されるアクティビティのコレクションを含むアクティビティです。  フローチャートには、変数の値に基づいて含まれているアクティビティ間の実行を指示する、<xref:System.Activities.Statements.FlowDecision> や <xref:System.Activities.Statements.FlowSwitch%601> などのフロー制御要素も含まれています。

## <a name="types-of-flow-nodes"></a>フロー ノードの種類

 要素の実行時に必要なフロー制御の種類に応じて、さまざまな種類の要素が使用されます。 フローチャート要素には次のような種類があります。

- `FlowStep` - フローチャートの実行の 1 ステップをモデル化します。

- `FlowDecision` - <xref:System.Activities.Statements.If> と同様に、ブール条件に基づいて実行を分岐します。

- `FlowSwitch` - <xref:System.Activities.Statements.Switch%601> と同様に、排他的スイッチに基づいて実行を分岐します。

各リンクには、子アクティビティの実行に使用できる `Action` を定義した <xref:System.Activities.ActivityAction> プロパティと、現在の要素の実行が完了したときに実行する 1 つ以上の要素を定義した 1 つ以上の `Next` プロパティがあります。

### <a name="creating-a-basic-activity-sequence-with-a-flowstep-node"></a>FlowStep ノードによる基本的なアクティビティ シーケンスの作成

2 つのアクティビティを連続して実行する基本的なシーケンスをモデル化するには、`FlowStep` 要素を使用します。 次の例では、2 つの `FlowStep` 要素を使用して 2 つのアクティビティを連続して実行しています。

```xml
<Flowchart>
  <FlowStep>
    <Assign DisplayName="Get Name">
      <Assign.To>
        <OutArgument x:TypeArguments="x:String">[result]</OutArgument>
      </Assign.To>
      <Assign.Value>
        <InArgument x:TypeArguments="x:String">["User"]</InArgument>
      </Assign.Value>
    </Assign>
    <FlowStep.Next>
      <FlowStep>
        <WriteLine Text="Hello, " & [result]/>
      </FlowStep>
    </FlowStep.Next>
  </FlowStep>
</Flowchart>
```

### <a name="creating-a-conditional-flowchart-with-a-flowdecision-node"></a>FlowDecision ノードによる条件付きフローチャートの作成

フローチャート ワークフローの条件付きフロー ノードをモデル化するには (つまり、従来のフローチャートの決定シンボルとして機能するリンクを作成するには)、<xref:System.Activities.Statements.FlowDecision> ノードを使用します。 ノードの <xref:System.Activities.Statements.FlowDecision.Condition%2A> プロパティは、条件を定義する式に設定されます。また、<xref:System.Activities.Statements.FlowDecision.True%2A> プロパティと <xref:System.Activities.Statements.FlowDecision.False%2A> プロパティは、式が <xref:System.Activities.Statements.FlowNode> または `true` に評価された場合に実行される `false` インスタンスに設定されます。 次の例では、<xref:System.Activities.Statements.FlowDecision> ノードを使用するワークフローを定義する方法を示します。

```xml
<Flowchart>
  <FlowStep>
    <Read Result="[s]"/>
    <FlowStep.Next>
      <FlowDecision>
        <IsEmpty Input="[s]" />
        <FlowDecision.True>
          <FlowStep>
            <Write Text="Empty"/>
          </FlowStep>
        </FlowDecision.True>
        <FlowDecision.False>
          <FlowStep>
            <Write Text="Non-Empty"/>
          </FlowStep>
        </FlowDecision.False>
      </FlowDecision>
    </FlowStep.Next>
  </FlowStep>
</Flowchart>
```

### <a name="creating-an-exclusive-switch-with-a-flowswitch-node"></a>FlowSwitch ノードによる排他的スイッチの作成

一致する値に基づいて 1 つの排他的パスを選択するフローチャートをモデル化するには、<xref:System.Activities.Statements.FlowSwitch%601> ノードを使用します。 <xref:System.Activities.Statements.FlowSwitch%601.Expression%2A> プロパティは、選択に対して一致する値を定義する <xref:System.Activities.Activity%601> の型パラメーターを持つ <xref:System.Object> に設定されます。 <xref:System.Activities.Statements.FlowSwitch%601.Cases%2A> プロパティは、条件式に対して一致する、キーと <xref:System.Activities.Statements.FlowNode> オブジェクトのディクショナリと、特定のケースが条件式に一致する場合のフローの実行方法を定義する <xref:System.Activities.Statements.FlowNode> オブジェクトのセットを定義します。 また、<xref:System.Activities.Statements.FlowSwitch%601> は、条件式に一致するケースがない場合の実行フローを定義する <xref:System.Activities.Statements.FlowSwitch%601.Default%2A> プロパティも定義します。 次のコードは、<xref:System.Activities.Statements.FlowSwitch%601> 要素を使用するワークフローを定義する方法の例です。

```xml
<Flowchart>
  <FlowSwitch>
    <FlowStep x:Key="Red">
      <WriteRed/>
    </FlowStep>
    <FlowStep x:Key="Blue">
      <WriteBlue/>
    </FlowStep>
    <FlowStep x:Key="Green">
      <WriteGreen/>
    </FlowStep>
  </FlowSwitch>
</Flowchart>
```
