---
title: Parallel.ForEach を使用して単純な並列プログラムを記述する
ms.date: 02/14/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- foreach, parallel version
- parallel programming, foreach
ms.assetid: cb5fab92-1c19-499e-ae91-8b7525dd875f
ms.openlocfilehash: 02b94b673dc4468e68a1dadd83aab0e3bfcfaa16
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160301"
---
# <a name="how-to-write-a-simple-parallelforeach-loop"></a>方法: 単純な Parallel.ForEach ループを記述する

この例は、<xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> ループを使用して、<xref:System.Collections.IEnumerable?displayProperty=nameWithType> または <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> データ ソースでのデータの並列処理を有効にする方法を示しています。

> [!NOTE]
> ここでは、ラムダ式を使用して PLINQ でデリゲートを定義します。 C# または Visual Basic のラムダ式についての情報が必要な場合は、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。

## <a name="example"></a>例

この例では、*C:\Users\Public\Pictures\Sample Pictures* フォルダーにいくつかの .jpg ファイルがあることを想定し、*Modified* という名前の新しいサブフォルダーを作成します。 例を実行すると、*Sample Pictures* のそれぞれの .jpg 画像が回転して、*Modified* に保存されます。 2 つのパスは必要に応じて変更することができます。

[!code-csharp[TPL_Parallel#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/simpleforeach.cs#03)]
[!code-vb[TPL_Parallel#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/simpleforeach.vb#03)]

<xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> ループは <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> ループのように動作します。 ループによってソース コレクションがパーティション分割され、作業はシステム環境に基づいて複数のスレッドでスケジューリングされます。 システムのプロセッサの数が多いほど、並列メソッドの実行が速くなります。 一部のソース コレクションでは、ソースのサイズやループで実行される作業の種類に応じて、順次ループがより高速になる可能性があります。 パフォーマンスの詳細については、「[データとタスクの並列化における注意点](potential-pitfalls-in-data-and-task-parallelism.md)」を参照してください。

並列ループの詳細については、「[方法: 単純な Parallel.For ループを記述する](../../../docs/standard/parallel-programming/how-to-write-a-simple-parallel-for-loop.md)」を参照してください。

非ジェネリック コレクションで <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> を使用する場合は、次の例に示すように、<xref:System.Linq.Enumerable.Cast%2A?displayProperty=nameWithType> 拡張メソッドを使用して、コレクションをジェネリック コレクションに変換することができます。

[!code-csharp[TPL_Parallel#07](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/nongeneric.cs#07)]
[!code-vb[TPL_Parallel#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/nongeneric.vb#07)]

また、Parallel LINQ (PLINQ) を使用して、<xref:System.Collections.Generic.IEnumerable%601> データ ソースの処理を並列化することもできます。 PLINQ では、宣言型のクエリ構文を使用して、ループの動作を表すことができます。 詳細については、「[Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)」を参照してください。

## <a name="compile-and-run-the-code"></a>コードをコンパイルして実行する

コードは .NET Framework のコンソール アプリケーションまたは .NET Core のコンソール アプリケーションとしてコンパイルできます。

Visual Studio には、Windows デスクトップと .NET Core 向けに、Visual Basic と C# のコンソール アプリケーション テンプレートがあります。

コマンド ラインから .NET Core CLI コマンド (`dotnet new console` や `dotnet new console -lang vb` など) を使用するか、またはファイルを作成して .NET Framework アプリケーション用のコマンド ライン コンパイラを使用することができます。

.NET Core プロジェクトの場合は、**System.Drawing.Common** NuGet パッケージを参照する必要があります。 Visual Studio ではパッケージのインストールに NuGet パッケージ マネージャーを使用します。 または、\*.csproj または \*.vbproj ファイルにパッケージへの参照を追加することもできます。

```xml
<ItemGroup>
     <PackageReference Include="System.Drawing.Common" Version="4.5.1" />
</ItemGroup>
```

コマンド ラインから .NET Core コンソール アプリケーションを実行するには、アプリケーションが含まれるフォルダーから `dotnet run` を使用します。

Visual Studio からコンソール アプリケーションを実行するには、**F5** キーを押します。

## <a name="see-also"></a>関連項目

- [データの並列化](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)
- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
