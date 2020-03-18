---
title: .NET Framework でのポート操作
ms.date: 07/20/2015
helpviewer_keywords:
- ports, Visual Basic
ms.assetid: 1eba223b-7bd3-401a-b097-982bce96df1b
ms.openlocfilehash: 68f0c67b8135c6bb7ce8a134e2a324bc12c0aad9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345507"
---
# <a name="port-operations-in-the-net-framework-with-visual-basic"></a>Visual Basic による .NET Framework でのポート操作

<xref:System.IO.Ports?displayProperty=nameWithType> 名前空間の .NET Framework クラスを介して、コンピューターのシリアル ポートにアクセスできます。 最も重要な役割を持つのが <xref:System.IO.Ports.SerialPort> クラスです。このクラスにより、同期 I/O とイベント ドリブン I/O のフレームワーク、ピンの状態とブレーク状態へのアクセス、およびシリアル ドライバーのプロパティへのアクセスを使用できます。 このクラスは、<xref:System.IO.Stream> プロパティを通じてアクセス可能な <xref:System.IO.Ports.SerialPort.BaseStream> オブジェクト内にラップできます。 <xref:System.IO.Ports.SerialPort> オブジェクト内で <xref:System.IO.Stream> をラップすることにより、ストリームを使用するクラスからシリアル ポートへのアクセスを実現できます。 名前空間には、シリアル ポートの制御を容易にする列挙型が含まれています。

最も簡単に <xref:System.IO.Ports.SerialPort> オブジェクトを作成する方法は、<xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A> メソッドを経由することです。

> [!NOTE]
> .NET Framework クラスを使用してパラレル ポートや USB ポートなどの他の種類のポートに直接アクセスすることはできません。

## <a name="enumerations"></a>列挙体

次の表は、シリアル ポートへのアクセスに使用される主な列挙型を一覧にまとめ、説明を加えたものです。

|Enumeration|[説明]|
|---|---|
|<xref:System.IO.Ports.Handshake>|<xref:System.IO.Ports.SerialPort> オブジェクトでのシリアル ポート通信の確立に使用する制御プロトコルを指定します。|
|<xref:System.IO.Ports.Parity>|<xref:System.IO.Ports.SerialPort> オブジェクトのパリティ ビットを指定します。|
|<xref:System.IO.Ports.SerialData>|<xref:System.IO.Ports.SerialPort> オブジェクトのシリアル ポートで受信した文字の型を指定します。|
|<xref:System.IO.Ports.SerialError>|<xref:System.IO.Ports.SerialPort> オブジェクトで発生するエラーを指定します。|
|<xref:System.IO.Ports.SerialPinChange>|<xref:System.IO.Ports.SerialPort> オブジェクトで発生した変更の種類を指定します。|
|<xref:System.IO.Ports.StopBits>|<xref:System.IO.Ports.SerialPort> オブジェクトで使用するストップ ビットの数を指定します。|

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Devices.Ports>
- [コンピューターのポートへのアクセス](../../../../visual-basic/developing-apps/programming/computer-resources/accessing-the-computer-s-ports.md)
