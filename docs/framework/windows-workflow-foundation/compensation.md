---
description: '詳細情報: 補正'
title: 補正
ms.date: 03/30/2017
ms.assetid: 722e9766-48d7-456c-9496-d7c5c8f0fa76
ms.openlocfilehash: d2c57a248939d0485075a5cbf57d91789f58d01a
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99792792"
---
# <a name="compensation"></a>補正

Windows Workflow Foundation (WF) での補正は、それ以降の障害が発生したときに、以前に完了した作業を元に戻したり補正したりするためのメカニズムです (アプリケーションで定義されているロジックに従ってください)。 ここでは、ワークフローで補正を使用する方法について説明します。  
  
## <a name="compensation-vs-transactions"></a>補正とトランザクション  

 トランザクションを使用することで、複数の操作を 1 つの作業単位にまとめることができます。 アプリケーションでトランザクションを使用すると、トランザクションのプロセスでエラーが発生した場合、トランザクション内で実行されたすべての変更を中止 (ロールバック) できます。 ただし、作業が長時間実行されている場合は、トランザクションの使用が適切でないことがあります。 たとえば、旅行計画用アプリケーションはワークフローとして実装されます。 このワークフローの手順は、フライトの予約、マネージャーによる承認の待機、およびチケットの支払いで構成されていることがあります。 このプロセスには数日かかる場合があり、フライトの予約と支払いの手順を同じトランザクションに参加させるのは実用的ではありません。 このようなシナリオでは、処理の後半でエラーが発生した場合、補正を使用してワークフローの予約の手順を取り消すことができます。  
  
> [!NOTE]
> このトピックでは、ワーク フローの補正について説明します。 ワークフローのトランザクションの詳細については、「 [トランザクション](workflow-transactions.md) 」と「」を参照してください <xref:System.Activities.Statements.TransactionScope> 。 トランザクションの詳細については、「」および「」を参照してください <xref:System.Transactions?displayProperty=nameWithType> <xref:System.Transactions.Transaction?displayProperty=nameWithType> 。  
  
## <a name="using-compensableactivity"></a>CompensableActivity の使用  

 <xref:System.Activities.Statements.CompensableActivity> は、[!INCLUDE[wf1](../../../includes/wf1-md.md)] の中心的な補正アクティビティです。 補正が必要な可能性がある作業を実行するアクティビティは、すべて <xref:System.Activities.Statements.CompensableActivity.Body%2A> の <xref:System.Activities.Statements.CompensableActivity> に配置されます。 この例では、航空券を購入する予約手順を <xref:System.Activities.Statements.CompensableActivity.Body%2A> の <xref:System.Activities.Statements.CompensableActivity> に配置し、予約の取り消しを <xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A> に配置します。 ワークフロー内の <xref:System.Activities.Statements.CompensableActivity> の直後には、マネージャーの承認を待ち、航空券の購入手順を完了するという 2 つのアクティビティがあります。 <xref:System.Activities.Statements.CompensableActivity> が正常に完了した後に、エラー条件によってワークフローが取り消された場合、<xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A> ハンドラー内のアクティビティがスケジュールされ、航空券は取り消されます。  
  
 [!code-csharp[CFX_CompensationExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#1)]  
  
 次の例は XAML のワークフローです。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken" />  
    </CompensableActivity.Result>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
    <c:ReserveFlight />  
  </CompensableActivity>  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
</Sequence>  
```  
  
 このワークフローが呼び出されると、次の出力がコンソールに表示されます。  
  
 **ReserveFlight: チケットは予約されています。**  
**Managerapproval: マネージャーの承認を受け取りました。** 
**PurchaseFlight: チケットが購入されます。** 
**ワークフローは正常に完了しました。状態: Closed。**
> [!NOTE]
> `ReserveFlight` などのこのトピックのサンプル アクティビティでは、コンソールにアクティビティの名前や目的が表示されるため、補正が行われるときのアクティビティの順序がわかりやすくなっています。  
  
### <a name="default-workflow-compensation"></a>既定のワークフローの補正  

 既定では、ワークフローを取り消すと、正常に完了済みであるがまだ確認または補正されていない補正可能なアクティビティに対して、補正ロジックが実行されます。  
  
> [!NOTE]
> <xref:System.Activities.Statements.CompensableActivity>が *確認* されると、アクティビティの補正を呼び出すことができなくなります。 確認プロセスについては、このセクションの後半で説明します。  
  
 この例では、航空券の予約後かつマネージャーの承認手順前に例外がスローされます。  
  
 [!code-csharp[CFX_CompensationExample#2](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#2)]  
  
 この例は XAML のワークフローです。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken" />  
    </CompensableActivity.Result>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
    <c:ReserveFlight />  
  </CompensableActivity>  
  <c:SimulatedErrorCondition />  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
</Sequence>  
```  
  
 [!code-csharp[CFX_CompensationExample#100](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#100)]  
  
 ワークフローが呼び出されると、シミュレートされたエラー状態の例外が <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> のホスト アプリケーションで処理されて、ワークフローが取り消され、補正ロジックが呼び出されます。  
  
 **ReserveFlight: チケットは予約されています。**  
**SimulatedErrorCondition: ApplicationException をスローしています。** 
**ワークフローのハンドルされない例外:** 
**ApplicationException: ワークフロー** 
 でのシミュレートされたエラー状態。**Cancelflight: チケットは取り消されました。** 
**ワークフローは正常に完了しました。状態: キャンセル。**

### <a name="cancellation-and-compensableactivity"></a>取り消しと CompensableActivity  

 <xref:System.Activities.Statements.CompensableActivity.Body%2A> の <xref:System.Activities.Statements.CompensableActivity> 内のアクティビティが完了せず、アクティビティが取り消されると、<xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> が実行されます。  
  
> [!NOTE]
> <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> が呼び出されるのは、<xref:System.Activities.Statements.CompensableActivity.Body%2A> の <xref:System.Activities.Statements.CompensableActivity> 内のアクティビティが完了せず、アクティビティが取り消された場合だけです。 <xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A> は、<xref:System.Activities.Statements.CompensableActivity.Body%2A> の <xref:System.Activities.Statements.CompensableActivity> 内のアクティビティが正常に完了し、その後にアクティビティで補正が呼び出された場合にのみ実行されます。  
  
 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> を使用すると、ワークフローの作成者は、適切な取り消しロジックを指定できます。 次の例では、<xref:System.Activities.Statements.CompensableActivity.Body%2A> の実行中に例外がスローされてから、<xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> が呼び出されます。  
  
```csharp  
Activity wf = new Sequence()  
{  
    Activities =  
    {  
        new CompensableActivity  
        {  
            Body = new Sequence  
            {  
                Activities =
                {  
                    new ChargeCreditCard(),  
                    new SimulatedErrorCondition(),  
                    new ReserveFlight()  
                }  
            },  
            CompensationHandler = new CancelFlight(),  
            CancellationHandler = new CancelCreditCard()  
        },  
        new ManagerApproval(),  
        new PurchaseFlight()  
    }  
};  
```  
  
 この例は XAML のワークフローです。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken" />  
    </CompensableActivity.Result>  
    <Sequence>  
      <c:ChargeCreditCard />  
      <c:SimulatedErrorCondition />  
      <c:ReserveFlight />  
    </Sequence>  
    <CompensableActivity.CancellationHandler>  
      <c:CancelCreditCard />  
    </CompensableActivity.CancellationHandler>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
  </CompensableActivity>  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
</Sequence>  
```  
  
 ワークフローが呼び出されると、シミュレートされたエラー状態の例外が、<xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> のホスト アプリケーションで処理されて、ワークフローが取り消され、<xref:System.Activities.Statements.CompensableActivity> の取り消しロジックが呼び出されます。 この例では、補正ロジックと取り消しロジックの目的は異なります <xref:System.Activities.Statements.CompensableActivity.Body%2A> が正常に完了した場合、これはクレジット カードに課金されてフライトが予約されたことを意味するので、補正で両方の手順を取り消す必要があります (この例では、フライトをキャンセルすると、クレジットカードの料金が自動的にキャンセルされます)。ただし、が取り消された場合は、が <xref:System.Activities.Statements.CompensableActivity> 完了していないことを意味し <xref:System.Activities.Statements.CompensableActivity.Body%2A> ます。そのため、のロジックは、 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> キャンセルを適切に処理する方法を決定できる必要があります。 この例では、<xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> はクレジット カードへの課金を取り消しますが、`ReserveFlight` は <xref:System.Activities.Statements.CompensableActivity.Body%2A> の最後のアクティビティであるので、航空券の取り消しを試みません。 `ReserveFlight` は <xref:System.Activities.Statements.CompensableActivity.Body%2A> の最後のアクティビティであるので、そのアクティビティが正常に完了して <xref:System.Activities.Statements.CompensableActivity.Body%2A> が完了すると、取り消しは不可能となります。  
  
 **ChargeCreditCard: 航空券の料金をクレジット カードに請求します。**  
**SimulatedErrorCondition: ApplicationException をスローしています。** 
**ワークフローのハンドルされない例外:** 
**ApplicationException: ワークフロー** 
 でのシミュレートされたエラー状態。**CancelCreditCard: クレジットカードの請求をキャンセルします。** 
**ワークフローは正常に完了しました。状態: キャンセル。**  取り消しの詳細については、「 [キャンセル](modeling-cancellation-behavior-in-workflows.md)」を参照してください。  
  
### <a name="explicit-compensation-using-the-compensate-activity"></a>Compensate アクティビティを使用する明示的な補正  

 前のセクションでは、暗黙的な補正について説明しました。 暗黙的な補正は単純なシナリオには適していますが、補正処理のスケジュールに関して、より明示的な制御が必要な場合は、<xref:System.Activities.Statements.Compensate> アクティビティを使用できます。 <xref:System.Activities.Statements.Compensate> アクティビティを使用して補正プロセスを開始するには、補正が望ましい <xref:System.Activities.Statements.CompensationToken> の <xref:System.Activities.Statements.CompensableActivity> を使用します。 <xref:System.Activities.Statements.Compensate> アクティビティは、完了した <xref:System.Activities.Statements.CompensableActivity> で、まだ確認または補正されていない場合に補正を開始するときに使用できます。 たとえば、<xref:System.Activities.Statements.Compensate> アクティビティは <xref:System.Activities.Statements.TryCatch.Catches%2A> アクティビティの <xref:System.Activities.Statements.TryCatch> セクションで使用できます。また、<xref:System.Activities.Statements.CompensableActivity> が完了した後の任意のタイミングで使用できます。 この例では、<xref:System.Activities.Statements.Compensate> アクティビティを <xref:System.Activities.Statements.TryCatch.Catches%2A> アクティビティの <xref:System.Activities.Statements.TryCatch> プロパティに使用し、<xref:System.Activities.Statements.CompensableActivity> のアクションを反転します。  
  
 [!code-csharp[CFX_CompensationExample#3](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#3)]  
  
 この例は XAML のワークフローです。  
  
```xaml  
<TryCatch  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:s="clr-namespace:System;assembly=mscorlib"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <TryCatch.Variables>  
    <Variable  
       x:TypeArguments="CompensationToken"  
       x:Name="__ReferenceID0"  
       Name="token1" />  
  </TryCatch.Variables>  
  <TryCatch.Try>  
    <Sequence>  
      <CompensableActivity>  
        <CompensableActivity.Result>  
          <OutArgument  
             x:TypeArguments="CompensationToken">  
            <VariableReference  
               x:TypeArguments="CompensationToken"  
               Variable="{x:Reference __ReferenceID0}">  
              <VariableReference.Result>  
                <OutArgument  
                   x:TypeArguments="Location(CompensationToken)" />  
              </VariableReference.Result>  
            </VariableReference>  
          </OutArgument>  
        </CompensableActivity.Result>  
        <CompensableActivity.CompensationHandler>  
          <c:CancelFlight />  
        </CompensableActivity.CompensationHandler>  
        <CompensableActivity.ConfirmationHandler>  
          <c:ConfirmFlight />  
        </CompensableActivity.ConfirmationHandler>  
        <c:ReserveFlight />  
      </CompensableActivity>  
      <c:SimulatedErrorCondition />  
      <c:ManagerApproval />  
      <c:PurchaseFlight />  
    </Sequence>  
  </TryCatch.Try>  
  <TryCatch.Catches>  
    <Catch  
       x:TypeArguments="s:ApplicationException">  
      <ActivityAction  
         x:TypeArguments="s:ApplicationException">  
        <Compensate>  
          <Compensate.Target>  
            <InArgument  
               x:TypeArguments="CompensationToken">  
              <VariableValue  
                 x:TypeArguments="CompensationToken"  
                 Variable="{x:Reference __ReferenceID0}">  
                <VariableValue.Result>  
                  <OutArgument  
                     x:TypeArguments="CompensationToken" />  
                </VariableValue.Result>  
              </VariableValue>  
            </InArgument>  
          </Compensate.Target>  
        </Compensate>  
      </ActivityAction>  
    </Catch>  
  </TryCatch.Catches>  
</TryCatch>  
```  
  
 このワークフローが呼び出されると、次の出力がコンソールに表示されます。  
  
 **ReserveFlight: チケットは予約されています。**  
**SimulatedErrorCondition: ApplicationException をスローしています。** 
**Cancelflight: チケットは取り消されました。** 
**ワークフローは正常に完了しました。状態: Closed。**

### <a name="confirming-compensation"></a>補正の確認  

 既定で、補正可能なアクティビティは、完了後の任意のタイミングで補正できます。 ただし、シナリオによっては適切でない場合があります。 前の例では、航空券予約の補正は、予約の取り消しでした。 ただし、この航空券に関する処理が完了すると、この補正手順は有効ではなくなります。 補正可能なアクティビティを確認すると、<xref:System.Activities.Statements.CompensableActivity.ConfirmationHandler%2A> で指定したアクティビティが呼び出されます。 この使用例としては、補正を実行する必要があるリソースを解放できるようになることが挙げられます。 補正可能なアクティビティは、確認されると補正できなくなります。また、この処理が試行されると、<xref:System.InvalidOperationException> 例外がスローされます。 ワークフローが正常に完了すると、正常に完了したがまだ確認も補正も実行されていない補正可能なすべてのアクティビティは、補正とは逆の順序で確認されます。 この例では、航空券の予約、購入、および完了後に、補正可能なアクティビティが確認されます。 <xref:System.Activities.Statements.CompensableActivity> を確認するには、<xref:System.Activities.Statements.Confirm> アクティビティを使用し、<xref:System.Activities.Statements.CompensationToken> の <xref:System.Activities.Statements.CompensableActivity> を指定します。  
  
 [!code-csharp[CFX_CompensationExample#4](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#4)]  
  
 この例は XAML のワークフローです。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <Sequence.Variables>  
    <x:Reference>__ReferenceID0</x:Reference>  
  </Sequence.Variables>  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken">  
        <VariableReference  
           x:TypeArguments="CompensationToken">  
          <VariableReference.Result>  
            <OutArgument  
               x:TypeArguments="Location(CompensationToken)" />  
          </VariableReference.Result>  
          <VariableReference.Variable>  
            <Variable  
               x:TypeArguments="CompensationToken"  
               x:Name="__ReferenceID0"  
               Name="token1" />  
          </VariableReference.Variable>  
        </VariableReference>  
      </OutArgument>  
    </CompensableActivity.Result>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
    <CompensableActivity.ConfirmationHandler>  
      <c:ConfirmFlight />  
    </CompensableActivity.ConfirmationHandler>  
    <c:ReserveFlight />  
  </CompensableActivity>  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
  <c:TakeFlight />  
  <Confirm>  
    <Confirm.Target>  
      <InArgument  
         x:TypeArguments="CompensationToken">  
        <VariableValue  
           x:TypeArguments="CompensationToken"  
           Variable="{x:Reference __ReferenceID0}">  
          <VariableValue.Result>  
            <OutArgument  
               x:TypeArguments="CompensationToken" />  
          </VariableValue.Result>  
        </VariableValue>  
      </InArgument>  
    </Confirm.Target>  
  </Confirm>  
</Sequence>  
```  
  
このワークフローが呼び出されると、次の出力がコンソールに表示されます。  
  
**ReserveFlight: チケットは予約されています。**  
**Managerapproval: マネージャーの承認を受け取りました。** 
**PurchaseFlight: チケットが購入されます。** 
**動力飛行: フライトが完了しました。** 
**Confirmflight: フライトが実行されました。補正することはできません。** 
**ワークフローは正常に完了しました。状態: Closed。**

## <a name="nesting-compensation-activities"></a>補正アクティビティの入れ子化  

<xref:System.Activities.Statements.CompensableActivity> は、別の <xref:System.Activities.Statements.CompensableActivity.Body%2A> の <xref:System.Activities.Statements.CompensableActivity> セクションに配置できます。 <xref:System.Activities.Statements.CompensableActivity> は、別の <xref:System.Activities.Statements.CompensableActivity> のハンドラーで処理されない場合があります。 親の <xref:System.Activities.Statements.CompensableActivity> が取り消されたとき、確認されたとき、または補正されたとき、正常に完了していてまだ確認または補正されていないすべての子の補正可能なアクティビティを、親が取り消し、確認、または補正を完了する前に、確認または補正されるようにするのが親のそのアクティビティの役割ですです。 補正が明示的にモデル化されていない場合、親の <xref:System.Activities.Statements.CompensableActivity> は、親が取り消しまたは補正のシグナルを受け取ったとき、子の補正可能なアクティビティを暗黙的に補正します。 親は、確認通知を受け取ると、暗黙的に子の補正可能なアクティビティを確認します。 取り消し、確認、または補正を処理するロジックが親の <xref:System.Activities.Statements.CompensableActivity> のハンドラーで明示的にモデル化されている場合、明示的に処理されなかった子は暗黙的に確認されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Activities.Statements.CompensableActivity>
- <xref:System.Activities.Statements.Compensate>
- <xref:System.Activities.Statements.Confirm>
- <xref:System.Activities.Statements.CompensationToken>
