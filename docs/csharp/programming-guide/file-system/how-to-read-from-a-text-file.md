---
title: テキスト ファイルから読み取る方法 - C# プログラミング ガイド
ms.date: 07/20/2015
f1_keywords:
- StreamReader.ReadToEnd
helpviewer_keywords:
- text files, writing to
- reading text files
- reading data, text files
- text files, reading
ms.assetid: 92246c5b-e819-4eea-9370-1a9460e12de3
ms.openlocfilehash: d401a1d1bb2c6fccb203c440f367bd14c80e70e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705016"
---
# <a name="how-to-read-from-a-text-file-c-programming-guide"></a>テキスト ファイルから読み取る方法 (C# プログラミング ガイド)
この例では、<xref:System.IO.File.ReadAllText%2A> クラスの静的メソッド <xref:System.IO.File.ReadAllLines%2A> と <xref:System.IO.File?displayProperty=nameWithType> を使用してテキスト ファイルの内容を読み取ります。  
  
<xref:System.IO.StreamReader> の使用例については、「[テキスト ファイルを 1 行ずつ読み取る方法](./how-to-read-a-text-file-one-line-at-a-time.md)」を参照してください。
  
> [!NOTE]
> この例では、「[テキスト ファイルに書き込む方法](./how-to-write-to-a-text-file.md)」トピックで作成したファイルを使用しています。
  
## <a name="example"></a>例  
 [!code-csharp[csFilesandFolders#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#4)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 コードをコピーし、C# のコンソール アプリケーションに貼り付けます。  
  
「[テキスト ファイルに書き込む方法](./how-to-write-to-a-text-file.md)」のテキスト ファイルを使用せずに独自のテキスト ファイルを使用する場合は、`ReadAllText` と `ReadAllLines` の引数を、ご使用のコンピューター上の該当するパスおよびファイル名に置き換えてください。
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- ファイルが存在しない、または指定した場所に存在しない。 ファイル名のパスとスペルを確認してください。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 ファイル名に基づいてファイルの内容を判断しないでください。 たとえば、`myFile.cs` というファイルが C# のソース ファイルではない可能性もあります。  
  
## <a name="see-also"></a>参照

- <xref:System.IO?displayProperty=nameWithType>
- [C# プログラミングガイド](../index.md)
- [ファイル システムとレジストリ (C# プログラミング ガイド)](./index.md)
