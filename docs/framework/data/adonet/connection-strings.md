---
title: 接続文字列
ms.date: 10/10/2018
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.openlocfilehash: cb0b2831a22f3fe51dd7c5bfbe51e72f266a0003
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980237"
---
# <a name="connection-strings-in-adonet"></a>ADO.NET での接続文字列

接続文字列には、データ プロバイダーからデータ ソースにパラメーターとして渡す初期化情報が含まれています。 データプロバイダーは、<xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType> プロパティの値として接続文字列を受け取ります。 プロバイダーは接続文字列を解析して、構文が正しいことと、キーワードがサポートされていることを確認します。 次に、<xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType> メソッドは、解析された接続パラメーターをデータソースに渡します。 データソースは、さらに検証を実行し、接続を確立します。

## <a name="connection-string-syntax"></a>接続文字列の構文

接続文字列は、キーと値のパラメーターのペアをセミコロンで区切ったリストです。

```csharp
keyword1=value; keyword2=value;
```

キーワードでは、大文字と小文字は区別されません。 ただし、データソースによっては、値によって大文字と小文字が区別されることがあります。 キーワードと値の両方に[空白文字](https://en.wikipedia.org/wiki/Whitespace_character#Unicode)を含めることができます。 キーワードおよび引用符で囲まれていない値では、先頭と末尾の空白文字は無視されます。

値にセミコロン、 [Unicode 制御文字](https://en.wikipedia.org/wiki/Unicode_control_characters)、または先頭または末尾の空白が含まれている場合は、一重引用符または二重引用符で囲む必要があります。 例:

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

囲んでいる文字が、囲まれた値内に出現しない可能性があります。 したがって、単一引用符を含む値は、二重引用符で囲むことができます。また、その逆も可能です。

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

また、2つの文字を一緒に使用して、囲み文字をエスケープすることもできます。

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

引用符自体と等号だけでは、エスケープは必要ないため、次の接続文字列が有効です。

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

各値は、次のセミコロンまたは文字列の末尾まで読み取られるため、後者の例の値は `a=b=c`、最後のセミコロンは省略可能です。

すべての接続文字列は、上記で説明したのと同じ基本的な構文を共有します。 認識されるキーワードのセットはプロバイダーによって異なり、 *ODBC*などの以前の api から長年にわたって進化しています。 *.NET Framework* data provider for *SQL Server* (`SqlClient`) は、古い api から多くのキーワードをサポートしていますが、一般的には、多くの一般的な接続文字列キーワードに対して、より柔軟で、シノニムを使用できます。

入力ミスによってエラーが発生する可能性があります。 たとえば、`Integrated Security=true` は有効ですが、`IntegratedSecurity=true` によってエラーが発生します。

実行時に未検証ユーザー入力から手動で構築された接続文字列は、文字列インジェクション攻撃に対して脆弱で、データソースのセキュリティが危険にさらされます。 これらの問題に対処するために、 *ADO.NET* 2.0 では *.NET Framework*データプロバイダーごとに[接続文字列ビルダー](connection-string-builders.md)が導入されました。 これらの接続文字列ビルダーは、パラメーターを厳密に型指定されたプロパティとして公開し、データソースに送信される前に接続文字列を検証できるようにします。

## <a name="in-this-section"></a>このセクションの内容

[接続文字列ビルダー](connection-string-builders.md)\
`ConnectionStringBuilder` クラスを使用して、有効な接続文字列を実行時に作成する方法について説明します。

[接続文字列と構成ファイル](connection-strings-and-configuration-files.md)\
構成ファイルを使用した接続文字列の格納と取得の方法について説明します。

[接続文字列の構文](connection-string-syntax.md)\
`SqlClient`、`OracleClient`、`OleDb`、`Odbc` の各プロバイダーに固有の接続文字列を構成する方法について説明します。

[接続情報の保護](protecting-connection-information.md)\
データ ソースへの接続に使用する情報を保護する方法を示します。

## <a name="see-also"></a>関連項目

- [データ ソースへの接続](/cpp/data/odbc/connecting-to-a-data-source)
- [ADO.NET の概要](ado-net-overview.md)
