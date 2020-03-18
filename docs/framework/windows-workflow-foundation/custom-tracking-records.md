---
title: カスタム追跡レコード
ms.date: 03/30/2017
ms.assetid: 24284565-c68b-40bf-b7f1-e148d151a6fc
ms.openlocfilehash: 986f0350c24414d0ff960474445adf6ac3f39734
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802623"
---
# <a name="custom-tracking-records"></a>カスタム追跡レコード

このトピックではカスタム追跡レコードを作成し、出力されたデータをレコードと一緒に読み込む方法を示します。

## <a name="emitting-custom-tracking-records"></a>カスタム追跡レコードの出力

次の例に示すように、カスタム追跡レコードはコード アクティビティから出力できます。

```csharp
protected override void Execute(CodeActivityContext context)
{
…
            CustomTrackingRecord customRecord = new CustomTrackingRecord("CustomEmailSentEvent");
            customRecord.Data.Add("SendTime", sendTime);
            context.Track(customRecord);
}
```

<xref:System.Activities.Tracking.CustomTrackingRecord> 上で <xref:System.Activities.NativeActivityContext.Track%2A> メソッドを呼び出すことで、`ActivityContext` をコード アクティビティに出力できます。

## <a name="see-also"></a>参照

- [Windows Server App Fabric の監視](https://docs.microsoft.com/previous-versions/appfabric/ee677251(v=azure.10))
- [App Fabric を使用したアプリケーションの監視](https://docs.microsoft.com/previous-versions/appfabric/ee677276(v=azure.10))
