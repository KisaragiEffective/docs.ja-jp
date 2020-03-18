---
title: Using ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.using
helpviewer_keywords:
- resource disposal
- Try...Catch...Finally statements, equivalent to Using statement
- resources [Visual Basic], disposing
- Using statement [Visual Basic]
ms.assetid: 665d1580-dd54-4e96-a9a9-6be2a68948f1
ms.openlocfilehash: 6ec0e228b3898f66f27e322b5db2dd7f3bf3d7d6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352756"
---
# <a name="using-statement-visual-basic"></a>Using ステートメント (Visual Basic)

`Using` ブロックの先頭を宣言し、必要に応じて、ブロックが制御するシステムリソースを取得します。

## <a name="syntax"></a>構文

```vb
Using { resourcelist | resourceexpression }
    [ statements ]
End Using
```

## <a name="parts"></a>指定項目

|用語|Definition|  
|---|---|  
|`resourcelist`|`resourceexpression`を指定しない場合は必須です。 この `Using` ブロックをコンマで区切った1つまたは複数のシステムリソースの一覧。|  
|`resourceexpression`|`resourcelist`を指定しない場合は必須です。 この `Using` ブロックによって制御されるシステムリソースを参照する、参照変数または式。|  
|`statements`|省略可。 `Using` ブロックによって実行されるステートメントのブロック。|  
|`End Using`|必須。 `Using` ブロックの定義を終了し、制御しているすべてのリソースを破棄します。|  

 `resourcelist` パートの各リソースには、次の構文と部分があります。

 `resourcename As New resourcetype [ ( [ arglist ] ) ]`

 または

 `resourcename As resourcetype = resourceexpression`

## <a name="resourcelist-parts"></a>resourcelist パーツ

|用語|Definition|  
|---|---|  
|`resourcename`|必須。 `Using` ブロックで制御されるシステムリソースを参照する参照変数。|  
|`New`|`Using` ステートメントがリソースを取得する場合は必須です。 リソースを既に取得している場合は、2番目の構文を使用します。|  
|`resourcetype`|必須。 リソースのクラス。 クラスは、<xref:System.IDisposable> インターフェイスを実装する必要があります。|  
|`arglist`|省略可。 `resourcetype`のインスタンスを作成するためにコンストラクターに渡す引数のリスト。 「[パラメーターリスト](parameter-list.md)」を参照してください。|  
|`resourceexpression`|必須。 `resourcetype`の要件を満たすシステムリソースを参照する変数または式。 2番目の構文を使用する場合は、`Using` ステートメントに制御を渡す前にリソースを取得する必要があります。|  
  
## <a name="remarks"></a>コメント

 コードには、ファイルハンドル、COM ラッパー、SQL 接続などのアンマネージリソースが必要な場合があります。 `Using` ブロックは、コードが終了したときに、そのようなリソースが確実に破棄されるようにします。 これにより、他のコードで使用できるようになります。

 マネージリソースは、.NET Framework ガベージコレクター (GC) によって破棄されます。その際、追加のコーディングは必要ありません。 マネージリソースに `Using` ブロックは必要ありません。 ただし、`Using` ブロックを使用して、ガベージコレクターを待機するのではなく、マネージリソースを強制的に破棄することもできます。

 `Using` ブロックには、取得、使用、および破棄という3つの部分があります。

- *取得*とは、変数を作成して初期化し、システムリソースを参照することを意味します。 `Using` ステートメントは1つ以上のリソースを取得できます。また、ブロックを入力する前に1つのリソースだけを取得し、それを `Using` ステートメントに指定することもできます。 `resourceexpression`を指定する場合は、`Using` ステートメントに制御を渡す前にリソースを取得する必要があります。

- *使用状況*とは、リソースにアクセスし、それらのリソースでアクションを実行することを意味します。 `Using` と `End Using` 間のステートメントは、リソースの使用状況を表します。

- *破棄*とは、`resourcename`内のオブジェクトに対して <xref:System.IDisposable.Dispose%2A> メソッドを呼び出すことを意味します。 これにより、オブジェクトはリソースを完全に終了できます。 `End Using` ステートメントは、`Using` ブロックのコントロールの下にあるリソースを破棄します。

## <a name="behavior"></a>動作

 `Using` ブロックは `Try`...`Finally` 構築のように動作し、`Try` ブロックはリソースを使用し、`Finally` ブロックはそれらを破棄します。 このため、ブロックの終了方法に関係なく、`Using` ブロックによってリソースが確実に破棄されます。 これは、ハンドルされない例外が発生した場合でも、<xref:System.StackOverflowException>を除き、true になります。

 `Using` ステートメントによって取得されるすべてのリソース変数のスコープは、`Using` ブロックに制限されます。

 `Using` ステートメントで複数のシステムリソースを指定した場合、その効果は、入れ子になった `Using` ブロックを別のでブロックした場合と同じになります。

 `resourcename` が `Nothing`場合、<xref:System.IDisposable.Dispose%2A> の呼び出しは行われず、例外はスローされません。

## <a name="structured-exception-handling-within-a-using-block"></a>Using ブロック内での構造化例外処理

 `Using` ブロック内で発生する可能性のある例外を処理する必要がある場合は、完全な `Try`...`Finally` 構築を追加できます。 `Using` ステートメントがリソースの取得に失敗した場合に対処する必要がある場合は、`resourcename` が `Nothing`かどうかをテストして確認できます。

## <a name="structured-exception-handling-instead-of-a-using-block"></a>Using ブロックの代わりに構造化例外処理

 リソースの取得をより細かく制御する必要がある場合、または `Finally` ブロックに追加のコードが必要な場合は、`Using` ブロックを `Try`...`Finally` の構築として書き換えることができます。 次の例は、`resource`の取得と破棄に相当するスケルトン `Try` と `Using` の構造を示しています。

```vb
Using resource As New resourceType
    ' Insert code to work with resource.
End Using

' For the acquisition and disposal of resource, the following  
' Try construction is equivalent to the Using block.
Dim resource As New resourceType
Try
    ' Insert code to work with resource.
Finally
    If resource IsNot Nothing Then
        resource.Dispose()
    End If
End Try
```

> [!NOTE]
> `Using` ブロック内のコードでは、`resourcename` 内のオブジェクトを別の変数に割り当てることはできません。 `Using` ブロックを終了すると、リソースが破棄され、他の変数はそのリソースが指すリソースにアクセスできなくなります。

## <a name="example"></a>例

 次の例では、test.txt という名前のファイルを作成し、ファイルに2行のテキストを書き込みます。 この例でも同じファイルが読み取られ、テキスト行が表示されます。

 <xref:System.IO.TextWriter> クラスと <xref:System.IO.TextReader> クラスには <xref:System.IDisposable> インターフェイスが実装されているため、コードでは `Using` ステートメントを使用して、書き込み操作と読み取り操作の後でファイルが正しく閉じられるようにすることができます。

 [!code-vb[VbVbalrStatements#50](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#50)]

## <a name="see-also"></a>参照

- <xref:System.IDisposable>
- [Try...Catch...Finally ステートメント](try-catch-finally-statement.md)
- [方法 : システム リソースを破棄する](../../programming-guide/language-features/control-flow/how-to-dispose-of-a-system-resource.md)
