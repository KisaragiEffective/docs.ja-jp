---
title: ネットワークのトレースの解釈
ms.date: 03/30/2017
helpviewer_keywords:
- TraceMode attribute
- hexidecimal data, network tracing output
- network tracing, analyzing
- protocolonly
- text, network tracing output
- includehex
ms.assetid: ad22b4b8-00af-4778-9cca-cb609ce1f8ff
ms.openlocfilehash: fd617e152b1e86cc71dd8e3cc8a01f1d2f52c30a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047902"
---
# <a name="interpreting-network-tracing"></a>ネットワークのトレースの解釈
ネットワークのトレースを有効にすると、トレースを使用して、アプリケーションからさまざまな <xref:System.Net> クラス メンバーへの呼び出しをキャプチャすることができます。 これらの呼び出しからの出力は、次の例のようになる場合があります。  
  
```output
[588]   (4357)   Entering Socket#33574638::Send()  
[588]   (4387)   Exiting Socket#33574638::Send()-> 61#61
```  
  
 上の例では、[588] は現在のスレッドの一意の識別子です。 (4357) と (4387) は、アプリケーションが起動してから経過したミリ秒数を示すタイムスタンプです。 タイムスタンプの後のデータは、メソッド **Socket.Send** を開始および終了するアプリケーションを示しています。 **Send** メソッドを実行するオブジェクトは、一意の識別子として 33574638 を持っています。 メソッドの exit トレースには、戻り値 (上の例では 61) が含まれています。  
  
 ネットワークのトレースは、ハイパーテキスト転送プロトコル (HTTP) などのアプリケーション レベルのプロトコルを使用して、アプリケーションから送信されるまたはアプリケーションで受信されるネットワーク トラフィックをキャプチャできます。 このデータは、テキストとして、また任意で、16 進数データとしてキャプチャできます。 16 進数データは、**tracemode** 属性の値として **includehex** を指定した場合に使用できます (この属性の詳細については、「[方法:ネットワークのトレースを構成する](how-to-configure-network-tracing.md)」を参照してください)。次のサンプル トレースは、**includehex** を使用して生成されました。  
  
 `[1692]   (1142)   00000000 : 47 45 54 20 2F 77 70 61-64 2E 64 61 74 20 48 54 : GET /wpad.dat HT`  
  
 `[1692]   (1142)   00000010 : 54 50 2F 31 2E 31 0D 0A-48 6F 73 74 3A 20 69 74 : TP/1.1..Host: it`  
  
 `[1692]   (1142)   00000020 : 67 70 72 6F 78 79 0D 0A-43 6F 6E 6E 65 63 74 69 : gproxy..Connecti`  
  
 `[1692]   (1142)   00000030 : 6F 6E 3A 20 43 6C 6F 73-65 0D 0A 0D 0A     : on: Close....`  
  
 16 進数データを省略するには、**tracemode** 属性の値として **protocolonly** を指定します。 次の例は、**protocolonly** が指定されている場合のトレースを示しています。  
  
 `[2444]   (594)   Data from ConnectStream#33574638::WriteHeaders<<GET /wpad.dat HTTP/1.1`  
  
 `Host: itgproxy`  
  
 `Connection: Close`  
  
## <a name="see-also"></a>関連項目

- [ネットワークのトレースの有効化](enabling-network-tracing.md)
- [方法: ネットワークのトレースを構成する](how-to-configure-network-tracing.md)
- [.NET Framework のネットワークのトレース](network-tracing.md)
