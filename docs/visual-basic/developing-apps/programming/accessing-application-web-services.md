---
description: '詳細情報: アプリケーションの Web サービスへのアクセス (Visual Basic)'
title: アプリケーションの Web サービスへのアクセス
ms.date: 07/20/2015
helpviewer_keywords:
- Web services [Visual Basic], My.WebServices object
- My.WebServices object
- applications [Visual Basic], Web services
ms.assetid: 8ad5405b-e771-42b1-82d3-ce97af2cea9e
ms.openlocfilehash: 4efd35ed7c8f4251a749b85a45242af299a51e6c
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99797888"
---
# <a name="accessing-application-web-services-visual-basic"></a>アプリケーションの Web サービスへのアクセス (Visual Basic)

`My.WebServices` オブジェクトは、現在のプロジェクトにより参照されている各 Web サービスのインスタンスを提供します。 各インスタンスは要求に応じてインスタンス化されます。 これらの Web サービスには `My.WebServices` オブジェクトのプロパティを介してアクセスできます。 プロパティの名前は、プロパティがアクセスする Web サービスの名前と同じになります。 <xref:System.Web.Services.Protocols.SoapHttpClientProtocol> から継承されたクラスはすべて Web サービスです。

## <a name="tasks"></a>タスク

次の表は、アプリケーションにより参照される Web サービスにアクセスする方法を一覧にしたものです。

|終了|解決方法については、|
|---|---|
|Web サービスを呼び出す|[My.WebServices オブジェクト](../../language-reference/objects/my-webservices-object.md)|
|Web サービスを非同期で呼び出し、完了時にイベントを処理する|[方法: Web サービスを非同期で呼び出す](how-to-call-a-web-service-asynchronously.md)|

## <a name="see-also"></a>関連項目

- [My.WebServices オブジェクト](../../language-reference/objects/my-webservices-object.md)
