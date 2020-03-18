---
title: End <keyword> ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.EndDefinition
helpviewer_keywords:
- End keyword [Visual Basic]
ms.assetid: 42d6e088-ab0f-4cda-88e8-fdce3e5fcf4f
ms.openlocfilehash: 87f4724cc036e6e0bdf0b558854a4034f45b9ab5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343733"
---
# <a name="end-keyword-statement-visual-basic"></a>End \<keyword > ステートメント (Visual Basic)

その後に追加のキーワードが続くと、は、そのキーワードによって導入されたステートメントブロックの定義を終了します。

## <a name="syntax"></a>構文

```vb
End AddHandler
End Class
End Enum
End Event
End Function
End Get
End If
End Interface
End Module
End Namespace
End Operator
End Property
End RaiseEvent  
End RemoveHandler  
End Select
End Set
End Structure
End Sub
End SyncLock
End Try
End While
End With  
```  
  
## <a name="parts"></a>指定項目

|要素|説明|
|---|---|
|`End`|必須。 プログラミング要素の定義を終了します。|
|`AddHandler`|カスタムの [Event ステートメント](event-statement.md)で、対応する `AddHandler` ステートメントによって開始された `AddHandler` アクセサを終了する場合に必要です。|
|`Class`|一致する[クラスステートメント](class-statement.md)によって開始されたクラス定義を終了するために必要です。|
|`Enum`|一致する列挙[ステートメント](enum-statement.md)によって開始された列挙型定義を終了するために必要です。|
|`Event`|一致する[イベントステートメント](event-statement.md)によって開始された `Custom` イベント定義を終了するために必要です。|  
|`Function`|一致する[Function ステートメント](function-statement.md)によって開始された `Function` プロシージャ定義を終了するために必要です。 実行時に `End Function` ステートメントが発生すると、呼び出し元のコードに制御が戻ります。|
|`Get`|一致する[Get ステートメント](get-statement.md)によって開始された `Property` プロシージャ定義を終了するために必要です。 実行時に `End Get` ステートメントが発生すると、プロパティの値を要求するステートメントに制御が戻ります。|
|`If`|一致する `If` ステートメントによって開始された `If`...`Then``Else` ブロック定義を終了するために必要です。 参照してください.. [.そうしたら。。。Else ステートメント](if-then-else-statement.md)。|
|`Interface`|一致する[インターフェイスステートメント](interface-statement.md)によって開始されたインターフェイス定義を終了するために必要です。|
|`Module`|一致する[モジュールステートメント](module-statement.md)によって開始されたモジュール定義を終了するために必要です。|
|`Namespace`|一致する [Namespace ステートメント](namespace-statement.md)で開始された名前空間定義を終了するために必要です。|
|`Operator`|一致する[Operator ステートメント](operator-statement.md)によって開始された演算子定義を終了するために必要です。|
|`Property`|一致する[プロパティステートメント](property-statement.md)によって開始されたプロパティ定義を終了するために必要です。|
|`RaiseEvent`|カスタム[イベントステートメント](event-statement.md)内の一致する `RaiseEvent` ステートメントによって開始された `RaiseEvent` アクセサーを終了するために必要です。|
|`RemoveHandler`|カスタム[イベントステートメント](event-statement.md)内の一致する `RemoveHandler` ステートメントによって開始された `RemoveHandler` アクセサーを終了するために必要です。|
|`Select`|一致する `Select` ステートメントによって開始された `Select`...`Case` ブロック定義を終了するために必要です。 参照してください.. [.Case ステートメント](select-case-statement.md)。  
|`Set`|一致する[Set ステートメント](set-statement.md)によって開始される `Property` プロシージャの定義を終了するために必要です。 実行時に `End Set` ステートメントが発生した場合、プロパティの値を設定するステートメントに制御が戻ります。  
|`Structure`|一致する[構造ステートメント](structure-statement.md)によって開始された構造体定義を終了するために必要です。  
|`Sub`|一致する[サブステートメント](sub-statement.md)によって開始された `Sub` プロシージャ定義を終了するために必要です。 実行時に `End Sub` ステートメントが発生すると、呼び出し元のコードに制御が戻ります。  
|`SyncLock`|一致する `SyncLock` ステートメントによって開始された `SyncLock` ブロック定義を終了するために必要です。 「 [SyncLock ステートメント](synclock-statement.md)」を参照してください。  
|`Try`|一致する `Try` ステートメントによって開始された `Try`...`Catch``Finally` ブロック定義を終了するために必要です。 [試してみる...キャッチ...Finally ステートメント](try-catch-finally-statement.md)。  
|`While`|一致する `While` ステートメントによって開始された `While` ループ定義を終了するために必要です。 しばらくお待ちください.. [.End While ステートメント](while-end-while-statement.md)。  
|`With`| 一致する `With` ステートメントによって開始された `With` ブロック定義を終了するために必要です。 参照[してください...End With ステートメント](with-end-with-statement.md)。  
|||
  
## <a name="directives"></a>ディレクティブ

先頭に番号記号 (`#`) がある場合、`End` キーワードは、対応するディレクティブによって導入された前処理ブロックを終了します。  

```vb
#End ExternalSource
#End If
#End Region
```

|要素|説明|
|---|---|
|`#End`|必須。 プリプロセスブロックの定義を終了します。|
|`ExternalSource`|一致する[#ExternalSource ディレクティブ](../directives/externalsource-directive.md)で開始された外部ソースブロックを終了するために必要です。|
|`If`|一致する `#If` ディレクティブで開始された条件付きコンパイルブロックを終了するために必要です。 #If を参照してください.. [.次に、ディレクティブ #Else](../directives/if-then-else-directives.md)ます。|
|`Region`|一致する[#Region ディレクティブ](../directives/region-directive.md)で開始されたソースリージョンブロックを終了するために必要です。|
|||

## <a name="remarks"></a>コメント

追加のキーワードを指定せずに[End ステートメント](end-statement.md)を実行すると、すぐに実行が終了します。

## <a name="smart-device-developer-notes"></a>スマートデバイスの開発者向けメモ  

追加のキーワードを使用しない `End` ステートメントはサポートされていません。  
  
## <a name="see-also"></a>参照

- [End ステートメント](end-statement.md)
