---
title: '方法 : XML リテラルの変更'
ms.date: 07/20/2015
helpviewer_keywords:
- XML axis [Visual Basic], Value
- XML literals [Visual Basic]
- XML literals [Visual Basic], modifying
ms.assetid: 4e864522-a37a-43a2-8236-af80277c5482
ms.openlocfilehash: 99ec35addcb9fc8d886c9151cde87227b5113eb9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74330857"
---
# <a name="how-to-modify-xml-literals-visual-basic"></a>方法 : XML リテラルを変更する (Visual Basic)

Visual Basic には、XML リテラルを変更する便利な方法が用意されています。 要素と属性を追加したり削除したりできます。また、既存の要素を新しい XML 要素に置き換えることもできます。 このトピックでは、既存の XML リテラルを変更する方法の例をいくつか紹介します。

### <a name="to-modify-the-value-of-an-xml-literal"></a>XML リテラルの値を変更するには

1. XML リテラルの値を変更するには、XML リテラルへの参照を取得し、`Value` プロパティを目的の値に設定します。

    次のコード例では、XML ドキュメント内のすべての \<Price > 要素の値を更新します。

    [!code-vb[VbXmlSamples2#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#4)]

    次に、このコード例のソース XML と変更された XML の例を示します。

    ソース XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    変更された XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>47.20</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>48.25</Price>
      </Book>
    </Catalog>
    ```

    > [!NOTE]
    > `Value` プロパティは、コレクション内の最初の XML 要素を参照します。 コレクション内に同じ名前の要素が複数ある場合、`Value` プロパティの設定は、コレクション内の最初の要素にのみ影響します。

### <a name="to-add-an-attribute-to-an-xml-literal"></a>XML リテラルに属性を追加するには

1. XML リテラルに属性を追加するには、最初に XML リテラルへの参照を取得します。 その後、新しい XML 属性軸プロパティを追加して、属性を追加できます。 また、<xref:System.Xml.Linq.XContainer.Add%2A> メソッドを使用して、新しい <xref:System.Xml.Linq.XAttribute> オブジェクトを XML リテラルに追加することもできます。 次の例では、両方のオプションを示します。

    [!code-vb[VbXmlSamples2#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#5)]

    次に、このコード例のソース XML と変更された XML の例を示します。

    ソース XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" >
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    変更された XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" genre="Computer" editorEmail="someone@example.com">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331" genre="Computer" editorEmail="someone@example.com">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    XML 属性軸のプロパティの詳細については、「 [Xml 属性軸](../../../../visual-basic/language-reference/xml-axis/xml-attribute-axis-property.md)のプロパティ」を参照してください。

### <a name="to-add-an-element-to-an-xml-literal"></a>XML リテラルに要素を追加するには

1. XML リテラルに要素を追加するには、最初に XML リテラルへの参照を取得します。 次に、<xref:System.Xml.Linq.XContainer.Add%2A> メソッドを使用して、新しい <xref:System.Xml.Linq.XElement> オブジェクトを要素の最後のサブ要素として追加できます。 <xref:System.Xml.Linq.XContainer.AddFirst%2A> メソッドを使用して、新しい <xref:System.Xml.Linq.XElement> オブジェクトを最初のサブ要素として追加できます。

    他のサブ要素を基準として、特定の位置に新しい要素を追加するには、最初に隣接するサブ要素への参照を取得します。 次に、<xref:System.Xml.Linq.XNode.AddBeforeSelf%2A> メソッドを使用して、隣接するサブ要素の前に新しい <xref:System.Xml.Linq.XElement> オブジェクトを追加できます。 <xref:System.Xml.Linq.XNode.AddAfterSelf%2A> メソッドを使用して、隣接するサブ要素の後に新しい <xref:System.Xml.Linq.XElement> オブジェクトを追加することもできます。

    次の例は、これらの各手法の例を示しています。

    [!code-vb[VbXmlSamples2#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#6)]

    次に、このコード例のソース XML と変更された XML の例を示します。

    ソース XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" >
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
    </Catalog>
    ```

    変更された XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" >
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk000"></Book>
      <Book id="bk331">
        <Publisher>Microsoft Press</Publisher>
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
        <PublishDate>2005-2-14</PublishDate>
      </Book>
      <Book id="bk999"></Book>
    </Catalog>
    ```

### <a name="to-remove-an-element-or-attribute-from-an-xml-literal"></a>XML リテラルから要素または属性を削除するには

1. XML リテラルから要素または属性を削除するには、次の例に示すように、要素または属性への参照を取得し、`Remove` メソッドを呼び出します。

    [!code-vb[VbXmlSamples2#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#7)]

    次に、このコード例のソース XML と変更された XML の例を示します。

    ソース XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" genre="Computer" editorEmail="someone@example.com">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk000"></Book>
      <Book id="bk331" genre="Computer" editorEmail="someone@example.com">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book>
      <Book id="bk999"></Book>
    </Catalog>
    ```

    変更された XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101" editorEmail="someone@example.com">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
      </Book>
      <Book id="bk000"></Book>
      <Book id="bk331" editorEmail="someone@example.com">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
      </Book></Catalog>
    ```

    XML リテラルからすべての要素または属性を削除するには、XML リテラルへの参照を取得し、<xref:System.Xml.Linq.XElement.RemoveAll%2A> メソッドを呼び出します。

### <a name="to-modify-an-xml-literal"></a>XML リテラルを変更するには

1. XML 要素の名前を変更するには、最初に要素への参照を取得します。 次に、新しい名前を持つ新しい <xref:System.Xml.Linq.XElement> オブジェクトを作成し、新しい <xref:System.Xml.Linq.XElement> オブジェクトを既存の <xref:System.Xml.Linq.XElement> オブジェクトの <xref:System.Xml.Linq.XNode.ReplaceWith%2A> メソッドに渡すことができます。

    置換する要素に保持する必要があるサブ要素がある場合は、新しい <xref:System.Xml.Linq.XElement> オブジェクトの値を既存の要素の <xref:System.Xml.Linq.XContainer.Nodes%2A> プロパティに設定します。 これにより、新しい要素の値が既存の要素の内部 XML に設定されます。 それ以外の場合は、新しい要素の値を、既存の要素の `Value` プロパティに設定できます。

    次のコード例では、すべての \<Description > 要素を \<抽象 > 要素に置き換えます。 \<Description > 要素の内容は、<xref:System.Xml.Linq.XContainer.Nodes%2A> の説明 \<オブジェクトの > のプロパティを使用して、新しい \<抽象 > 要素に保持されます。<xref:System.Xml.Linq.XElement>

    [!code-vb[VbXmlSamples2#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXmlSamples2/VB/Module2.vb#8)]

    次に、このコード例のソース XML と変更された XML の例を示します。

    ソース XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <Price>44.95</Price>
        <Description>
          An in-depth look at creating applications
          with <technology>XML</technology>. For
          <audience>beginners</audience> or
          <audience>advanced</audience> developers.
        </Description>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <Price>45.95</Price>
        <Description>
          Get the expert insights, practical code samples, and best
          practices you need to advance your expertise with
          <technology>Visual Basic .NET</technology>.
          Learn how to create faster, more reliable applications
          based on professional, pragmatic guidance by today's top
          <audience>developers</audience>.
        </Description>
      </Book>
    </Catalog>
    ```

    変更された XML:

    ```xml
    <?xml version="1.0"?>
    <Catalog>
      <Book id="bk101">
        <Author>Garghentini, Davide</Author>
        <Title>XML Developer's Guide</Title>
        <MSRP>44.95</MSRP>    <Abstract>
          An in-depth look at creating applications
          with <technology>XML</technology>. For
          <audience>beginners</audience> or
          <audience>advanced</audience> developers.
        </Abstract>
      </Book>
      <Book id="bk331">
        <Author>Spencer, Phil</Author>
        <Title>Developing Applications with Visual Basic .NET</Title>
        <MSRP>45.95</MSRP>    <Abstract>
          Get the expert insights, practical code samples, and best
          practices you need to advance your expertise with
          <technology>Visual Basic .NET</technology>.
          Learn how to create faster, more reliable applications
          based on professional, pragmatic guidance by today's top
          <audience>developers</audience>.
        </Abstract>
      </Book>
    </Catalog>
    ```

## <a name="see-also"></a>参照

- [Visual Basic での XML の操作](../../../../visual-basic/programming-guide/language-features/xml/manipulating-xml.md)
- [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)
- [方法 : ファイル、文字列、またはストリームからの XML の読み込み](../../../../visual-basic/programming-guide/language-features/xml/how-to-load-xml-from-a-file-string-or-stream.md)
- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
