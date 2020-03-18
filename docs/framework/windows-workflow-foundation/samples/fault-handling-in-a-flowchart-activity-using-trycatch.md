---
title: TryCatch を使用した Flowchart アクティビティでのエラー処理
ms.date: 03/30/2017
ms.assetid: 50922964-bfe0-4ba8-9422-0e7220d514fd
ms.openlocfilehash: 8e3ca59bc9743300a230877a6fbcbed5468a1589
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74710836"
---
# <a name="fault-handling-in-a-flowchart-activity-using-trycatch"></a>TryCatch を使用した Flowchart アクティビティでのエラー処理

このサンプルでは、複雑な制御フロー アクティビティ内で <xref:System.Activities.Statements.TryCatch> アクティビティを使用する方法を示します。

このサンプルでは、販売促進コードに対応する式に基づいて割引率を計算する <xref:System.Activities.Statements.Flowchart> アクティビティに、販売促進コードと子供の数が変数として渡されます。 このサンプルには、命令型コードとワークフロー デザイナー バージョンのサンプルが含まれています。

次の表で、`CreateFlowchartWithFaults` アクティビティの変数の詳細を説明します。

|パラメーター|説明|
|----------------|-----------------|
|promoCode|販売促進コード。 型: String<br /><br /> 使用できる値は次のとおりです (かっこ内は値の説明)。<br /><br /> -Single (Single)<br />-MNK (子供がいない結婚)<br />-MWK (子供との結婚)|
|numKids|子供の数。 型: int|

`CreateFlowchartWithFaults` アクティビティでは、<xref:System.Activities.Statements.FlowSwitch%601> 引数を有効にする `promoCode` アクティビティを使用し、次の式を使って割引率を計算します。

|`promoCode` の値|割引率 (%)|
|--------------------------|--------------------|
|Single|10|
|MNK|16|
|MWK|15 + (1 ~ 1/`numberOfKids`)\*10**メモ:** この計算では <xref:System.DivideByZeroException>がスローできる可能性があります。 そのため、割引率の計算は、<xref:System.Activities.Statements.TryCatch> 例外をキャッチして割引率をゼロに設定する <xref:System.DivideByZeroException> アクティビティでラップされます。|

#### <a name="to-use-this-sample"></a>このサンプルを使用するには

1. Visual Studio 2010 を使用して、FlowchartWithFaultHandling ソリューションファイルを開きます。

2. ソリューションをビルドするには、Ctrl キーと Shift キーを押しながら B キーを押します。

3. ソリューションを実行するには、F5 キーを押します。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Built-InActivities\FlowChartWithFaultHandling`

## <a name="see-also"></a>参照

- [Flowchart のワークフロー](../flowchart-workflows.md)
- [例外](../exceptions.md)
