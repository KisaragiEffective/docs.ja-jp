---
title: 呼び出し元情報
description: 呼び出し元情報の引数属性を使用して、メソッドから呼び出し元情報を取得する方法について説明します。
ms.date: 11/04/2019
ms.openlocfilehash: d995b37149277b7c7d1b6217ee484d3c90a7f8b3
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976796"
---
# <a name="caller-information"></a>呼び出し元情報

呼び出し元情報の属性を使用すると、メソッドへの呼び出し元に関する情報を取得できます。 ソース コードのファイル パス、ソース コードの行番号、および呼び出し元のメンバー名を取得できます。 この情報は、トレース、デバッグ、および診断ツールの作成に役立ちます。

この情報を取得するには、省略可能なパラメーターに適用される属性を使用します。各パラメーターには既定値があります。 次の表に、 [system.runtime.compilerservices](/dotnet/api/system.runtime.compilerservices)名前空間で定義されている呼び出し元情報の属性を示します。

|属性|説明|[種類]|
|---------|-----------|----|
|[CallerFilePath](/dotnet/api/system.runtime.compilerservices.callerfilepathattribute)|呼び出し元を含むソース ファイルのフル パスです。 これは、コンパイル時のファイル パスです。|`String`
|[CallerLineNumber](/dotnet/api/system.runtime.compilerservices.callerlinenumberattribute)|メソッドが呼び出されたソース ファイルの行番号。|`Integer`|
|[CallerMemberName](/dotnet/api/system.runtime.compilerservices.callermembernameattribute)|呼び出し元のメソッド名またはプロパティ名。 このトピックで後述する「メンバー名」を参照してください。|`String`|

## <a name="example"></a>例

次の例は、これらの属性を使用して呼び出し元をトレースする方法を示しています。

```fsharp
open System.Diagnostics
open System.Runtime.CompilerServices
open System.Runtime.InteropServices

type Tracer() =
    member _.DoTrace(message: string,
                      [<CallerMemberName; Optional; DefaultParameterValue("")>] memberName: string,
                      [<CallerFilePath; Optional; DefaultParameterValue("")>] path: string,
                      [<CallerLineNumber; Optional; DefaultParameterValue(0)>] line: int) =
        Trace.WriteLine(sprintf "Message: %s" message)
        Trace.WriteLine(sprintf "Member name: %s" memberName)
        Trace.WriteLine(sprintf "Source file path: %s" path)
        Trace.WriteLine(sprintf "Source line number: %d" line)
```

## <a name="remarks"></a>Remarks

呼び出し元情報の属性は、省略可能なパラメーターにのみ適用できます。 呼び出し元情報属性により、コンパイラは、呼び出し元情報属性で修飾された省略可能な各パラメーターに対して適切な値を書き込みます。

呼び出し元情報の値は、コンパイル時に中間言語 (IL) 内にリテラルとして出力されます。 例外の[StackTrace](/dotnet/api/system.diagnostics.stacktrace)プロパティの結果とは異なり、結果は難読化の影響を受けません。

省略可能な引数を明示的に指定して、呼び出し元情報を制御したり、非表示にしたりできます。

## <a name="member-names"></a>メンバー名

[`CallerMemberName`](/dotnet/api/system.runtime.compilerservices.callermembernameattribute)属性を使用して、呼び出されたメソッドの `String` 引数としてメンバー名を指定しないようにすることができます。 この手法を使用すると、リファクタリングの名前変更によって `String` 値が変更されないという問題を回避できます。 この利点は、次のタスクで役立ちます。

- トレース ルーチンと診断ルーチンの使用。
- データをバインドするときに[INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged)インターフェイスを実装します。 このインターフェイスを使用すると、オブジェクトのプロパティが、プロパティが変更されたことをデータ バインド コントロールに通知できます。これによって、このコントロールは、更新された情報を表示できます。 [`CallerMemberName`](/dotnet/api/system.runtime.compilerservices.callermembernameattribute)属性を指定しない場合は、プロパティ名をリテラルとして指定する必要があります。

次のグラフは、CallerMemberName 属性を使用したときに返されるメンバー名を示しています。

|呼び出しは、次のものの中で発生します。|メンバー名の結果|
|-------------------|------------------|
|メソッド、プロパティ、またはイベント|呼び出しが発生したメソッド、プロパティ、またはイベントの名前。|
|コンストラクター|文字列「.ctor」|
|静的コンストラクター|文字列「.cctor」|
|デストラクターです。|文字列「Finalize」|
|ユーザー定義の演算子または変換|生成されたメンバー名 (「op_Addition」など)。|
|Attribute コンストラクター|属性が適用されるメンバーの名前。 属性がメンバー内の要素 (パラメーター、戻り値、ジェネリック型パラメーターなど) である場合、この結果は、その要素に関連付けられているメンバーの名前になります。|
|含んでいないメンバー (型に適用されているアセンブリ レベルや属性など)|省略可能なパラメーターの既定値。|

## <a name="see-also"></a>関連項目

- [属性](attributes.md)
- [名前付き引数](parameters-and-arguments.md#named-arguments)
- [省略可能なパラメーター](parameters-and-arguments.md#optional-parameters)
