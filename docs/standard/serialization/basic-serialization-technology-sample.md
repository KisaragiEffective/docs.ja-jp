---
title: 基本的なシリアル化の技術サンプル
ms.date: 03/30/2017
ms.assetid: 9d824e16-08d1-4a36-bc7f-2388c1f75f34
ms.openlocfilehash: e5dcc9ec7cf6f996c97262b14020552286c530da
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71353138"
---
# <a name="basic-serialization-technology-sample"></a>基本的なシリアル化の技術サンプル

[サンプルのダウンロード](https://download.microsoft.com/download/4/7/B/47B2164C-E780-4B10-8DE4-2CB5B886E0A6/Technologies/Serialization/Runtime%20Serialization/Basic.zip.exe)

このサンプルでは、メモリ内のオブジェクト グラフをシリアル化してストリームに変換する、共通言語ランタイムの機能の例を示します。 このサンプルでは、<xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> または <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> のいずれかを使用して、シリアル化を行います。 データを格納したリンク リストは、ファイル ストリームとの間でシリアル化または逆シリアル化されます。 いずれの場合でも、リストが表示されるので、結果を参照できます。 このリンク リストは `LinkedList` の一種で、このサンプルで定義してある型です。

シリアル化の詳細については、ソース コード ファイルおよび build.proj ファイル内のコメントを参照してください。

### <a name="to-build-the-sample-using-the-command-prompt"></a>コマンド プロンプトを使用してサンプルをビルドするには

1. コマンド プロンプトを使用して、Technologies\Serialization\Runtime Serialization\Basic ディレクトリにある、言語固有のサブディレクトリのいずれかに移動します。

2. 選択したプログラミング言語に応じて、コマンド ラインで、「**msbuild SerializationCS.sln**」、「**msbuild SerializationJSL.sln**」、または「**msbuild SerializationVB.sln**」と入力します。

### <a name="to-build-the-sample-using-visual-studio"></a>Visual Studio を使用してサンプルをビルドするには

1. ファイルエクスプローラーを開き、サンプルの言語固有のサブディレクトリのいずれかに移動します。

2. 使用しているプログラミング言語に応じて、SerializationCS.sln ファイル、SerializationJSL.sln ファイル、または SerializationVB.sln ファイルのアイコンをダブルクリックして、このファイルを Visual Studio で開きます。

3. **[ビルド]** メニューの **[ソリューションのビルド]** を選択します。

 サンプル アプリケーションは、既定の \bin サブディレクトリまたは \bin\Debug サブディレクトリにビルドされます。

### <a name="to-run-the-sample"></a>サンプルを実行するには

1. ビルドした実行可能ファイルが格納されているディレクトリに移動します。

2. コマンド ラインで、「**Serialization.exe**」と入力し、任意のパラメーター値を指定します。

  > [!NOTE]
  > このサンプルでは、コンソール アプリケーションをビルドします。 出力を表示するには、コマンド プロンプトでこれを実行する必要があります。

## <a name="remarks"></a>コメント

このサンプル アプリケーションは、実行するテストを示すコマンド ライン パラメーターを受け取ります。 SOAP フォーマッタを使用して、ノード数 10 個のリストを **Test.xml** というファイルにシリアル化するには、**sx Test.xml 10** というパラメーターを使用します。

たとえば、次のように入力します。

```console
Serialize.exe -sx Test.xml 10
```

前述の例の **Test.xml** ファイルを逆シリアル化するには、**dx Test.xml** というパラメーターを使用します。

たとえば、次のように入力します。

```console
Serialize.exe -dx Test.xml
```

上の 2 つの例で、コマンド ライン スイッチに指定した "x" は、XML SOAP シリアル化を行うことを示しています。 バイナリ シリアル化を使用するには、代わりに "b" を使用します。 ノード数が非常に多いデータをシリアル化する場合は、コンソール出力をファイルにリダイレクトすると便利です。

たとえば、次のように入力します。

```console
Serialize.exe -sb Test.bin 10000 >somefile.txt
```

このサンプルで使用するクラスと技術についての簡単な説明を、以下に箇条書きします。

- ランタイム シリアル化

  - <xref:System.Runtime.Serialization.IFormatter> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> または <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> オブジェクトを参照するために使用されます。

  - <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>、バイナリ形式でリンクリストをストリームにシリアル化するために使用されます。 バイナリ フォーマッタでは、<xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 型でのみ理解できる形式を使用します。 ただし、データは簡潔です。

  - <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>、SOAP 形式でリンクリストをストリームにシリアル化するために使用されます。 SOAP は標準の形式です。

- ストリーム入出力

  - <xref:System.IO.Stream> シリアル化と逆シリアル化に使用します。 このサンプルで使用する特有のストリーム型として、<xref:System.IO.FileStream> 型があります。 ただし、シリアル化は、<xref:System.IO.Stream> から派生する任意の型で行うことができます。

  - <xref:System.IO.File>、ディスク上のファイルの読み取りと作成のための <xref:System.IO.FileStream> オブジェクトを作成するために使用されます。

  - リンクリストをシリアル化および逆シリアル化するために使用 <xref:System.IO.FileStream> ます。

## <a name="see-also"></a>参照

- <xref:System.IO>
- <xref:System.IO.File>
- <xref:System.IO.FileStream>
- <xref:System.IO.Stream>
- <xref:System.Random>
- <xref:System.Runtime.Serialization>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
- <xref:System.Runtime.Serialization.IFormatter>
- <xref:System.SerializableAttribute>
- <xref:System.Xml.Serialization>
- [基本的なシリアル化](../../../docs/standard/serialization/basic-serialization.md)
- [バイナリ シリアル化](../../../docs/standard/serialization/binary-serialization.md)
- [属性を使用した XML シリアル化の制御](../../../docs/standard/serialization/controlling-xml-serialization-using-attributes.md)
- [XML シリアル化の概要](../../../docs/standard/serialization/introducing-xml-serialization.md)
- [シリアル化](../../../docs/standard/serialization/index.md)
- [XML シリアル化および SOAP シリアル化](../../../docs/standard/serialization/xml-and-soap-serialization.md)
