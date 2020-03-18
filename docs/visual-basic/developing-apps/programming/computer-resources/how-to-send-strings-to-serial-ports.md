---
title: '方法 : シリアル ポートに文字列を送信する'
ms.date: 07/20/2015
helpviewer_keywords:
- ports, sending strings to
- strings [Visual Basic], sending to serial ports
- My.Computer.Ports object
- serial ports, sending strings to
ms.assetid: 6ebf46cd-b2d0-4b2c-9a1f-be177b22ad52
ms.openlocfilehash: b2051451142a7818a3b7d1bc564c5ae36b2579fe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345586"
---
# <a name="how-to-send-strings-to-serial-ports-in-visual-basic"></a>方法 : Visual Basic でシリアル ポートに文字列を送信する

このトピックでは、`My.Computer.Ports` を使用して、Visual Basic でコンピューターのシリアルポートに文字列を送信する方法について説明します。  
  
## <a name="example"></a>例  

 この例では、COM1 シリアル ポートに文字列を送信します。 コンピューターによっては、別のシリアル ポートを使用する必要が生じる場合があります。  
  
 `My.Computer.Ports.OpenSerialPort` メソッドを使用して、ポートへの参照を取得します。 詳細については、<xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A> を参照してください。  
  
 `Using` ブロックを使用すると、アプリケーションが例外を生成した場合でも、シリアル ポートを閉じることができます。 シリアル ポートを操作するコードはすべて、このブロックまたは `Try...Catch...Finally` ブロック内に記述する必要があります。  
  
 <xref:System.IO.Ports.SerialPort.WriteLine%2A> メソッドで、データをシリアル ポートに送信しています。  
  
 [!code-vb[VbVbalrMyComputer#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#33)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- この例では、コンピューターが `COM1` を使用しているものと想定しています。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 この例では、コンピューターが `COM1` を使用しているものと想定しています。実際に作成するコードでは、柔軟性を高めるために、利用可能なポートの一覧から目的のシリアル ポートを選択できるようにすることをお勧めします。 詳細については、「[方法: 利用可能なシリアル ポートを表示する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-show-available-serial-ports.md)」を参照してください。  
  
 この例では、アプリケーションが例外をスローした場合でもポートを閉じられるよう、`Using` ブロックを使用しています。 詳細については、「[Using ステートメント](../../../../visual-basic/language-reference/statements/using-statement.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [方法 : シリアル ポートに接続されているモデムをダイヤルする](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-dial-modems-attached-to-serial-ports.md)
- [方法 : 利用可能なシリアル ポートを表示する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-show-available-serial-ports.md)
