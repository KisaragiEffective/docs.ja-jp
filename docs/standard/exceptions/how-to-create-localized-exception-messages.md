---
title: ローカライズされた例外メッセージを使用するユーザー定義の例外を作成する方法
description: ローカライズされた例外メッセージを使用するユーザー定義の例外を作成する方法について説明します。
author: Youssef1313
dev_langs:
- csharp
- vb
ms.date: 09/13/2019
ms.openlocfilehash: 5a02c71b16e2c8e5ade5128866af7dc46a03ba4a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160184"
---
# <a name="how-to-create-user-defined-exceptions-with-localized-exception-messages"></a>ローカライズされた例外メッセージを使用するユーザー定義の例外を作成する方法

この記事では、サテライト アセンブリを使用してローカライズされた例外メッセージ使用する、基底の <xref:System.Exception> クラスから継承されるユーザー定義の例外を作成する方法について説明します。

## <a name="create-custom-exceptions"></a>カスタムの例外を作成する

.NET には、使用できるさまざまな例外があります。 ただし、いずれもニーズに合わない場合は、独自のカスタムの例外を作成できます。

たとえば、`StudentNotFoundException` プロパティを含む `StudentName` を作成するとします。
カスタムの例外を作成するには、次の手順を実行します。

1. <xref:System.Exception> から継承されるシリアル化可能なクラスを作成します。 クラス名の末尾は "Exception" にするようにします。

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception { }
    ```

    ```vb
    <Serializable>
    Public Class StudentNotFoundException
        Inherits Exception
    End Class
    ```

1. 既定のコンストラクターを追加します。

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }
    }
    ```

    ```vb
    <Serializable>
    Public Class StudentNotFoundException
        Inherits Exception

        Public Sub New()
        End Sub

        Public Sub New(message As String)
            MyBase.New(message)
        End Sub

        Public Sub New(message As String, inner As Exception)
            MyBase.New(message, inner)
        End Sub
    End Class
    ```

1. 追加のプロパティとコンストラクターを定義します。

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public string StudentName { get; }

        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }

        public StudentNotFoundException(string message, string studentName)
            : this(message)
        {
            StudentName = studentName;
        }
    }
    ```

    ```vb
    <Serializable>
    Public Class StudentNotFoundException
        Inherits Exception

        Public ReadOnly Property StudentName As String

        Public Sub New()
        End Sub

        Public Sub New(message As String)
            MyBase.New(message)
        End Sub

        Public Sub New(message As String, inner As Exception)
            MyBase.New(message, inner)
        End Sub

        Public Sub New(message As String, studentName As String)
            Me.New(message)
            StudentName = studentName
        End Sub
    End Class
    ```

## <a name="create-localized-exception-messages"></a>ローカライズされた例外メッセージを作成する

カスタムの例外を作成すると、次のようなコードを使用して任意の場所に例外をスローすることができます。

```csharp
throw new StudentNotFoundException("The student cannot be found.", "John");
```

```vb
Throw New StudentNotFoundException("The student cannot be found.", "John")
```

前の行の問題は、`"The student cannot be found."` が定数文字列であることです。 ローカライズされるアプリケーションでは、ユーザーのカルチャに応じて異なるメッセージを使用する必要があります。
[サテライト アセンブリ](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)はこの処理に適しています。 サテライト アセンブリは、特定の言語のリソースを含む .dll です。 実行時に特定のリソースを要求すると、ユーザーのカルチャに応じて CLR によってそのリソースが検索されます。 そのカルチャのサテライト アセンブリが見つからない場合は、既定のカルチャのリソースが使用されます。

ローカライズされた例外メッセージを作成するには:

1. リソース ファイルを保持するために、*Resources* という名前の新しいフォルダーを作成します。
1. そこに新しいリソース ファイルを追加します。 Visual Studio でこれを行うには、**ソリューション エクスプローラー**でフォルダーを右クリックし、 **[追加]**  >  **[新しい項目]**  >  **[リソース ファイル]** を選択します。 ファイルに *ExceptionMessages.resx* という名前を付けます。 これは既定のリソース ファイルです。
1. 次の図に示すように、例外メッセージの名前と値のペアを追加します。

   ![既定のカルチャにリソースを追加する](media/add-resources-to-default-culture.jpg)

1. フランス語用の新しいリソース ファイルを追加します。 *ExceptionMessages.fr-FR.resx* という名前を付けます。
1. 例外メッセージの名前と値のペアを再び追加します。今回はフランス語の値を使用します。

   ![fr-FR カルチャにリソースを追加する](media/add-resources-to-fr-culture.jpg)

1. プロジェクトをビルドすると、ビルド出力フォルダーには、サテライト アセンブリである *.dll* ファイルを含む *fr-FR* フォルダーが作成されます。
1. 次のようなコードを使用して例外をスローします。

    ```csharp
    var resourceManager = new ResourceManager("FULLY_QIALIFIED_NAME_OF_RESOURCE_FILE", Assembly.GetExecutingAssembly());
    throw new StudentNotFoundException(resourceManager.GetString("StudentNotFound"), "John");
    ```

    ```vb
    Dim resourceManager As New ResourceManager("FULLY_QIALIFIED_NAME_OF_RESOURCE_FILE", Assembly.GetExecutingAssembly())
    Throw New StudentNotFoundException(resourceManager.GetString("StudentNotFound"), "John")
    ```

    > [!NOTE]
    > プロジェクト名が `TestProject` で、リソース ファイル *ExceptionMessages.resx* がプロジェクトの *Resources* フォルダー内にある場合、リソース ファイルの完全修飾名は `TestProject.Resources.ExceptionMessages` です。

## <a name="see-also"></a>参照

- [ユーザー定義の例外を作成する方法](how-to-create-user-defined-exceptions.md)
- [デスクトップ アプリケーションに対するサテライト アセンブリの作成](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)
- [base (C# リファレンス)](../../csharp/language-reference/keywords/base.md)
- [this (C# リファレンス)](../../csharp/language-reference/keywords/this.md)
