---
title: '方法 : シリアル ポートに接続されているモデムをダイヤルする'
ms.date: 07/20/2015
helpviewer_keywords:
- modems [Visual Basic], dialing
- serial ports [Visual Basic], dialing
- My.Computer.Ports object
ms.assetid: 3834db40-f431-45f1-b671-dc91787164b6
ms.openlocfilehash: febec0a8579d34f8ff59066da5b5aa59c1cce6b2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345636"
---
# <a name="how-to-dial-modems-attached-to-serial-ports-in-visual-basic"></a>方法 : Visual Basic で、シリアル ポートに接続されているモデムをダイヤルする

このトピックでは、Visual Basic で `My.Computer.Ports` を使用してモデムをダイヤルする方法について説明します。  
  
 通常、モデムはコンピューターのいずれかのシリアル ポートに接続されています。 アプリケーションがモデムとやり取りするには、適切なシリアル ポートにコマンドを送信する必要があります。  
  
### <a name="to-dial-a-modem"></a>モデムをダイヤルするには  
  
1. モデムが接続されているシリアル ポートを確認します。 この例では、モデムが COM1 に接続されていることを前提としています。  
  
2. `My.Computer.Ports.OpenSerialPort` メソッドを使用して、ポートへの参照を取得します。 詳細については、<xref:Microsoft.VisualBasic.Devices.Ports.OpenSerialPort%2A> を参照してください。  
  
     `Using` ブロックを使用すると、アプリケーションが例外を生成した場合でも、シリアル ポートを閉じることができます。 シリアル ポートを操作するコードはすべて、このブロックまたは `Try...Catch...Finally` ブロック内に記述する必要があります。  
  
     [!code-vb[VbVbalrMyComputer#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#28)]  
  
3. コンピューターがモデムからの伝送を受け取る準備ができたことを示しよう `DtrEnable` プロパティを設定します。  
  
     [!code-vb[VbVbalrMyComputer#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#29)]  
  
4. <xref:System.IO.Ports.SerialPort.Write%2A> メソッドを使用して、ダイヤル コマンドと電話番号をシリアル ポート経由でモデムに送信します。  
  
     [!code-vb[VbVbalrMyComputer#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#30)]  
  
## <a name="example"></a>例  

 [!code-vb[VbVbalrMyComputer#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#27)]  
  
 このコード例は IntelliSense コード スニペットとしても利用できます。 コード スニペット ピッカーでは、これは **[接続とネットワーク]** にあります。 詳細については、「 [Code Snippets](/visualstudio/ide/code-snippets)」を参照してください。  
  
## <a name="compiling-the-code"></a>コードのコンパイル  

 この例では、<xref:System?displayProperty=nameWithType> 名前空間への参照が必要です。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 この例では、モデムが COM1 に接続されていることを前提としています。 コードによって、利用可能なシリアル ポートの一覧から、目的のポートをユーザーが選択できるようにすることをお勧めします。 詳細については、「[方法: 利用可能なシリアル ポートを表示する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-show-available-serial-ports.md)」を参照してください。  
  
 この例では、アプリケーションが例外をスローした場合でもポートを閉じられるよう、`Using` ブロックを使用しています。 詳細については、「[Using ステートメント](../../../../visual-basic/language-reference/statements/using-statement.md)」を参照してください。  
  
 この例では、アプリケーションは、モデムをダイヤルした後でシリアル ポートを切断しています。 実際には、モデムとの間でデータの転送が必要となります。 詳細については、「[方法: シリアル ポートから文字列を受信する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-receive-strings-from-serial-ports.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Devices.Ports>
- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
- [方法 : シリアル ポートに文字列を送信する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-send-strings-to-serial-ports.md)
- [方法 : シリアル ポートから文字列を受信する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-receive-strings-from-serial-ports.md)
- [方法 : 利用可能なシリアル ポートを表示する](../../../../visual-basic/developing-apps/programming/computer-resources/how-to-show-available-serial-ports.md)
