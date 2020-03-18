---
title: 呼び出し元情報
ms.date: 07/20/2015
ms.assetid: 15d556eb-4d0c-4497-98a3-7f60abb7d6a1
ms.openlocfilehash: 7c87b540a68f4d0219918fed66de6c1b635104a9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349475"
---
# <a name="caller-information-visual-basic"></a>呼び出し元情報 (Visual Basic)
呼び出し元情報の属性を使用すると、メソッドへの呼び出し元に関する情報を取得できます。 ソース コードのファイル パス、ソース コードの行番号、および呼び出し元のメンバー名を取得できます。 この情報は、トレース、デバッグ、および診断ツールの作成に役立ちます。  
  
 この情報を取得するには、省略可能なパラメーターに適用される属性を使用します。各パラメーターには既定値があります。 次の表は、<xref:System.Runtime.CompilerServices?displayProperty=nameWithType> 名前空間で定義されている呼び出し元情報の属性の一覧です。  
  
|属性|説明|種類|  
|---|---|---|  
|<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>|呼び出し元を含むソース ファイルのフル パスです。 これは、コンパイル時のファイル パスです。|`String`|  
|<xref:System.Runtime.CompilerServices.CallerLineNumberAttribute>|メソッドが呼び出されたソース ファイルの行番号。|`Integer`|  
|<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>|呼び出し元のメソッド名またはプロパティ名。 後で説明する「[メンバー名](#MEMBERNAMES)」を参照してください。|`String`|  
  
## <a name="example"></a>例  
 呼び出し元情報の属性を使用する方法の例を次に示します。 `TraceMessage` メソッドを呼び出すたびに、呼び出し元情報は、省略可能なパラメーターの引数として設定されます。  
  
```vb  
Private Sub DoProcessing()  
    TraceMessage("Something happened.")  
End Sub  
  
Public Sub TraceMessage(message As String,  
        <System.Runtime.CompilerServices.CallerMemberName> Optional memberName As String = Nothing,  
        <System.Runtime.CompilerServices.CallerFilePath> Optional sourcefilePath As String = Nothing,  
        <System.Runtime.CompilerServices.CallerLineNumber()> Optional sourceLineNumber As Integer = 0)  
  
    System.Diagnostics.Trace.WriteLine("message: " & message)  
    System.Diagnostics.Trace.WriteLine("member name: " & memberName)  
    System.Diagnostics.Trace.WriteLine("source file path: " & sourcefilePath)  
    System.Diagnostics.Trace.WriteLine("source line number: " & sourceLineNumber)  
End Sub  
  
' Sample output:  
'   message: Something happened.  
'   member name: DoProcessing  
'   source file path: C:\Users\username\Documents\Visual Studio 2012\Projects\CallerInfoVB\CallerInfoVB\Form1.vb  
'   source line number: 15  
```  
  
## <a name="remarks"></a>コメント  
 省略可能な各パラメーターには、明示的な既定値を指定する必要があります。 省略可能と指定されていないパラメーターには、呼び出し元情報の属性を適用できません。  
  
 呼び出し元情報の属性によってパラメーターが省略可能になるわけではありません。 この属性は、引数を省略したときに渡される既定値に影響します。  
  
 呼び出し元情報の値は、コンパイル時に中間言語 (IL) 内にリテラルとして出力されます。 例外の <xref:System.Exception.StackTrace%2A> プロパティの結果とは異なり、難読化による影響は受けません。  
  
 省略可能な引数を明示的に指定して、呼び出し元情報を制御したり、非表示にしたりできます。  
  
### <a name="MEMBERNAMES"></a>メンバー名  
 `CallerMemberName` 属性を使用して、呼び出されたメソッドにメンバー名を `String` 引数として指定することを回避できます。 この方法を使用すると、**リファクタリングの名前の変更**で `String` 値が変更されないという問題が発生しなくなります。 この利点は、次のタスクで役立ちます。  
  
- トレース ルーチンと診断ルーチンの使用。  
  
- データ バインディング時の <xref:System.ComponentModel.INotifyPropertyChanged> インターフェイスの実装。 このインターフェイスを使用すると、オブジェクトのプロパティが、プロパティが変更されたことをデータ バインド コントロールに通知できます。これによって、このコントロールは、更新された情報を表示できます。 `CallerMemberName` 属性がない場合は、リテラルとしてプロパティ名を指定する必要があります。  
  
 次のグラフは、`CallerMemberName` 属性の使用時に返されるメンバー名を示します。  
  
|呼び出しは、次のものの中で発生します。|メンバー名の結果|  
|-------------------------|------------------------|  
|メソッド、プロパティ、またはイベント|呼び出しが発生したメソッド、プロパティ、またはイベントの名前。|  
|コンストラクター|文字列「.ctor」|  
|静的コンストラクター|文字列「.cctor」|  
|デストラクター|文字列「Finalize」|  
|ユーザー定義の演算子または変換|生成されたメンバー名 (「op_Addition」など)。|  
|Attribute コンストラクター|属性が適用されるメンバーの名前。 属性がメンバー内の要素 (パラメーター、戻り値、ジェネリック型パラメーターなど) である場合、この結果は、その要素に関連付けられているメンバーの名前になります。|  
|含んでいないメンバー (型に適用されているアセンブリ レベルや属性など)|省略可能なパラメーターの既定値。|  
  
## <a name="see-also"></a>参照

- [属性 (Visual Basic)](../../../visual-basic/language-reference/attributes.md)
- [一般的な属性 (Visual Basic)](../../../visual-basic/programming-guide/concepts/attributes/common-attributes.md)
- [省略可能なパラメーター](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)
- [プログラミングの概念 (Visual Basic)](../../../visual-basic/programming-guide/concepts/index.md)
