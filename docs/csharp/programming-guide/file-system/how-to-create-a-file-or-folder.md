---
title: ファイルまたはフォルダーを作成する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- folders [C#]
- creating files [C#]
- files [C#]
- creating folders [C#]
ms.assetid: 4582ee2d-d72d-4687-bcb9-08d336c62c25
ms.openlocfilehash: cdcc0a375aa1eca29c024d1e0c9008f337d0c772
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79167558"
---
# <a name="how-to-create-a-file-or-folder-c-programming-guide"></a>ファイルまたはフォルダーを作成する方法 (C# プログラミング ガイド)
プログラムによって、コンピューター上でのフォルダーの作成、サブフォルダーの作成、サブフォルダー内でのファイルの作成、およびファイルへのデータの記述を行うことができます。  
  
## <a name="example"></a>例  
 [!code-csharp[csFilesandFolders#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#10)]  
  
 フォルダーが既に存在していた場合、<xref:System.IO.Directory.CreateDirectory%2A> は何も実行せず、例外はスローされません。 ただし <xref:System.IO.File.Create%2A?displayProperty=nameWithType> は、既存のファイルを新しいファイルに置き換えます。 この例では、`if`-`else` ステートメントを使用して、既存のファイルが置き換えられないようにします。  
  
 この例に次の変更を加えることによって、特定の名前のファイルが既に存在するかどうかに基づいて異なる結果を指定できます。 そのようなファイルが存在しない場合、コードによって作成されます。 このようなファイルがある場合、コードはそのファイルにデータを追加します。  
  
- ランダムではないファイル名を指定します。  
  
    ```csharp  
    // Comment out the following line.  
    //string fileName = System.IO.Path.GetRandomFileName();  
  
    // Replace that line with the following assignment.  
    string fileName = "MyNewFile.txt";  
    ```  
  
- 次のコードで、`if`-`else` ステートメントを `using` に置き換えます。  
  
    ```csharp  
    using (System.IO.FileStream fs = new System.IO.FileStream(pathString, FileMode.Append))
    {  
        for (byte i = 0; i < 100; i++)  
        {  
            fs.WriteByte(i);  
        }  
    }  
    ```  
  
 データがファイルに追加されたことを毎回確認するために、この例を数回実行します。  
  
 試用できるその他の `FileMode` 値については、<xref:System.IO.FileMode> を参照してください。  
  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- フォルダー名が不適切である場合。 たとえば、不正な文字が含まれている場合や、空白だけの場合などがその例です (<xref:System.ArgumentException> クラス)。 有効なパス名を作成するには、<xref:System.IO.Path> クラスを使用します。  
  
- 作成するフォルダーの親フォルダーが読み取り専用である場合 (<xref:System.IO.IOException> クラス)。  
  
- フォルダー名が `null` である場合 (<xref:System.ArgumentNullException> クラス)。  
  
- フォルダー名が長すぎる場合 (<xref:System.IO.PathTooLongException> クラス)。  
  
- フォルダー名がコロン (":") だけである場合 (<xref:System.IO.PathTooLongException> クラス)。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  
 部分的に信頼された状況では、<xref:System.Security.SecurityException> クラスのインスタンスがスローされることがあります。  
  
 フォルダーの作成に必要なアクセス許可が与えられていない場合、この例では <xref:System.UnauthorizedAccessException> クラスのインスタンスがスローされます。  
  
## <a name="see-also"></a>参照

- <xref:System.IO?displayProperty=nameWithType>
- [C# プログラミングガイド](../index.md)
- [ファイル システムとレジストリ (C# プログラミング ガイド)](./index.md)
