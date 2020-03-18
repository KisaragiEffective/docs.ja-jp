---
title: XML ドキュメント機能を使用する方法 - C# プログラミング ガイド
ms.date: 06/01/2018
helpviewer_keywords:
- XML documentation [C#]
- C# language, XML documentation features
ms.assetid: 8f33917b-9577-4c9a-818a-640dbbb0b399
ms.openlocfilehash: e279b13d9216120e25f454faa14dc71ad24c74ef
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79157001"
---
# <a name="how-to-use-the-xml-documentation-features"></a>XML ドキュメント機能を使用する方法

次の例では、ドキュメント化された型の基本的な概要について説明します。

## <a name="example"></a>例

[!code-csharp[csProgGuideDocComments#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#15)]

この例では、次の内容を含む *.xml* ファイルが生成されます。

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>xmlsample</name>
    </assembly>
    <members>
        <member name="T:TestClass">
            <summary>
            Class level summary documentation goes here.
            </summary>
            <remarks>
            Longer comments can be associated with a type or member through
            the remarks tag.
            </remarks>
        </member>
        <member name="F:TestClass._name">
            <summary>
            Store for the Name property.
            </summary>
        </member>
        <member name="M:TestClass.#ctor">
            <summary>
            The class constructor.
            </summary>
        </member>
        <member name="P:TestClass.Name">
            <summary>
            Name property.
            </summary>
            <value>
            A value tag is used to describe the property value.
            </value>
        </member>
        <member name="M:TestClass.SomeMethod(System.String)">
            <summary>
            Description for SomeMethod.
            </summary>
            <param name="s"> Parameter description for s goes here.</param>
            <seealso cref="T:System.String">
            You can use the cref attribute on any tag to reference a type or member
            and the compiler will check that the reference exists.
            </seealso>
        </member>
        <member name="M:TestClass.SomeOtherMethod">
            <summary>
            Some other method.
            </summary>
            <returns>
            Return values are described through the returns tag.
            </returns>
            <seealso cref="M:TestClass.SomeMethod(System.String)">
            Notice the use of the cref attribute to reference a specific method.
            </seealso>
        </member>
        <member name="M:TestClass.Main(System.String[])">
            <summary>
            The entry point for the application.
            </summary>
            <param name="args"> A list of command line arguments.</param>
        </member>
        <member name="T:TestInterface">
            <summary>
            Documentation that describes the interface goes here.
            </summary>
            <remarks>
            Details about the interface go here.
            </remarks>
        </member>
        <member name="M:TestInterface.InterfaceMethod(System.Int32)">
            <summary>
            Documentation that describes the method goes here.
            </summary>
            <param name="n">
            Parameter n requires an integer argument.
            </param>
            <returns>
            The method returns an integer.
            </returns>
        </member>
    </members>
</doc>
```

## <a name="compiling-the-code"></a>コードのコンパイル

この例をコンパイルするには、次のコマンド行を入力します。

`csc XMLsample.cs /doc:XMLsample.xml`

このコマンドによって作成される XML ファイル *XMLsample.xml* は、ブラウザーで、または TYPE コマンドを使って、表示できます。

## <a name="robust-programming"></a>信頼性の高いプログラミング

XML ドキュメントは、/// で始まります。 新しいプロジェクトを作成すると、開始の /// 行がいくつか自動的に挿入されます。 これらのコメントの処理にはいくつか制限があります。

- ドキュメントは整形式の XML である必要があります。 XML が整形式ではない場合は、警告が生成され、エラーが発生したことを示すコメントがドキュメント ファイルに追加されます。

- 開発者は、独自のタグ セットを自由に作成できます。 [推奨されるタグのセット](recommended-tags-for-documentation-comments.md)があります。 推奨されるタグの一部には特別な意味があります。

  - \<param> タグは、パラメーターの記述に使われます。 このタグがあると、コンパイラは、パラメーターが存在すること、およびすべてのパラメーターがドキュメントで記述されていることを確認します。 検証で問題がある場合、コンパイラは警告を生成します。

  - `cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。 コンパイラは、このコード要素が存在することを確認します。 検証で問題がある場合、コンパイラは警告を生成します。 コンパイラは、`using` 属性で記述されている型を探すとき、`cref` ステートメントに従います。

  - \<summary> タグは、型またはメンバーに関する追加情報を表示するために、Visual Studio の IntelliSense によって使われます。

    > [!NOTE]
    > XML ファイルでは、型とメンバーに関する完全な情報は提供されません (たとえば、型の情報は含まれません)。 型またはメンバーの完全な情報を取得するには、ドキュメント ファイルと併せて、実際の型またはメンバーでリフレクションを使う必要があります。

## <a name="see-also"></a>参照

- [C# プログラミング ガイド](../index.md)
- [-doc (C# コンパイラ オプション)](../../language-reference/compiler-options/doc-compiler-option.md)
- [XML ドキュメント コメント](./index.md)
- [DocFX ドキュメント プロセッサ](https://dotnet.github.io/docfx/)
- [Sandcastle ドキュメント プロセッサ](https://github.com/EWSoftware/SHFB)
