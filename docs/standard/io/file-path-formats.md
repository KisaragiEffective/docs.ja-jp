---
title: Windows システムのファイル パス形式
ms.date: 06/06/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O, long paths
- long paths
- path formats, Windows
ms.openlocfilehash: b3510be5d417b555d2db163636eac5ce0c0779e4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77628047"
---
# <a name="file-path-formats-on-windows-systems"></a>Windows システムのファイル パス形式

<xref:System.IO> 名前空間の型の多くのメンバーには `path` パラメーターが含まれています。このパラメーターによって、ファイル システム リソースの絶対パスまたは相対パスを指定できます。 このパスはその後、[Windows ファイル システム API](/windows/desktop/fileio/file-systems) に渡されます。 このトピックでは、Windows システムで利用できるファイル パスの形式について説明します。

## <a name="traditional-dos-paths"></a>従来の DOS パス

標準 DOS パスは 3 つの要素で構成できます。

- ボリュームまたはドライブ文字とそれに続くボリューム区切り記号 (`:`)。
- ディレクトリ名。 [ディレクトリ区切り文字](<xref:System.IO.Path.DirectorySeparatorChar>)によって、入れ子になっているディレクトリ階層内でサブディレクトリが分割されます。
- 任意のファイル名。 [ディレクトリ区切り文字](<xref:System.IO.Path.DirectorySeparatorChar>)によって、ファイル パスとファイル名が分割されます。

3 つのコンポーネントがすべて存在する場合、パスは絶対パスになります。 ボリュームまたはドライブ文字が指定されておらず、ディレクトリ名が[ディレクトリ区切り文字](<xref:System.IO.Path.DirectorySeparatorChar>)で始まる場合、パスは現在のドライブのルートからの相対パスとなります。 それ以外の場合、パスは現在のディレクトリに相対となります。 次の表では、想定されるディレクトリとファイル パスをいくつかまとめています。

|Path  |[説明]  |
| -- | -- |
| `C:\Documents\Newsletters\Summer2018.pdf` | ドライブ C: のルートからの絶対ファイル パス。 |
| `\Program Files\Custom Utilities\StringFinder.exe` | 現在のドライブのルートからの絶対パス。 |
| `2018\January.xlsx` | 現在のディレクトリのサブディレクトリにあるファイルへの相対パス。 |
| `..\Publications\TravelBrochure.pdf` | 現在のディレクトリと同等になるディレクトリにあるファイルへの相対パス。 |
| `C:\Projects\apilibrary\apilibrary.sln` | ドライブ C: のルートからのファイルへの絶対パス。 |
| `C:Projects\apilibrary\apilibrary.sln` | C: ドライブの現在のディレクトリからの相対パス。 |

> [!IMPORTANT]
> 最後の 2 つのパスの違いにご注意ください。 いずれも任意のボリューム指定子を指定しますが (いずれの場合も C:)、最後から 2 番目のパスが指定ボリュームのルートから始まるのに対し、最後のパスは指定ボリュームのルートから始まりません。 結果として、最後から 2 番目のパスがドライブ C: のルート ディレクトリからの絶対パスであるのに対し、最後のパスはドライブ C: の現在のディレクトリからの相対パスになります。 最後から 2 番目のパス形式を意図しているときに最後のパス形式を使用することが、Windows ファイル パス関連のバグの共通の源になっています。

<xref:System.IO.Path.IsPathFullyQualified%2A?displayProperty=nameWthType> メソッドを呼び出すことで、ファイルが完全修飾であるかどうかを判断できます。完全修飾の場合、パスが現在のディレクトリに依存せず、現在のディレクトリが変更されても完全修飾のパスは変わりません。 そのようなパスには相対ディレクトリのセグメント (`.` や `..`) が含まれている可能性があり、解決後のパスが常に同じ場所を指す場合、完全修飾できることにご留意ください。

次の例では、絶対パスと相対パスの違いを示します。 ディレクトリ D:\FY2018\ が存在すること、この例を実行する前に、コマンド プロンプトから D:\ に現在のディレクトリを設定していないことを想定しています。

[!code-csharp[absolute-and-relative-paths](~/samples/snippets/standard/io/file-names/cs/paths.cs)]
[!code-vb[absolute-and-relative-paths](~/samples/snippets/standard/io/file-names/vb/paths.vb)]

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="unc-paths"></a>UNC パス

UNC (汎用命名規則) パスはネットワーク リソースへのアクセスに利用され、次の形式になっています。

- サーバーまたはホストの名前。先頭に \\\\ が付きます。 サーバー名は、NetBIOS マシン名か IP/FQDN アドレス (IPv4 と v6 に対応) にすることができます。
- 共有名。ホスト名とは \\ で区切られます。 サーバーと共有名を合わせてボリュームになります。
- ディレクトリ名。 [ディレクトリ区切り文字](<xref:System.IO.Path.DirectorySeparatorChar>)によって、入れ子になっているディレクトリ階層内でサブディレクトリが分割されます。
- 任意のファイル名。 [ディレクトリ区切り文字](<xref:System.IO.Path.DirectorySeparatorChar>)によって、ファイル パスとファイル名が分割されます。

UNC パスの例を次に示します。

|Path  |[説明]  |
| -- | -- |
| `\\system07\C$\` | `system07` の C: のルート ディレクトリ。 |
| `\\Server2\Share\Test\Foo.txt` | \\\\Server2\\Share ボリュームの Test ディレクトリの Foo.txt ファイル。|

UNC パスは常に完全修飾にする必要があります。 相対ディレクトリ セグメント (`.` や `..`) を含めることができますが、そのようなセグメントは完全修飾パスの一部にする必要があります。 相対パスは、UNC パスをドライブ文字にマッピングする方法でのみ使用できます。

## <a name="dos-device-paths"></a>DOS デバイス パス

Windows オペレーティング システムには、ファイルを含む、すべてのリソースを指す統一オブジェクト モデルがあります。 これらのオブジェクト パスにはコンソール ウィンドウからアクセスできます。また、これらのオブジェクト パスは、DOS と UNC のレガシ パスがマッピングされているシンボリック リンクの特別なフォルダーを介して Win32 層に公開されます。 この特別なフォルダーには、DOS デバイス パス構文を介してアクセスできます。この構文は次のいずれかになります。

`\\.\C:\Test\Foo.txt`
`\\?\C:\Test\Foo.txt`

ドライブ文字でドライブを識別できるだけでなく、ボリューム GUID を使用してボリュームを識別できます。 次のような形式になります。

`\\.\Volume{b75e2c83-0000-0000-0000-602f00000000}\Test\Foo.txt`
`\\?\Volume{b75e2c83-0000-0000-0000-602f00000000}\Test\Foo.txt`

> [!NOTE]
> DOS デバイス パス構文は、Windows で実行されている .NET 実装でサポートされます。 .NET Core は 1.1 以降、.NET Framework は 4.6.2 以降となります。

DOS デバイス パスは次の要素から構成されます。

- デバイス パス指定子 (`\\.\` か `\\?\`)。この指定子により、DOS デバイス パスとしてパスが識別されます。

   > [!NOTE]
   > `\\?\` は、あらゆるバージョンの .NET Core とバージョン 4.6.2 以降の .NET Framework でサポートされます。

- "実際の" デバイス オブジェクトへのシンボリック リンク (ドライブ名の場合は C:、ボリュームの GUID の場合は Volume{b75e2c83-0000-0000-0000-602f00000000})。

   デバイス パス指定子の後の最初のセグメントによって、ボリュームまたはドライブが識別されます。 (たとえば、`\\?\C:\` や `\\.\BootPartition\`)。

   UNC のためのリンク (わかりやすいことに `UNC`) が呼び出されます。 次に例を示します。

  `\\.\UNC\Server\Share\Test\Foo.txt`
  `\\?\UNC\Server\Share\Test\Foo.txt`

    デバイス UNC の場合、サーバー/共有の部分がボリュームになります。 たとえば、`\\?\server1\e:\utilities\\filecomparer\` では、サーバー/共有部分は server1\utilities です。 相対ディレクトリ セグメントのある <xref:System.IO.Path.GetFullPath(System.String,System.String)?displayProperty=nameWithType> のようなメソッドを呼び出すとき、これは重要です。ボリュームを通り過ぎて移動することはできません。

DOS デバイス パスは定義によって完全修飾されます。 相対ディレクトリ セグメント (`.` や `..`) は許可されません。 現在のディレクトリが使用されることはありません。

## <a name="example-ways-to-refer-to-the-same-file"></a>例: 同じファイルを参照する方法

次の例では、<xref:System.IO> 名前空間で API を使用するとき、あるファイルを参照する方法をいくつか示しています。 この例では <xref:System.IO.FileInfo> オブジェクトをインスタンス化し、その <xref:System.IO.FileInfo.Name> プロパティと <xref:System.IO.FileInfo.Length> プロパティを使用してファイルの名前と長さを表示します。

[!code-csharp[referring-to-the-same-file](~/samples/snippets/standard/io/file-names/cs/file-refs.cs)]
[!code-vb[referring-to-the-same-file](~/samples/snippets/standard/io/file-names/vb/file-refs.vb)]

## <a name="path-normalization"></a>パスの正規化

Windows API に渡されるパスはほとんどすべて正規化されます。 正規化の間、Windows では次の手順が実行されます。

- パスを識別します。
- 部分的に修飾された (相対) パスに現在のディレクトリが適用されます。
- コンポーネントとディレクトリの区切り記号を正規化します。
- 相対ディレクトリ コンポーネントを評価します (現在のディレクトリの場合は `.`、親ディレクトリの場合は `..`)。
- 特定の文字をトリミングします。

この正規化は暗黙的に行われますが、<xref:System.IO.Path.GetFullPath%2A?displayProperty=nameWithType> メソッドを呼び出すことで明示的に行うことができます。このメソッドは [GetFullPathName() 関数](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea)の呼び出しをラップします。 Windows [GetFullPathName() 関数](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) を P/Invoke で直接呼び出すこともできます。

### <a name="identifying-the-path"></a>パスの識別

パス正規化の最初の手順は、パスの種類の識別です。 パスはいくつかあるカテゴリの 1 つに分類されます。

- パスはデバイス パスです。つまり、2 つの区切り記号で始まり、疑問符かピリオドが続きます (`\\?` または `\\.`)。
- パスは UNC パスです。つまり、2 つの区切り記号で始まります。疑問符やピリオドは付きません。
- パスは完全修飾 DOS パスです。つまり、ドライブ文字で始まり、ボリューム区切り記号、コンポーネント区切り記号が続きます (`C:\`)。
- レガシ デバイスを示します (`CON`、`LPT1`)。
- 現在のドライブのルートに相対です。つまり、1 つのコンポーネント区切り記号で始まります (`\`)。
- 指定ドライブの現在のディレクトリに相対です。つまり、ドライブ文字で始まり、ボリューム区切り記号が続きますが、コンポーネント区切り記号はありません (`C:`)。
- 現在のディレクトリに相対です。つまり、パスの最初の要素は問われません (`temp\testfile.txt`)。

パスの種類によって、現在のディレクトリが何らかの方法で適用されるかどうかが決まります。 パスの "ルート" も決定されます。

### <a name="handling-legacy-devices"></a>レガシ デバイスの取り扱い

パスが `CON`、`COM1`、`LPT1` など、レガシ DOS デバイスの場合、`\\.\` を先頭に付けることでデバイス パスに変換した上で返されます。

<xref:System.IO.Path.GetFullPath(System.String)?displayProperty=nameWithType> メソッドは、レガシ デバイス名で始まるパスを常にレガシ デバイスとして解釈します。 たとえば、`CON.TXT` の DOS デバイス パスは `\\.\CON` であり、`COM1.TXT\file1.txt` の DOS デバイス パスは `\\.\COM1` です。

### <a name="applying-the-current-directory"></a>現在のディレクトリを適用する

パスが完全修飾ではない場合、Windows はそのパスに現在のディレクトリを適用します。 UNC とデバイス パスの場合、現在のディレクトリは適用されません。 区切り記号 (C:\\) を使用するフル ドライブの場合も適用されません。

パスが 1 つのコンポーネント区切り記号で始まる場合、現在のディレクトリからのドライブが適用されます。 たとえば、ファイル パスが `\utilities` で、現在のディレクトリが `C:\temp\` の場合、正規化の結果、`C:\utilities` になります。

パスがドライブ文字で始まり、ボリューム区切り記号が続くが、コンポーネント区切り記号はない場合、指定ドライブのコマンド シェルから設定された最後の現在のディレクトリが適用されます。 最後の現在のディレクトリが設定されなかった場合、ドライブだけが適用されます。 たとえば、ファイル パスが `D:sources`、現在のディレクトリが `C:\Documents\`、ドライブ D: の最後の現在のディレクトリが `D:\sources\` の場合、結果は `D:\sources\sources` になります。 プログラムやスクリプト ロジックでエラーが出るとき、一般的に、このような "ドライブ相対" パスが原因となります。 文字やコロンで始まるパスは相対ではないと考えることは明らかに正しくありません。

パスが区切り記号以外で始まる場合、現在のドライブと現在のディレクトリが適用されます。 たとえば、パスが `filecompare` で、現在のディレクトリが `C:\utilities\` の場合、結果は `C:\utilities\filecompare\` です。

> [!IMPORTANT]
> マルチスレッド アプリケーションの場合 (つまり、ほとんどのアプリケーションでは)、現在のディレクトリはプロセスごとの設定となるため、相対パスは危険です。 スレッドによって現在のディレクトリが変更されることは、いつでも起こりえることです。 .NET Core 2.1 以降、<xref:System.IO.Path.GetFullPath(System.String,System.String)?displayProperty=nameWithType> メソッドを呼び出し、絶対パスに対して解決する場合、相対パスと基本パス (現在のディレクトリ) から絶対パスを取得できます。

### <a name="canonicalizing-separators"></a>区切り記号の正規化

フォワード スラッシュ (`/`) はすべて標準の Windows 区切り記号であるバック スラッシュ (`\`) に変換されます。 フォワード スラッシュがあるとき、最初の 2 つのスラッシュに続くスラッシュはすべて 1 つのスラッシュにまとめられます。

### <a name="evaluating-relative-components"></a>相対コンポーネントを評価する

パスが処理されるとき、1 つか 2 つのピリオド (`.` や `..`) で構成されているコンポーネントやセグメントがあればそれは評価されます。

- ピリオドが 1 つの場合、現在のセグメントが削除されます。現在のディレクトリを参照するためです。

- ピリオドが 2 つの場合、現在のセグメントと親セグメントが削除されます。二重ピリオドは親ディレクトリを参照するためです。

   親ディレクトリは、それがパスのルートを通らない場合にのみ削除されます。 パスのルートは、パスの種類によって異なります。 DOS パスの場合はドライブ (`C:\`)、UNC の場合はサーバー/共有 (`\\Server\Share`)、デバイス パスの場合はデバイス パス プレフィックス (`\\?\` または `\\.\`) となります。

### <a name="trimming-characters"></a>文字のトリミング

一連の区切り記号や相対セグメントが削除されることを確認しましたが、それに加え、正規化の間、一部の追加文字が削除されます。

- セグメントがシングル ピリオドで終わる場合、そのピリオドは削除されます。 (シングルまたはダブル ピリオドのセグメントは前の手順で正規化されます。 ピリオドが 3 つ以上のセグメントは正規化されません。実際には、有効なファイル/ディレクトリ名です。)

- パスの終わりが区切り記号ではない場合、後ろに続くピリオドとスペース (U+0020) はすべて削除されます。 最後のセグメントがシングルまたはダブル ピリオドの場合、上記の相対コンポーネント ルールに該当します。

   このルールによると、スペースの後に区切り記号を追加することで、スペースが後ろに続くディレクトリ名を作成できます。

   > [!IMPORTANT]
   > スペースが後ろに続くディレクトリやファイル名は**決して**作成しないでください。 後続スペースはディレクトリへのアクセスを困難または不可能にします。一般的に、名前に後続スペースが含まれるディレクトリやファイルを処理しようとすると、アプリケーションは失敗します。

## <a name="skipping-normalization"></a>正規化の省略

通常、Windows API に渡されるパスはすべて、[GetFullPathName 関数](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea)に (効果的に) 渡され、正規化されます。 重要な例外が 1 つあります。ピリオドではなく疑問符から始まるデバイス パスです。 厳密に `\\?\` で始まらない限り (正規のバックスラッシュが使われていることに注目してください)、パスは正規化されます。

正規化の省略が必要となる理由について考えてみます。 大きな理由が 3 つあります。

1. 通常は利用できないが正当なパスにアクセスするため。 たとえば、`hidden.` というファイルまたはディレクトリには他の方法ではアクセスできません。

1. 既に正規化している場合、省略することでパフォーマンスを改善するため。

1. .NET Framework でのみ、259 文字を超えるパスの場合、パス長の `MAX_PATH` チェックを省略できます。 一部例外がありますが、ほとんどの API でこれが可能です。

> [!NOTE]
> .NET Core は長いパスを暗黙的に処理し、`MAX_PATH` チェックを実行しません。 `MAX_PATH` は .NET Framework にのみ適用されます。

正規化と MAX_PATH チェックの省略は、2 つのデバイス パス構文の間の唯一の違いです。それ以外、この 2 つは同じです。 正規化の省略には注意してください。"普通の" アプリケーションでは処理できないパスが簡単に作られてしまいます。

`\\?\` で始まるパスは、[GetFullPathName 関数](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea)に明示的に渡す場合、正規化されます。

`MAX_PATH` 文字を超えるパスは [ なしで ](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea)GetFullPathName`\\?\` に渡すことができます。 Windows で処理できる最大文字列サイズまで、任意の長さのパスがサポートされます。

## <a name="case-and-the-windows-file-system"></a>大文字/小文字の区別と Windows ファイル システム

Windows を使っていないユーザーや開発者がとまどうことには、Windows ファイル システムではパス名とディレクトリ名で大文字と小文字が区別されないという特徴があります。 つまり、ディレクトリ名とファイル名は、それが作成されたときの大文字/小文字の使い分けを反映します。 たとえば、次のメソッドを呼び出すと、

```csharp
Directory.Create("TeStDiReCtOrY");
```

```vb
Directory.Create("TeStDiReCtOrY")
```

TeStDiReCtOrY という名前のディレクトリが作成されます。 ディレクトリやファイルの名前を変更し、大文字を小文字に変えるか、小文字を大文字に変えると、その名前変更時の大文字/小文字の使い方がディレクトリ名またはファイル名に反映されます。 たとえば、次のコードでは test.txt というファイルの名前が Test.txt に変更されます。

[!code-csharp[case-and-renaming](~/samples/snippets/standard/io/file-names/cs/rename.cs)]
[!code-vb[case-and-renaming](~/samples/snippets/standard/io/file-names/vb/rename.vb)]

しかしながら、ディレクトリ名とファイル名の比較では、大文字と小文字が区別されません。 "test.txt" という名前のファイルを検索すると、.NET ファイル システム API は比較で大文字/小文字の使い方を無視します。 Test.txt、TEST.TXT、test.TXT、大文字と小文字のその他すべての組み合わせが "test.txt" に一致します。
