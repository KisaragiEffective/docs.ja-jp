---
title: サウンドの再生
ms.date: 07/20/2015
helpviewer_keywords:
- system sounds, playing
- system sounds
- playing sounds, Visual Basic
- sound loops
- My.Computer.Audio object, tasks
- sounds, playing
- sounds, background
- playing sounds
ms.assetid: f0d9e4ab-57c7-47b6-86d3-99ff07078040
ms.openlocfilehash: 416fedd011ff35d2b32d1b64932e3908a73ed14e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74345522"
---
# <a name="playing-sounds-visual-basic"></a>サウンドの再生 (Visual Basic)

`My.Computer.Audio` オブジェクトには、サウンドを再生するためのメソッドが用意されています。  
  
## <a name="playing-sounds"></a>サウンドの再生  

 バックグラウンド再生により、アプリケーションでサウンドを再生しながら他のコードを実行できます。 `My.Computer.Audio.Play` メソッドを使用すると、アプリケーションでバックグラウンド サウンドを一度に 1 つだけ再生できます。アプリケーションで新しいバックグラウンド サウンドが再生されると、その前のバックグラウンド サウンドの再生は停止します。 また、サウンドを再生してから、その再生が完了するまで待機することもできます。  
  
 次の例では、`My.Computer.Audio.Play` メソッドによってサウンドが再生されます。 `AudioPlayMode.WaitToComplete` が指定されている場合、`My.Computer.Audio.Play` はサウンドの再生が完了するまで待機し、その後呼び出し元のコードが処理を再開します。 この例を使用する場合は、必ず自分のコンピューター上の .wav サウンド ファイルを示すファイル名を指定してください。  
  
 [!code-vb[VbVbalrMyComputer#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#15)]  
  
 次の例では、`My.Computer.Audio.Play` メソッドによってサウンドが再生されます。 この例を使用する場合は、アプリケーション リソースに Waterfall という名前の .wav サウンド ファイルが含まれていることを確認してください。  
  
 [!code-vb[VbVbalrMyComputer#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#16)]  
  
## <a name="playing-looping-sounds"></a>ループ サウンドの再生  

 次の例では、`My.Computer.Audio.Play` が指定されている場合に、指定されているサウンドが `PlayMode.BackgroundLoop` メソッドによってバックグラウンドで再生されます。 この例を使用する場合は、必ず自分のコンピューター上の .wav サウンド ファイルを示すファイル名を指定してください。  
  
 [!code-vb[VbVbalrMyComputer#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#11)]  
  
 次の例では、`My.Computer.Audio.Play` が指定されている場合に、指定されているサウンドが `PlayMode.BackgroundLoop` メソッドによってバックグラウンドで再生されます。 この例を使用する場合は、アプリケーション リソースに Waterfall という名前の .wav サウンド ファイルが含まれていることを確認してください。  
  
 [!code-vb[VbVbalrMyComputer#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#12)]  
  
 上記のコード例は、IntelliSense コード スニペットとしても利用できます。 コード スニペット ピッカーでこのスニペットにアクセスするには、 **[Windows フォーム アプリケーション]、[サウンド]** の順に移動します。 詳細については、「 [Code Snippets](/visualstudio/ide/code-snippets)」を参照してください。  
  
 一般に、アプリケーションでループ サウンドを再生する場合は、最終的にそのサウンドを停止する必要があります。  
  
## <a name="stopping-the-playing-of-sounds-in-the-background"></a>バックグラウンドで再生しているサウンドの停止  

 `My.Computer.Audio.Stop` メソッドを使用し、アプリケーションで現在再生中のバックグラウンド サウンドまたはループ サウンドを停止します。  
  
 一般に、アプリケーションでループ サウンドを再生する場合は、どこかの時点でそのサウンドを停止する必要があります。  
  
 次の例では、バックグラウンドで再生中のサウンドを停止します。  
  
 [!code-vb[VbVbalrMyComputer#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#18)]  
  
 上記のコード例は、IntelliSense コード スニペットとしても利用できます。 コード スニペット ピッカーでこのスニペットにアクセスするには、 **[Windows フォーム アプリケーション]、[サウンド]** の順に移動します。 詳細については、「 [Code Snippets](/visualstudio/ide/code-snippets)」を参照してください。  
  
## <a name="playing-system-sounds"></a>システム サウンドの再生  

 `My.Computer.Audio.PlaySystemSound` メソッドを使用して、指定したシステム サウンドを再生します。  
  
 `My.Computer.Audio.PlaySystemSound` メソッドは、<xref:System.Media.SystemSound> クラスのいずれかの共有メンバーをパラメーターとして受け取ります。 システム サウンド <xref:System.Media.SystemSounds.Asterisk%2A> は、一般にエラーを表します。  
  
 次の例では、`My.Computer.Audio.PlaySystemSound` メソッドを使用してシステム サウンドを再生しています。  
  
 [!code-vb[VbVbalrMyComputer#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Devices.Audio>
- <xref:Microsoft.VisualBasic.Devices.Audio.Play%2A>
- <xref:Microsoft.VisualBasic.Devices.Audio.PlaySystemSound%2A>
- <xref:Microsoft.VisualBasic.Devices.Audio.Stop%2A>
- <xref:Microsoft.VisualBasic.AudioPlayMode>
