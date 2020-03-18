---
title: '方法: .NET Framework ストリームと Windows ランタイム ストリームの間で変換を行う (Windows のみ)'
ms.date: 01/14/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 23a763ea-8348-4244-9f8c-a4280b870b47
ms.openlocfilehash: 7413c3fae7d7189ec8dca43b0c77f6b56158f416
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159469"
---
# <a name="how-to-convert-between-net-framework-and-windows-runtime-streams-windows-only"></a>方法: .NET Framework ストリームと Windows ランタイム ストリームの間で変換を行う (Windows のみ)

UWP アプリ用 .NET Framework は、完全な .NET Framework のサブセットです。 UWP アプリのセキュリティおよびその他の要件のために、ファイルを開いたり読み取ったりするために使用する .NET Framework API の完全なセットを使用できません。 詳細については、「[.NET for UWP apps overview](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))」(UWP アプリ用 .NET の概要) を参照してください。 ただし、他のストリームの処理操作のために .NET Framework API を使用することもできます。 これらのストリームを操作するには、<xref:System.IO.MemoryStream> や <xref:System.IO.FileStream> などの .NET Framework ストリーム型と、<xref:Windows.Storage.Streams.IInputStream>、<xref:Windows.Storage.Streams.IOutputStream>、<xref:Windows.Storage.Streams.IRandomAccessStream> などの Windows ランタイム ストリームの間で変換できます。

<xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=nameWithType> クラスには、これらの変換を容易にするメソッドが含まれています。 ただし、次のセクションで説明するように、.NET Framework と Windows ランタイムのストリームの間にある根本的な違いにより、これらのメソッドの使用結果に影響があります。

## <a name="convert-from-a-windows-runtime-to-a-net-framework-stream"></a>Windows ランタイムから .NET Framework ストリームに変換する
Windows ランタイム ストリームから .NET Framework ストリームに変換するには、次の <xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=nameWithType> メソッドのいずれかを使用します。

- <xref:System.IO.WindowsRuntimeStreamExtensions.AsStream%2A?displayProperty=nameWithType> は、Windows ランタイムのランダム アクセス ストリームを UWP アプリ用 .NET のマネージド ストリームに変換します。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsStreamForWrite%2A?displayProperty=nameWithType> は、Windows ランタイムの出力ストリームを UWP アプリ用 .NET のマネージド ストリームに変換します。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsStreamForRead%2A?displayProperty=nameWithType> は、Windows ランタイムの入力ストリームを UWP アプリ用 .NET のマネージド ストリームに変換します。

Windows ランタイムでは、読み取り専用、書き込み専用、または読み取りと書き込みをサポートするストリームの種類が提供されています。 Windows ランタイム ストリームを .NET Framework ストリームに変換するとき、これらの機能は維持されます。 さらに、Windows ランタイム ストリームを .NET Framework ストリームに変換してから元に戻すと、元の Windows ランタイム インスタンスに戻ります。

変換する Windows ランタイム ストリームの機能と一致する変換メソッドを使用することをお勧めします。 ただし、<xref:Windows.Storage.Streams.IRandomAccessStream> は読み取りも書き込みも可能なので (<xref:Windows.Storage.Streams.IOutputStream> と <xref:Windows.Storage.Streams.IInputStream> の両方を実装しているので)、変換メソッドでは元のストリームの機能が維持されます。 たとえば、<xref:System.IO.WindowsRuntimeStreamExtensions.AsStreamForRead%2A?displayProperty=nameWithType> を使用して <xref:Windows.Storage.Streams.IRandomAccessStream> を変換しても、変換された .NET Framework ストリームが読み取り可能に制限されるわけではありません。 書き込みも可能です。

## <a name="example-convert-windows-runtime-random-access-to-net-framework-stream"></a>例:Windows ランタイム ランダム アクセスを、.NET Framework ストリームに変換する
Windows ランタイム ランダム アクセス ストリームから .NET Framework ストリームに変換するには、<xref:System.IO.WindowsRuntimeStreamExtensions.AsStream%2A?displayProperty=nameWithType> メソッドを使用します。

次のコード例では、ファイルを選択するように求められ、そのファイルが Windows ランタイム API で開かれた後、.NET Framework ストリームに変換されます。 ストリームを読み取って、テキスト ブロックに出力します。 通常、結果を出力する前に .NET Framework API でストリームを処理します。

この例を実行するには、`TextBlock1` という名前のテキスト ブロックと `Button1` という名前のボタンを含む UWP XAML アプリを作成します。 ボタン クリック イベントを、例で示されている `button1_Click` メソッドに関連付けます。

  [!code-csharp[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/cs/mainpage1.xaml.cs)]
  [!code-vb[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/vb/mainpage1.xaml.vb)]

## <a name="convert-from-a-net-framework-to-a-windows-runtime-stream"></a>.NET Framework から Windows ランタイム ストリームに変換する
.NET Framework ストリームから Windows ランタイム ストリームに変換するには、次の <xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=nameWithType> メソッドのいずれかを使用します。

- <xref:System.IO.WindowsRuntimeStreamExtensions.AsInputStream%2A?displayProperty=nameWithType> は、UWP アプリ用 .NET のマネージド ストリームを Windows ランタイムの入力ストリームに変換します。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsOutputStream%2A?displayProperty=nameWithType> は、UWP アプリ用 .NET のマネージド ストリームを Windows ランタイムの出力ストリームに変換します。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsRandomAccessStream%2A?displayProperty=nameWithType> では、UWP アプリ用 .NET のマネージド ストリームが、Windows ランタイムで読み取りまたは書き込みに使用できるランダム アクセス ストリームに変換されます。

.NET Framework ストリームを Windows ランタイム ストリームに変換する場合、変換されたストリームの機能は元のストリームによって異なります。 たとえば、元のストリームが読み取りと書き込みの両方をサポートしており、<xref:System.IO.WindowsRuntimeStreamExtensions.AsInputStream%2A?displayProperty=nameWithType> を呼び出してそのストリームを変換した場合、返される型は `IRandomAccessStream` になります。 `IRandomAccessStream` では `IInputStream` と `IOutputStream` が実装されており、読み取りと書き込みがサポートされます。

.NET Framework ストリームでは、変換後も複製はサポートされません。 .NET Framework ストリームを Windows ランタイム ストリームに変換し、<xref:Windows.Storage.Streams.RandomAccessStreamOverStream.CloneStream%2A> を呼び出す <xref:Windows.Storage.Streams.InMemoryRandomAccessStream.GetInputStreamAt%2A> または <xref:Windows.Storage.Streams.IRandomAccessStream.GetOutputStreamAt%2A> を呼び出した場合、または <xref:Windows.Storage.Streams.RandomAccessStreamOverStream.CloneStream%2A> を直接呼び出した場合は、例外が発生します。

## <a name="example-convert-net-framework-to-windows-runtime-random-access-stream"></a>例:.NET Framework を Windows ランタイム ランダム アクセス ストリームに変換する

次の例で示すように、.NET Framework ストリームから Windows ランタイム ランダム アクセス ストリームに変換するには、<xref:System.IO.WindowsRuntimeStreamExtensions.AsRandomAccessStream%2A> メソッドを使用します。

> [!IMPORTANT]
> 使用している .NET Framework ストリームがシークをサポートすること、またはそれを実行するストリームにコピーすることを確認します。 この確認には、 <xref:System.IO.Stream.CanSeek%2A?displayProperty=nameWithType> プロパティを使用できます。

この例を実行するには、.NET Framework 4.5.1 を対象とし、`TextBlock2` という名前のテキスト ブロックと `Button2` という名前のボタンを含む、UWP XAML アプリを作成します。 ボタン クリック イベントを、例で示されている `button2_Click` メソッドに関連付けます。

  [!code-csharp[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/cs/mainpage2.xaml.cs)]
  [!code-vb[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/vb/mainpage2.xaml.vb)]

## <a name="see-also"></a>関連項目

- [クイック スタート:ファイルの読み取りと書き込みを行う (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/hh464978(v=win.10))  
- [Windows ストア アプリ用 .NET の概要](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))  
- [Windows ストア アプリ用 .NET の API](https://docs.microsoft.com/previous-versions/br230232(v=vs.120))  
