---
title: .NET での I/O エラーの処理
ms.date: 08/27/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O, exception handling
- I/O, errors
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: 51eb0e758f1ae8fb41c842ef9b32a9f8928af9ac
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73120742"
---
# <a name="handling-io-errors-in-net"></a>.NET での I/O エラーの処理

任意のメソッド呼び出しでスローできる例外 (システムに負荷がかかっているときの <xref:System.OutOfMemoryException> やプログラム エラーによる <xref:System.NullReferenceException> など) に加え、.NET ファイル システムのメソッドは、次の例外をスローできます。

- <xref:System.IO.IOException?displayProperty=nameWithType>。すべての <xref:System.IO> 例外の種類の基底クラス。 これは、オペレーティング システムからのリターン コードが他の例外の種類に直接マップされないエラーに対してスローされます。
- [https://login.microsoftonline.com/consumers/](<xref:System.IO.FileNotFoundException?displayProperty=nameWithType>)
- [https://login.microsoftonline.com/consumers/](<xref:System.IO.DirectoryNotFoundException?displayProperty=nameWithType>)
- [https://login.microsoftonline.com/consumers/](<xref:System.IO.DriveNotFoundException??displayProperty=nameWithType>)
- [https://login.microsoftonline.com/consumers/](<xref:System.IO.PathTooLongException?displayProperty=nameWithType>)
- [https://login.microsoftonline.com/consumers/](<xref:System.OperationCanceledException?displayProperty=nameWithType>)
- [https://login.microsoftonline.com/consumers/](<xref:System.UnauthorizedAccessException?displayProperty=nameWithType>)
- <xref:System.ArgumentException?displayProperty=nameWithType>、.NET Framework と .NET Core 2.0 以前のバージョンで、無効なパス文字に対してスローされます。
- <xref:System.NotSupportedException?displayProperty=nameWithType>、.NET Framework で無効なコロンに対してスローされます。
- <xref:System.Security.SecurityException?displayProperty=nameWithType>、.NET Framework で必要な権限が欠けている限定的な信頼で実行されているアプリケーションに対してスローされます (完全な信頼は .NET Framework の既定の設定です)。

## <a name="mapping-error-codes-to-exceptions"></a>エラー コードの例外へのマッピング

ファイル システムは、オペレーティング システムのリソースであるため、.NET Core と .NET Framework の両方の I/O メソッドは、基になるオペレーティング システムに呼び出しをラップします。 オペレーティング システムによって実行されるコードで I/O エラーが発生すると、オペレーティング システムは、.NET の I/O メソッドにエラー情報を返します。 その後、メソッドがエラー情報を .NET 例外の種類に変換します。通常はエラー コードの形式に変換されます。 ほとんどの場合、これは、エラー コードを対応する例外の種類に直接変換することによって実行され、メソッド呼び出しのコンテキストに基づく特別なエラーのマッピングは行われません。

たとえば、Windows オペレーティング システムでは、エラー コード`ERROR_FILE_NOT_FOUND` (または 0x02) を返すメソッドの呼び出しは <xref:System.IO.FileNotFoundException> にマップされ、エラー コード `ERROR_PATH_NOT_FOUND` (または 0x03) は <xref:System.IO.DirectoryNotFoundException> にマップされます。

ただし、オペレーティング システムが特定のエラー コードが返す正確な条件は、多くの場合、文書化されていないか、適切に文書化されていません。 その結果、予期しない例外が発生する可能性があります。 たとえば、ファイルではなくディレクトリを操作している場合、無効なディレクトリ パスを <xref:System.IO.DirectoryInfo.%23ctor%2A?displayProperty=nameWithType> に提供すると、<xref:System.IO.DirectoryNotFoundException> コンストラクターがスローされることが想定されます。 ただし、<xref:System.IO.FileNotFoundException> がスローされる場合もあります。

## <a name="exception-handling-in-io-operations"></a>I/O 操作の例外処理

オペレーティング システムへのこの依存性によって、同じ例外条件 (例に示したディレクトリが見つからないというエラーなど) で、I/O メソッドが I/O 例外クラス全体のいずれかをスルーする可能性があります。 これは、I/O API を呼び出すときは、次の表に示すように、これらの例外のほとんどまたはすべてを処理するようにコードを準備する必要があることを意味します。

| 例外の種類 | .NET Core | .NET Framework |
|---|---|---|
| <xref:System.IO.IOException> | はい | はい |
| <xref:System.IO.FileNotFoundException> | はい | はい |
| <xref:System.IO.DirectoryNotFoundException> | はい | はい |
| <xref:System.IO.DriveNotFoundException?> | はい | はい |
| <xref:System.IO.PathTooLongException> | はい | はい |
| <xref:System.OperationCanceledException> | はい | はい |
| <xref:System.UnauthorizedAccessException> | はい | はい |
| <xref:System.ArgumentException> | .NET Core 2.0 以前| はい |
| <xref:System.NotSupportedException> | いいえ | はい |
| <xref:System.Security.SecurityException> | いいえ | 限定的な信頼のみ |

## <a name="handling-ioexception"></a>IOException の処理

<xref:System.IO> 名前空間内の例外の基底クラスとして、定義済みの例外の種類にマップされないすべてのエラー コードに対して <xref:System.IO.IOException> もスローされます。 つまり、すべての I/O 操作によってスローされる可能性があります。

> [!IMPORTANT]
> <xref:System.IO.IOException> は、<xref:System.IO> 名前空間内の他の例外の種類の基底クラスであるため、他の I/O 関連例外を処理した後で `catch` ブロック内で処理する必要があります。

さらに、.NET Core 2.1 以降、パスの正確性の検証チェック (たとえばパス内に無効な文字が存在しないことを確認するためのチェック) が削除されており、ランタイムでは、独自の検証コードではなく、オペレーティング システムのエラー コードからマップされた例外がスローされます。 この場合、最もスローされる可能性が高い例外は <xref:System.IO.IOException> ですが、その他の種類の例外もスローされる可能性があります。

例外処理コードでは、<xref:System.IO.IOException> は常に最後に処理する必要があることに注意してください。 それ以外の場合、それは他のすべての IO 例外の基底クラスであるため、派生クラスの catch ブロックは評価されません。

<xref:System.IO.IOException> の場合、[IOException.HResult](xref:System.Exception.HResult) プロパティから追加のエラー情報を取得できます。 HResult 値を Win32 エラー コードに変換するには、32 ビット値の上位 16 ビットを削除します。 次の表に、<xref:System.IO.IOException> にラップされる可能性があるエラー コードの一覧を示します。

| HResult | 定数 | [説明] |
| --- | --- | --- |
| ERROR_SHARING_VIOLATION | 32 | ファイル名が存在しないか、ファイルまたはディレクトリが使用中です。 |
| ERROR_FILE_EXISTS | 80 | ファイルは既に存在します。 |
| ERROR_INVALID_PARAMETER | 87 | メソッドに指定された引数が無効です。 |
| ERROR_ALREADY_EXISTS | 183 | ファイルまたはディレクトリは既に存在します。 |

catch ステートメントの `When` 句を使用してこれらを処理できます。次に例を示します。

[!code-csharp[io-exception-handling](~/samples/snippets/standard/io/io-exceptions/cs/io-exceptions.cs)]
[!code-vb[io-exception-handling](~/samples/snippets/standard/io/io-exceptions/vb/io-exceptions.vb)]

## <a name="see-also"></a>参照

- [.NET での例外の処理とスロー](../exceptions/index.md)
- [例外処理 (タスク並列ライブラリ)](../parallel-programming/exception-handling-task-parallel-library.md)
- [例外のベスト プラクティス](../exceptions/best-practices-for-exceptions.md)
- [catch ブロックで特定の例外を使用する方法](../exceptions/how-to-use-specific-exceptions-in-a-catch-block.md)
