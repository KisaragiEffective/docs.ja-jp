---
title: .NET アプリ用のリソース ファイルを作成する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- resource files, .resources files
- .resources files
- application resources, creating files
- resource files, creating
ms.assetid: 6c5ad891-66a0-4e7a-adcf-f41863ba6d8d
ms.openlocfilehash: 92e52fb130adecd6acdbeb8eac8d624d3c291094
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129977"
---
# <a name="create-resource-files-for-net-apps"></a>.NET アプリ用のリソース ファイルを作成する

リソース ファイルにリソース (文字列、イメージ、オブジェクト データなど) を追加して、アプリケーションで簡単に使用できるようにすることが可能です。 .NET Framework では、次の 5 つの方法でリソース ファイルを作成できます。

- 文字列リソースを格納するテキスト ファイルを作成します。 [リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使用すると、テキスト ファイルをバイナリ リソース (.resources) ファイルに変換できます。 次に、そのバイナリ リソース ファイルを、言語コンパイラを使用してアプリケーションの実行可能ファイルまたはアプリケーション ライブラリに埋め込むか、[アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用してサテライト アセンブリに埋め込むことができます。 詳細については、「[テキスト ファイル内のリソース](creating-resource-files-for-desktop-apps.md#TextFiles)」を参照してください。

- 文字列、イメージ、またはオブジェクト データを格納する XML リソース (.resx) ファイルを作成します。 [リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使用すると、.resx ファイルをバイナリ リソース (.resources) ファイルに変換できます。 次に、そのバイナリ リソース ファイルを、言語コンパイラを使用してアプリケーションの実行可能ファイルまたはアプリケーション ライブラリに埋め込むか、[アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用してサテライト アセンブリに埋め込むことができます。 詳細については、「[.resx ファイル内のリソース](creating-resource-files-for-desktop-apps.md#ResxFiles)」を参照してください。

- <xref:System.Resources> 名前空間の型を使用して、XML リソース (.resx) ファイルをプログラムによって作成します。 .resx ファイルを作成し、そのリソースを列挙して、特定のリソースを名前で取得することができます。 詳細については、「[プログラムによる .resx ファイルの使用](working-with-resx-files-programmatically.md)」のトピックを参照してください。

- バイナリ リソース (.resources) ファイルをプログラムによって作成します。 次に、そのファイルを、言語コンパイラを使用してアプリケーションの実行可能ファイルまたはアプリケーション ライブラリに埋め込むか、[アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用してサテライト アセンブリに埋め込むことができます。 詳細については、「[.resources ファイル内のリソース](creating-resource-files-for-desktop-apps.md#ResourcesFiles)」を参照してください。

- [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) を使用してリソース ファイルを作成し、そのファイルをプロジェクトに追加します。 Visual Studio には、リソースを追加、削除、および変更するためのリソース エディターがあります。 コンパイル時には、リソース ファイルが自動的にバイナリ .resources ファイルに変換されて、アプリケーション アセンブリまたはサテライト アセンブリに埋め込まれます。 詳細については、「[Visual Studio のリソース ファイル](creating-resource-files-for-desktop-apps.md#VSResFiles)」を参照してください。

<a name="TextFiles"></a>
## <a name="resources-in-text-files"></a>テキスト ファイル内のリソース

テキスト (.txt または .restext) ファイルを使用して格納できるのは、文字列リソースのみです。 文字列以外のリソースの場合は、.resx ファイルを使用するか、またはプログラムで作成します。 文字列リソースを格納するテキスト ファイルの形式は次のとおりです。

```text
# This is an optional comment.
name = value

; This is another optional comment.
name = value

; The following supports conditional compilation if X is defined.
#ifdef X
name1=value1
name2=value2
#endif

# The following supports conditional compilation if Y is undefined.
#if !Y
name1=value1
name2=value2
#endif
```

 .txt ファイルと .restext ファイルのリソース ファイル形式は同じです。 .restext ファイル拡張子は、テキスト ファイルがテキスト ベースのリソース ファイルであることをすぐに識別するために使用します。

 文字列リソースは、*name/value* のペアとして表示されます。ここで、*name* はリソースを識別する文字列です。*value* は、リソース取得メソッド (<xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType> など) に *name* を渡すときに返されるリソース文字列です。 *name* と *value* は等号 (=) で区切る必要があります。 (例:

```text
FileMenuName=File
EditMenuName=Edit
ViewMenuName=View
HelpMenuName=Help
```

> [!CAUTION]
> パスワード、セキュリティの配慮が必要な情報、プライベートなデータなどの格納には、リソース ファイルを使用しないでください。

 空の文字列 (つまり、値が <xref:System.String.Empty?displayProperty=nameWithType> であるリソース) はテキスト ファイルで使用できます。 (例:

```text
EmptyString=
```

 .NET Framework 4.5 以降および .NET Core のすべてのバージョンでは、`#ifdef`*symbol*... `#endif` および `#if !`*symbol*... `#endif` コンストラクトを使用した条件付きコンパイルが、テキスト ファイルでサポートされています。 次に、`/define` スイッチと[リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使用して、シンボルを定義できます。 各リソースには、独自の `#ifdef`*symbol*... `#endif` または `#if !`*symbol*... `#endif` コンストラクトが必要です。 `#ifdef` ステートメントを使用した場合、*シンボル*を定義した場合に関連付けられたリソースが .resources ファイルに追加されます。定義していない場合は追加されません。 `#if !` ステートメントを使用した場合、*シンボル*を定義していない場合に関連付けられたリソースが .resources ファイルに追加されます。定義した場合は追加されません。

 テキスト ファイルでは、コメントはオプションです。コメントの行頭にはセミコロン (;) またはシャープ記号 (#) が付きます。 コメントの行はファイル内の任意の場所に配置できます。 [リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使用して作成されたコンパイル済みの .resources ファイルにはコメントが含まれません。

 テキスト ファイル内の空白行はすべて空白と見なされて無視されます。

 次の例では、`OKButton` および `CancelButton` という名前の 2 つの文字列リソースを定義します。

```text
#Define resources for buttons in the user interface.
OKButton=OK
CancelButton=Cancel
```

 テキスト ファイルに *name* が重複して出現する場合、[リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) によって警告が表示され、2 番目の名前が無視されます。

 *値*に改行文字を含めることはできませんが、`\n` などの C 言語形式のエスケープ文字を使用すると、新しい行を表し、タブを表す `\t` ことができます。エスケープされている場合は、円記号を含めることもできます (たとえば、"\\\\")。 空の文字列を使用することもできます。

 リトル エンディアンまたはビッグ エンディアンのバイト順の UTF-8 エンコーディングまたは UTF-16 エンコーディングを使用して、テキスト ファイル形式でリソースを保存する必要があります。 ただし、.txt ファイルを .resources ファイルに変換する[リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) では、ファイルを既定では UTF-8 として扱います。 UTF-16 でエンコードされたファイルを Resgen.exe が認識できるようにする場合は、ファイルの先頭に Unicode バイト順マーク (U+FEFF) を置く必要があります。

 テキスト形式のリソース ファイルを .NET アセンブリに埋め込むには、[リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使用してファイルをバイナリ リソース (.resources) ファイルに変換する必要があります。 次に、その .resources ファイルを、言語コンパイラを使用して .NET アセンブリに埋め込むか、[アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用してサテライト アセンブリに埋め込むことができます。

 次の例では、単純な "Hello World" コンソール アプリケーションで、GreetingResources.txt という名前のテキスト形式のリソース ファイルを使用します。 このテキスト ファイルでは、ユーザーに名前の入力を求め、あいさつ文を表示する 2 つの文字列 (`prompt` および `greeting`) を定義します。

```text
# GreetingResources.txt
# A resource file in text format for a "Hello World" application.
#
# Initial prompt to the user.
prompt=Enter your name:
# Format string to display the result.
greeting=Hello, {0}!
```

このテキスト ファイルは、次のコマンドを使用して .resources ファイルに変換できます。

```console
resgen GreetingResources.txt
```

 次の例は、.resources ファイルを使用してユーザーにメッセージを表示するコンソール アプリケーションのソース コードを示しています。

 [!code-csharp[Conceptual.Resources.TextFiles#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.textfiles/cs/greeting.cs#1)]
 [!code-vb[Conceptual.Resources.TextFiles#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.textfiles/vb/greeting.vb#1)]

 Visual Basic を使用している場合、ソース コード ファイル名が Greeting.vb であれば、埋め込みの .resources ファイルを含む実行可能ファイルを次のコマンドによって作成します。

```console
vbc greeting.vb -resource:GreetingResources.resources
```

 C# を使用している場合、ソース コード ファイル名が Greeting.cs であれば、埋め込みの .resources ファイルを含む実行可能ファイルを次のコマンドによって作成します。

```console
csc greeting.cs -resource:GreetingResources.resources
```

<a name="ResxFiles"></a>
## <a name="resources-in-resx-files"></a>.resx ファイル内のリソース
 文字列リソースのみの格納が可能なテキスト ファイルとは異なり、XML リソース (.resx) ファイルは文字列、バイナリ データ (イメージ、アイコン、オーディオ クリップなど)、およびプログラム オブジェクトを格納できます。 .resx ファイルには、リソース エントリの形式を説明し、データの解析に使用する XML のバージョン管理情報を示す標準ヘッダーが含まれます。 XML ヘッダーの後に、リソース ファイル データが続きます。 各データ項目は、`data` タグに含まれる名前と値のペアで構成されます。 `name` 属性ではリソース名を定義し、入れ子になった `value` タグにはリソースの値を格納します。 文字列データの場合は、`value` タグに文字列が格納されます。

 たとえば、次の `data` タグでは、`prompt` という名前の文字列リソースを定義します。このリソースの値は "Enter your name:" です。

```xml
<data name="prompt" xml:space="preserve">
  <value>Enter your name:</value>
</data>
```

> [!WARNING]
> パスワード、セキュリティの配慮が必要な情報、プライベートなデータなどの格納には、リソース ファイルを使用しないでください。

 リソース オブジェクトの場合は、リソースのデータ型を示す `type` 属性が **data** タグに含まれます。 バイナリ データで構成されるオブジェクトの場合、`data` タグには、バイナリ データの `mimetype` 型を示す `base64` 属性も含まれます。

> [!NOTE]
> すべての .resx ファイルは、指定された型のバイナリ データを生成し解析するために、バイナリのシリアル化フォーマッタを使用します。 このため、オブジェクトのバイナリ シリアル化形式が互換性のない形式に変更されると、.resx ファイルは無効になる可能性があります。

 次の例は、<xref:System.Int32> リソースおよびビットマップ イメージを含む .resx ファイルの一部を示しています。

```xml
<data name="i1" type="System.Int32, mscorlib">
  <value>20</value>
</data>

<data name="flag" type="System.Drawing.Bitmap, System.Drawing,
    Version=1.0.5000.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
    mimetype="application/x-microsoft.net.object.bytearray.base64">
  <value>
    AAEAAAD/////AQAAAAAAAAAMAgAAADtTeX…
  </value>
</data>
```

> [!IMPORTANT]
> .resx ファイルは定義されている形式の整形式 XML で構成する必要があるため、特に文字列以外のリソースが .resx ファイルに格納されている場合に、.resx ファイルを手動で操作することはお勧めしません。 代わりに、[Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) では、.resx ファイルを作成および操作するための透過的なインターフェイスが提供されています。 詳細については、「[Visual Studio のリソース ファイル](creating-resource-files-for-desktop-apps.md#VSResFiles)」を参照してください。 .resx ファイルは、プログラムによって作成および操作することもできます。 詳細については、「[プログラムによる .resx ファイルの使用](working-with-resx-files-programmatically.md)」を参照してください。

<a name="ResourcesFiles"></a>
## <a name="resources-in-resources-files"></a>.resources ファイル内のリソース

<xref:System.Resources.ResourceWriter?displayProperty=nameWithType> クラスを使用すると、プログラムによってコードから直接、バイナリ リソース (.resources) ファイルを作成できます。 また、[リソース ファイル ジェネレーター (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) を使用して、テキスト ファイルまたは .resx ファイルから .resources ファイルを作成することもできます。 .resources ファイルには、文字列データに加えて、バイナリ データ (バイト配列) およびオブジェクト データを格納できます。 .resources ファイルをプログラムによって作成するには、次の手順が必要です。

1. 一意のファイル名を付けて <xref:System.Resources.ResourceWriter> オブジェクトを作成します。 そのためには、ファイル名またはファイル ストリームを <xref:System.Resources.ResourceWriter> クラス コンストラクターに指定します。

2. ファイルに追加する名前付きリソースごとに <xref:System.Resources.ResourceWriter.AddResource%2A?displayProperty=nameWithType> メソッドのオーバーロードの 1 つを呼び出します。 リソースには、文字列、オブジェクト、またはバイナリ データのコレクション (バイト配列) を指定できます。

3. <xref:System.Resources.ResourceWriter.Close%2A?displayProperty=nameWithType> メソッドを呼び出してファイルにリソースを書き込み、<xref:System.Resources.ResourceWriter> オブジェクトを閉じます。

> [!NOTE]
> パスワード、セキュリティの配慮が必要な情報、プライベートなデータなどの格納には、リソース ファイルを使用しないでください。

 次の例では、6 つの文字列、1 つのアイコン、2 つのアプリケーション定義オブジェクト (2 つの `Automobile` オブジェクト) を格納する、CarResources.resources という名前の .resources ファイルをプログラムによって作成します。 この例で定義およびインスタンス化される `Automobile` クラスは、<xref:System.SerializableAttribute> 属性でタグ付けされます。これにより、バイナリのシリアル化フォーマッタによってクラスを永続化できます。

 [!code-csharp[Conceptual.Resources.Resources#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resources/cs/resources1.cs#1)]
 [!code-vb[Conceptual.Resources.Resources#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resources/vb/resources1.vb#1)]

 作成した .resources ファイルは、言語コンパイラの `/resource` スイッチを追加してランタイム実行可能ファイルまたはライブラリに埋め込むか、[アセンブリ リンカー (Al.exe)](../tools/al-exe-assembly-linker.md) を使用してサテライト アセンブリに埋め込むことができます。

<a name="VSResFiles"></a>
## <a name="resource-files-in-visual-studio"></a>Visual Studio でのリソース ファイル

[Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) プロジェクトにリソース ファイルを追加すると、プロジェクト ディレクトリに .resx ファイルが作成されます。 Visual Studio には、文字列、イメージ、およびバイナリ オブジェクトを追加するためのリソース エディターがあります。 このエディターは静的データのみを処理するように設計されているので、プログラム オブジェクトを格納するために使用することはできません。オブジェクト データは、.resx ファイルまたは .resources ファイルにプログラムによって書き込む必要があります。 詳しくは、「[プログラムによる .resx ファイルの使用](working-with-resx-files-programmatically.md)」および「[.resources ファイル内のリソース](creating-resource-files-for-desktop-apps.md#ResourcesFiles)」セクションをご覧ください。

ローカライズされたリソースを追加する場合は、メイン リソース ファイルと同じルート ファイル名を指定します。 ファイル名内でカルチャを指定する必要もあります。 たとえば、Resources.resx という名前のリソース ファイルを追加する場合に、英語 (米国) とフランス語 (フランス) の各カルチャ用にローカライズされたリソースを保持するには、Resources.en-US.resx および Resources.fr-FR.resx という名前のリソース ファイルを作成します。 また、アプリケーションの既定のカルチャも指定しておく必要があります。 この情報は、特定のカルチャ用にローカライズされたリソースが見つからない場合に、どのカルチャのリソースを使用するか決定するために使用されます。 既定のカルチャを指定するには、Visual Studio のソリューション エクスプローラーで、プロジェクト名を右クリックし、[アプリケーション] をポイントして、 **[アセンブリ情報]** をクリックします。次に、 **[ニュートラル言語]** ボックスの一覧で適切な言語/カルチャをクリックします。

コンパイル時に、Visual Studio ではまずプロジェクト内の .resx ファイルがバイナリ リソース (.resources) ファイルに変換されて、プロジェクトの *obj* ディレクトリのサブディレクトリに格納されます。 ローカライズされたリソースが格納されていないリソース ファイルは、プロジェクトで生成されたメイン アセンブリに埋め込みます。 ローカライズされたリソースがリソース ファイルに格納されている場合、Visual Studio では、各ローカライズ カルチャの個別のサテライト アセンブリにそのファイルを埋め込みます。 各サテライト アセンブリは、ローカライズ カルチャに対応する名前のディレクトリに格納します。 たとえば、ローカライズされた英語 (米国) リソースは、en-US サブディレクトリ内のサテライト アセンブリに格納されます。

## <a name="see-also"></a>関連項目

- <xref:System.Resources>
- [デスクトップ アプリケーションのリソース](index.md)
- [リソースのパッケージ化と配置](packaging-and-deploying-resources-in-desktop-apps.md)
