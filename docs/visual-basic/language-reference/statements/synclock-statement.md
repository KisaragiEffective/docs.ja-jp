---
title: SyncLock ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.SyncLock
- SyncLock
helpviewer_keywords:
- threading [Visual Basic], locks
- SyncLock statement [Visual Basic]
- locks, threads
ms.assetid: 14501703-298f-4d43-b139-c4b6366af176
ms.openlocfilehash: 0f430edce99513b0de9ef437d70648a128b336b8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352818"
---
# <a name="synclock-statement"></a>SyncLock ステートメント
ブロックを実行する前に、ステートメントブロックの排他ロックを取得します。  
  
## <a name="syntax"></a>構文  
  
```vb  
SyncLock lockobject  
    [ block ]  
End SyncLock  
```  
  
## <a name="parts"></a>指定項目  
 `lockobject`  
 必須。 オブジェクト参照に評価される式。  
  
 `block`  
 任意。 ロックが取得されたときに実行されるステートメントのブロック。  
  
 `End SyncLock`  
 `SyncLock` ブロックを終了します。  
  
## <a name="remarks"></a>コメント  
 `SyncLock` ステートメントを実行すると、複数のスレッドが同時にステートメントブロックを実行しなくなります。 `SyncLock` は、他のスレッドが実行しないように、各スレッドがブロックに入るのを防ぎます。  
  
 `SyncLock` を使用する最も一般的な方法は、複数のスレッドによってデータが同時に更新されないように保護することです。 データを操作するステートメントが中断せずに完了する必要がある場合は、`SyncLock` ブロック内に配置します。  
  
 排他ロックによって保護されているステートメントブロックは、*クリティカルセクション*と呼ばれることがあります。  
  
## <a name="rules"></a>ルール  
  
- 分岐. ブロックの外側から `SyncLock` ブロックに分岐することはできません。  
  
- オブジェクト値をロックします。 `lockobject` の値を `Nothing`することはできません。 `SyncLock` ステートメントで使用する前に、lock オブジェクトを作成する必要があります。  
  
     `SyncLock` ブロックの実行中に `lockobject` の値を変更することはできません。 このメカニズムでは、ロックオブジェクトが変更されないようにする必要があります。  
  
- `SyncLock` ブロックで[Await](../../../visual-basic/language-reference/operators/await-operator.md)演算子を使用することはできません。  
  
## <a name="behavior"></a>動作  
  
- しくみ. スレッドが `SyncLock` ステートメントに達すると、`lockobject` 式が評価され、式によって返されたオブジェクトの排他ロックを取得するまで実行が中断されます。 別のスレッドが `SyncLock` ステートメントに到達すると、最初のスレッドが `End SyncLock` ステートメントを実行するまでロックは取得されません。  
  
- 保護されたデータ。 `lockobject` が `Shared` 変数の場合、排他ロックによって、クラスのインスタンス内のスレッドは、他のスレッドが実行している間に `SyncLock` ブロックを実行できなくなります。 これにより、すべてのインスタンス間で共有されているデータが保護されます。  
  
     `lockobject` がインスタンス変数 (`Shared`ではありません) の場合、ロックは、現在のインスタンスで実行されているスレッドが、同じインスタンス内の別のスレッドと同時に `SyncLock` ブロックを実行できないようにします。 これにより、個々のインスタンスによって保持されるデータが保護されます。  
  
- 取得と解放。 `SyncLock` ブロックは、`Try` ブロックが `lockobject` に対して排他ロックを取得する `Try...Finally` の構築と同様に動作し、`Finally` ブロックによって解放されます。 このため、`SyncLock` ブロックは、ブロックを終了する方法に関係なく、ロックの解放を保証します。 これは、ハンドルされない例外が発生した場合でも当てはまります。  
  
- フレームワークの呼び出し。 `SyncLock` ブロックは、<xref:System.Threading> 名前空間の `Monitor` クラスの `Enter` および `Exit` メソッドを呼び出すことによって、排他ロックを取得し、解放します。  
  
## <a name="programming-practices"></a>プログラミングプラクティス  
 `lockobject` 式は、常に、クラスにのみ属しているオブジェクトに評価される必要があります。 現在のインスタンスに属するデータを保護するには `Private` オブジェクト変数を宣言し、すべてのインスタンスに共通のデータを保護するには `Private Shared` オブジェクト変数を宣言する必要があります。  
  
 `Me` キーワードを使用して、インスタンスデータ用のロックオブジェクトを指定しないでください。 クラスの外部のコードにクラスのインスタンスへの参照が含まれている場合、その参照を `SyncLock` ブロックのロックオブジェクトとして使用して、さまざまなデータを保護することができます。 このようにして、クラスとその他のクラスは、互いに関連付けられていない `SyncLock` ブロックの実行をブロックできます。 同様に、同じ文字列を使用するプロセス内の他のコードは同じロックを共有するため、文字列のロックが問題になることがあります。  
  
 また、`Me.GetType` メソッドを使用して、共有データのロックオブジェクトを指定することもできません。 これは、`GetType` は常に、指定されたクラス名に対して同じ `Type` オブジェクトを返すためです。 外部コードは、クラスで `GetType` を呼び出し、使用しているのと同じロックオブジェクトを取得することができます。 これにより、2つのクラスが `SyncLock` ブロックから相互にブロックされることになります。  
  
## <a name="examples"></a>使用例  
  
### <a name="description"></a>説明  
 次の例は、メッセージの単純なリストを保持するクラスを示しています。 このメソッドは、配列内のメッセージと、その配列の最後に使用された要素を変数に保持します。 `addAnotherMessage` プロシージャは、最後の要素をインクリメントし、新しいメッセージを格納します。 これら2つの操作は、`SyncLock` および `End SyncLock` ステートメントによって保護されます。最後の要素がインクリメントされると、他のすべてのスレッドが最後の要素を再度インクリメントできるようになる前に、新しいメッセージが格納される必要があります。  
  
 `simpleMessageList` クラスがすべてのインスタンス間で1つのメッセージリストを共有している場合、`messagesList` と `messagesLast` の変数は `Shared`として宣言されます。 この場合、すべてのインスタンスで1つのロックオブジェクトが使用されるように、変数 `messagesLock` も `Shared`する必要があります。  
  
### <a name="code"></a>コード  
 [!code-vb[VbVbalrThreading#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrThreading/VB/Class1.vb#1)]  
  
### <a name="description"></a>説明  
 次の例では、スレッドと `SyncLock`を使用します。 `SyncLock` ステートメントが存在する限り、ステートメントブロックはクリティカルセクションであり `balance` 負の値になることはありません。 `SyncLock` と `End SyncLock` ステートメントをコメントアウトして、`SyncLock` キーワードを残すことによる影響を確認できます。  
  
### <a name="code"></a>コード  
 [!code-vb[VbVbalrThreading#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrThreading/VB/class2.vb#21)]  
  
### <a name="comments"></a>コメント  
  
## <a name="see-also"></a>関連項目

- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- <xref:System.Threading.Interlocked?displayProperty=nameWithType>
- [同期プリミティブの概要](../../../standard/threading/overview-of-synchronization-primitives.md)
