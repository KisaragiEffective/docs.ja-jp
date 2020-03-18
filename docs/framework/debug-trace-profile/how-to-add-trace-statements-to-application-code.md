---
title: '方法 : アプリケーション コードにトレース ステートメントを追加する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework], conditional writes based on switches
- trace statements
- WriteLineIf method
- tracing [.NET Framework], adding trace statements
- Assert method, tracing code
- trace switches, conditional writes based on switches
- WriteIf method
ms.assetid: f3a93fa7-1717-467d-aaff-393e5c9828b4
ms.openlocfilehash: 21df0e8129505e50e6b7f29c4f4f5aea94f380e3
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217468"
---
# <a name="how-to-add-trace-statements-to-application-code"></a>方法 : アプリケーション コードにトレース ステートメントを追加する
トレースで最も使用頻度の高いメソッドは、出力をリスナーに書き込むメソッドである **Write**、**WriteIf**、**WriteLine**、**WriteLineIf**、**Assert**、および **Fail** です。 これらのメソッドは、次の 2 つのカテゴリに分類できます。**Write**、**WriteLine**、および **Fail** はすべて出力を無条件に生成します。それに対して、**WriteIf**、**WriteLineIf**、および **Assert** はブール条件をテストし、条件の値に基づいて書き込みを行ったり行わなかったりします。 **WriteIf** と **WriteLineIf** は条件が `true` の場合に出力を生成し、**Assert** は条件が `false` の場合に出力を生成します。  
  
 トレースおよびデバッグの方法をデザインするときは、出力の表示方法について検討する必要があります。 関連のない情報で埋め尽くされた複数の **Write** ステートメントの場合、読みにくいログが作成されます。 その一方で、**WriteLine** を使用して関連のあるステートメントを別々の行に出力すると、どの情報が関連し合っているかを読み取るのが困難になる場合があります。 一般に、複数のソースからの情報を結合する場合には複数の **Write** ステートメントを使用して 1 つの情報メッセージを作成し、単一の完全なメッセージを作成する場合には **WriteLine** ステートメントを使用します。  
  
### <a name="to-write-a-complete-line"></a>完結した行を書き込むには  
  
1. <xref:System.Diagnostics.Trace.WriteLine%2A> メソッドまたは <xref:System.Diagnostics.Trace.WriteLineIf%2A> メソッドを呼び出します。  
  
     このメソッドが返すメッセージの末尾には、キャリッジ リターンが追加されます。したがって、次回の **Write**、**WriteIf**、**WriteLine**、または **WriteLineIf** が返すメッセージは、次の行から開始されます。  
  
    ```vb  
    Dim errorFlag As Boolean = False  
    Trace.WriteLine("Error in AppendData procedure.")  
    Trace.WriteLineIf(errorFlag, "Error in AppendData procedure.")  
    ```  
  
    ```csharp  
    bool errorFlag = false;  
    System.Diagnostics.Trace.WriteLine ("Error in AppendData procedure.");  
    System.Diagnostics.Trace.WriteLineIf(errorFlag,   
       "Error in AppendData procedure.");  
    ```  
  
### <a name="to-write-a-partial-line"></a>完結しない行を書き込むには  
  
1. <xref:System.Diagnostics.Trace.Write%2A> メソッドまたは <xref:System.Diagnostics.Trace.WriteIf%2A> メソッドを呼び出します。  
  
     **Write**、**WriteIf**、**WriteLine** または **WriteLineIf** によって次に書き込まれるメッセージは、今回の **Write** または **WriteIf** ステートメントによって書き込まれるメッセージと同じ行から開始されます。  
  
    ```vb  
    Dim errorFlag As Boolean = False  
    Trace.WriteIf(errorFlag, "Error in AppendData procedure.")  
    Debug.WriteIf(errorFlag, "Transaction abandoned.")  
    Trace.Write("Invalid value for data request")  
    ```  
  
    ```csharp  
    bool errorFlag = false;  
    System.Diagnostics.Trace.WriteIf(errorFlag,   
       "Error in AppendData procedure.");  
    System.Diagnostics.Debug.WriteIf(errorFlag, "Transaction abandoned.");  
    Trace.Write("Invalid value for data request");  
    ```  
  
### <a name="to-verify-that-certain-conditions-exist-either-before-or-after-you-execute-a-method"></a>メソッドの実行前または実行後に特定の条件が存在するかどうかを確認するには  
  
1. <xref:System.Diagnostics.Trace.Assert%2A> メソッドを呼び出します。  
  
    ```vb  
    Dim i As Integer = 4  
    Trace.Assert(i = 5, "i is not equal to 5.")  
    ```  
  
    ```csharp  
    int i = 4;  
    System.Diagnostics.Trace.Assert(i == 5, "i is not equal to 5.");  
    ```  
  
    > [!NOTE]
    > **Assert** は、トレースとデバッグの両方で使用できます。 この例では、呼び出し履歴を **Listeners** コレクションのリスナーに出力しています。 詳細については、「[マネージド コードのアサーション](/visualstudio/debugger/assertions-in-managed-code)」および「<xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Diagnostics.Debug.WriteIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Debug.WriteLineIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Trace.WriteIf%2A?displayProperty=nameWithType>
- <xref:System.Diagnostics.Trace.WriteLineIf%2A?displayProperty=nameWithType>
- [アプリケーションのトレースとインストルメント](tracing-and-instrumenting-applications.md)
- [方法 : トレース スイッチを作成、初期化、および構成する](how-to-create-initialize-and-configure-trace-switches.md)
- [トレース スイッチ](trace-switches.md)
- [トレース リスナー](trace-listeners.md)
