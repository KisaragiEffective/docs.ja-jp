---
title: トレース スイッチ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tracing [.NET Framework], trace switches
- trace switches, about trace switches
- tracing [.NET Framework], level of detail
- switches, trace
- trace switches
- trace switches, creating custom
ms.assetid: 8ab913aa-f400-4406-9436-f45bc6e54fbe
ms.openlocfilehash: c164e26c6757094b9820af14a098229ab11eb137
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217204"
---
# <a name="trace-switches"></a>トレース スイッチ
トレース スイッチを使用すると、トレース出力を有効/無効にしたり、トレースの出力をフィルター処理したりできます。 トレース スイッチは、コードに存在すれるオブジェクトであり、.config ファイルによって外部的に設定できます。 .NET Framework に用意されたトレース スイッチには、 <xref:System.Diagnostics.BooleanSwitch> クラス、 <xref:System.Diagnostics.TraceSwitch> クラス、および <xref:System.Diagnostics.SourceSwitch> クラスの 3 種類があります。 <xref:System.Diagnostics.BooleanSwitch> クラスは、トグル スイッチとして機能し、各種のトレース ステートメントを有効にしたり無効にしたりできます。 <xref:System.Diagnostics.TraceSwitch> クラスと <xref:System.Diagnostics.SourceSwitch> クラスを使用すると、特定のトレース レベルごとにトレース スイッチを有効にすることができます。これにより、該当するレベルおよびそれ以下のすべてのレベルに対して指定された <xref:System.Diagnostics.Trace> メッセージまたは <xref:System.Diagnostics.TraceSource> メッセージが表示されます。 スイッチを無効にした場合、トレース メッセージは表示されません。 すべてのクラスは、他のユーザー定義のスイッチと同様に、どちらも**Switch**という抽象 ( **MustInherit**) クラスから派生したクラスです。  
  
 トレース スイッチは情報のフィルター処理に役立つことがあります。 たとえば、データ アクセス モジュールではすべてのトレース メッセージを表示し、アプリケーションの他の部分ではエラー メッセージだけを表示できます。 この場合は、データ アクセス モジュールに対してトレース スイッチを 1 つ使用し、アプリケーションの他の部分に対してスイッチを 1 つ使用します。 .config ファイルの構成でスイッチを適切に設定することにより、受け取るトレース メッセージの種類を制御できます。 詳細については、「[方法 : トレース スイッチを作成、初期化、および構成する](how-to-create-initialize-and-configure-trace-switches.md)」を参照してください。  
  
 一般に、配置されたアプリケーションはスイッチが無効にされた状態で実行されるため、ユーザーにとって意味のないトレース メッセージが画面に表示されたり、アプリケーションの実行ごとにログ ファイルに書き込んだりする必要はありません。 アプリケーションの実行時に問題が発生した場合は、アプリケーションを中断し、スイッチを有効にしてからアプリケーションを再起動します。 これにより、トレース メッセージが表示されるようになります。  
  
 スイッチを使用するには、まず、 **BooleanSwitch** クラス、 **TraceSwitch** クラス、または開発者が定義したスイッチ クラスからスイッチ オブジェクトを作成する必要があります。 開発者定義スイッチの作成方法については、「.NET Framework リファレンス」の「 <xref:System.Diagnostics.Switch> クラス」を参照してください。 次に、スイッチ オブジェクトを使用するタイミングを指定する構成値を設定します。 その後で、スイッチ オブジェクトの設定を各種の **Trace** (または **Debug**) トレース メソッドでテストします。  
  
## <a name="trace-levels"></a>トレース レベル  
 **TraceSwitch**を使用するときには、いくつかの付加的な考慮事項があります。 **TraceSwitch** オブジェクトには、スイッチを特定のレベル以上として設定するかどうかを示す **Boolean** 値を返す、4 つのプロパティがあります。  
  
- <xref:System.Diagnostics.TraceSwitch.TraceError%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceWarning%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceInfo%2A?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceSwitch.TraceVerbose%2A?displayProperty=nameWithType>  
  
 レベルを使用することにより、受け取るトレース情報を問題解決に必要な情報だけに制限できます。 トレース スイッチを適切なトレース レベルに設定したり構成したりして、トレース出力に必要な詳細レベルを指定します。 エラー メッセージ、警告メッセージ、情報メッセージ、または詳細トレース メッセージを受け取ることができます。また、メッセージを一切受け取らないようにすることもできます。  
  
 各レベルに関連付けるメッセージの種類は、すべて開発者が指定します。 トレース メッセージの内容は、通常、各レベルに関連付けた内容に依存しますが、レベルごとに動作の違いを設定できます。 たとえば、レベル 3 (**Info**) の問題については詳細な説明を提供するが、レベル 1 (**Error**) の問題についてはエラー参照番号だけを提供する場合があります。 アプリケーションで最適な動作を得ることができるスキームは、独自に判断してください。  
  
 これらのプロパティは、 **TraceLevel** 列挙型の値 1 から 4 に対応します。 **TraceLevel** 列挙型のレベルとそれぞれの値を次の表に示します。  
  
|列挙値|整数値|表示されるメッセージ (または指定された出力対象に書き込まれるメッセージ) の種類|  
|----------------------|-------------------|---------------------------------------------------------------------------|  
|オフ|0|なし|  
|エラー|1|エラー メッセージのみ|  
|［警告］|2|警告メッセージとエラー メッセージ|  
|[情報]|3|通知メッセージ、警告メッセージ、およびエラー メッセージ|  
|"詳細"|4|詳細メッセージ、情報メッセージ、警告メッセージ、およびエラー メッセージ|  
  
 **TraceSwitch** のプロパティは、スイッチの最大トレース レベルを示します。 つまり、指定されたレベルとそれ以下のすべてのレベルについて、トレース情報が書き込まれます。 たとえば、 **TraceInfo** が **true**の場合、 **TraceError** と **TraceWarning** も **true** になりますが、 **TraceVerbose** は **false**になることがあります。  
  
 これらのプロパティは読み取り専用です。 **TraceSwitch** オブジェクトは、 **TraceLevel** プロパティの設定時にこれらのプロパティを自動的に設定します。 例 :  
  
```vb  
Dim myTraceSwitch As New TraceSwitch("SwitchOne", "The first switch")  
myTraceSwitch.Level = TraceLevel.Info  
' This message box displays true, because setting the level to  
' TraceLevel.Info sets all lower levels to true as well.  
MessageBox.Show(myTraceSwitch.TraceWarning.ToString())  
' This messagebox displays false.  
MessageBox.Show(myTraceSwitch.TraceVerbose.ToString())  
```  
  
```csharp  
System.Diagnostics.TraceSwitch myTraceSwitch =   
   new System.Diagnostics.TraceSwitch("SwitchOne", "The first switch");  
myTraceSwitch.Level = System.Diagnostics.TraceLevel.Info;  
// This message box displays true, because setting the level to   
// TraceLevel.Info sets all lower levels to true as well.  
MessageBox.Show(myTraceSwitch.TraceWarning.ToString());  
// This message box displays false.  
MessageBox.Show(myTraceSwitch.TraceVerbose.ToString());  
```  
  
## <a name="developer-defined-switches"></a>開発者が定義するスイッチ  
 **BooleanSwitch** と **TraceSwitch**を指定する以外に、 **Switch** クラスから継承される独自のスイッチを定義して、カスタマイズしたメソッドで基本クラスのメソッドをオーバーライドできます。 開発者定義スイッチの作成方法については、「.NET Framework リファレンス」の「 <xref:System.Diagnostics.Switch> クラス」を参照してください。  
  
## <a name="see-also"></a>参照

- [トレース リスナー](trace-listeners.md)
- [方法 : アプリケーション コードにトレース ステートメントを追加する](how-to-add-trace-statements-to-application-code.md)
- [アプリケーションのトレースとインストルメント](tracing-and-instrumenting-applications.md)
