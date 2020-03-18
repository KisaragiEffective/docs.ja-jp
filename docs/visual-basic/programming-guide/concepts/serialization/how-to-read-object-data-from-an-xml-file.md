---
title: '方法: XML ファイルからオブジェクト データを読み込む'
ms.date: 07/20/2015
ms.assetid: 1e1423bf-74a4-4dde-a3bb-ae1bfc0a68ed
ms.openlocfilehash: efd5fb72487c92bcccf1fc797106f93c0d2a39fc
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345990"
---
# <a name="how-to-read-object-data-from-an-xml-file-visual-basic"></a>方法: XML ファイルからオブジェクトデータを読み取る (Visual Basic)
次の例では、<xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、XML ファイルに以前に書き込まれたオブジェクト データを読み込みます。  
  
## <a name="example"></a>使用例  
  
```vb  
Public Class Book  
    Public Title As String  
End Class  
  
Public Sub ReadXML()  
    Dim reader As New System.Xml.Serialization.XmlSerializer(GetType(Book))  
    Dim file As New System.IO.StreamReader(  
        "c:\temp\SerializationOverview.xml")  
    Dim overview As Book  
    overview = CType(reader.Deserialize(file), Book)  
    Console.WriteLine(overview.Title)  
End Sub  
```  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 ファイル名 "c:\temp\SerializationOverview.xml" を、シリアル化されたデータを含むファイルの名前に置き換えます。 データのシリアル化の詳細については、「[方法: XML ファイルにオブジェクトデータを書き込む (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-write-object-data-to-an-xml-file.md)」を参照してください。  
  
 クラスには、パラメーターのないパブリック コンストラクターが必要です。  
  
 パブリック プロパティとパブリック フィールドだけが逆シリアル化されます。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 次の条件を満たす場合は、例外が発生する可能性があります。  
  
- シリアル化されるクラスにパブリックなパラメーターなしのコンストラクターがない場合  
  
- ファイル内のデータが、逆シリアル化されるクラスのデータを表していない場合。  
  
- ファイルが存在しない (<xref:System.IO.IOException>)。  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 入力を常に検証し、信頼できないソースから決してデータを逆シリアル化しないでください。 再作成されたオブジェクトは、そのオブジェクトを逆シリアル化したコードと同じアクセス許可を持つローカル コンピューターで実行されます。 アプリケーションでデータを使用する前に、入力をすべて検証してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.IO.StreamWriter>
- [方法: XML ファイルにオブジェクト データを書き込む (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-write-object-data-to-an-xml-file.md)
- [シリアル化 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/index.md)
- [Visual Basic のプログラミング ガイド](../../../../visual-basic/programming-guide/index.md)
