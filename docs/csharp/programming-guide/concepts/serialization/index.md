---
title: シリアル化 (C#)
ms.date: 01/02/2020
ms.openlocfilehash: d914298a370b09307e84c88959542b4823cf37ce
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79167596"
---
# <a name="serialization-c"></a>シリアル化 (C#)

シリアル化は、オブジェクトを格納するか、メモリ、データベース、またはファイルに転送するためにバイト ストリームに変換するプロセスです。 その主な目的は、必要なときに再作成できるように、オブジェクトの状態を保存しておくことです。 逆のプロセスは、逆シリアル化と呼ばれます。

## <a name="how-serialization-works"></a>シリアル化のしくみ

この図は、シリアル化の全体的なプロセスを示しています:

![シリアル化グラフィック](./media/index/serialization-process.gif)

オブジェクトが、データを格納するストリームにシリアル化されます。 ストリームには、バージョン、カルチャ、アセンブリ名など、オブジェクトの種類に関する情報が含まれている場合もあります。 そのストリームから、オブジェクトをデータベース、ファイル、またはメモリに格納できます。

### <a name="uses-for-serialization"></a>シリアル化の使用方法

開発者は、シリアル化を使用して、オブジェクトの状態を保存し、必要に応じて、オブジェクトのストレージとデータ交換を指定することで、オブジェクトを再作成することができます。 開発者は、シリアル化を使用して次のようなアクションを実行できます。

* Web サービスを使用してオブジェクトをリモート アプリケーションに送信する
* オブジェクトをあるドメインから別のドメインに渡す
* ファイアウォールを介してオブジェクトを JSON または XML 文字列として渡す
* アプリケーション間でセキュリティまたはユーザー固有の情報を保持する

## <a name="json-serialization"></a>JSON シリアル化

<xref:System.Text.Json> 名前空間には、JavaScript Object Notation (JSON) シリアル化および逆シリアル化のためのクラスが含まれています。 JSON は、Web 経由でデータを共有するために一般的に使用されるオープン標準です。

JSON シリアル化では、オブジェクトのパブリック プロパティが、[RFC 8259 JSON 仕様](https://tools.ietf.org/html/rfc8259)に準拠した文字列、バイト配列、またはストリームにシリアル化されます。 <xref:System.Text.Json.JsonSerializer> でクラスのインスタンスをシリアル化または逆シリアル化する方法を制御するには、次の処理を行います。

* <xref:System.Text.Json.JsonSerializerOptions> オブジェクトを使用する
* <xref:System.Text.Json.Serialization> 名前空間からクラスまたはプロパティに属性を適用する
* [カスタム コンバーターを実装する](../../../../standard/serialization/system-text-json-converters-how-to.md)

## <a name="binary-and-xml-serialization"></a>バイナリ シリアル化と XML シリアル化

<xref:System.Runtime.Serialization> 名前空間には、バイナリおよび XML シリアル化および逆シリアル化のためのクラスが含まれています。

バイナリ シリアル化では、バイナリ エンコードを使用して、ストレージやソケット ベースのネットワーク ストリームなどのためのコンパクトなシリアル化を生成します。 バイナリ シリアル化では、読み取り専用メンバーも含め、すべてのメンバーがシリアル化され、パフォーマンスが向上します。

XML シリアル化では、オブジェクトのパブリック フィールドやパブリック プロパティ、またはメソッドのパラメーターや戻り値を、特定の XML スキーマ定義言語 (XSD) ドキュメントに準拠する XML ストリームにシリアル化します。 XML シリアル化では、XML に変換されるパブリック プロパティとパブリック フィールドによって厳密に型指定されたクラスが生成されます。 <xref:System.Xml.Serialization> には、XML のシリアル化および逆シリアル化のためのクラスが含まれています。 属性をクラスおよびクラス メンバーに適用すると、<xref:System.Xml.Serialization.XmlSerializer> がそのクラスのインスタンスをシリアル化または逆シリアル化する方法を制御できます。

### <a name="making-an-object-serializable"></a>オブジェクトをシリアル化可能にする

バイナリまたは XML シリアル化には、次が必要です。

* シリアル化対象のオブジェクト
* シリアル化されたオブジェクトを格納するストリーム
* <xref:System.Runtime.Serialization.Formatter?displayProperty=fullName> のインスタンス

この型のインスタンスをシリアル化できることを示すには、<xref:System.SerializableAttribute> 属性を適用します。 型に <xref:System.SerializableAttribute> 属性が適用されていない状態でシリアル化しようとすると、例外がスローされます。

フィールドがシリアル化されないようにするには、<xref:System.NonSerializedAttribute> 属性を適用します。 シリアル化できる型のフィールドに、特定の環境に固有のポインター、ハンドル、その他のデータ構造が含まれているときに、そのフィールドを別の環境で意味があるように再構成できない場合は、シリアル化不可にすることができます。

シリアル化されたクラスに、<xref:System.SerializableAttribute> とマークされている他のクラスのオブジェクトへの参照が含まれている場合は、これらのオブジェクトもシリアル化されます。

### <a name="basic-and-custom-serialization"></a>基本的なシリアル化とカスタムのシリアル化

バイナリおよび XML シリアル化は、2 つの方法で実行できます (基本およびカスタム)。

基本的なシリアル化では、.NET Framework を使用して、オブジェクトが自動的にシリアル化されます。 唯一の要件は、クラスに <xref:System.SerializableAttribute> 属性が適用されていることです。 <xref:System.NonSerializedAttribute> を使用して、特定のフィールドがシリアル化されないようにすることができます。

基本的なシリアル化を使用する場合、オブジェクトのバージョン管理によって問題が発生する場合があります。 バージョン管理の問題が重要な場合は、カスタムのシリアル化を使用します。 基本的なシリアル化は、シリアル化を実行する最も簡単な方法ですが、プロセスを細かく制御することはできません。

カスタムのシリアル化では、シリアル化するオブジェクトとシリアル化の方法を正確を指定できます。 クラスを <xref:System.SerializableAttribute> でマークし、<xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装する必要があります。 カスタムの方法でオブジェクトを逆シリアル化する場合は、カスタム コンストラクターを使用します。

## <a name="designer-serialization"></a>デザイナーのシリアル化

デザイナーのシリアル化はシリアル化の特殊な形式であり、開発ツールに関連付けられているオブジェクトの永続性の種類を含みます。 デザイナーのシリアル化は、後でオブジェクト グラフを復元できるように、オブジェクト グラフをソース ファイルに変換するプロセスです。 ソース ファイルには、コードとマークアップを含めることができますが、SQL テーブル情報を含めることもできます。

## <a name="BKMK_RelatedTopics"></a>関連トピックと例  

[System.Text.Json の概要](../../../../standard/serialization/system-text-json-overview.md)。`System.Text.Json` ライブラリを取得する方法を示します。

[.NET で JSON をシリアル化および逆シリアル化する方法](../../../../standard/serialization/system-text-json-how-to.md)。
<xref:System.Text.Json.JsonSerializer> クラスを使用して JSON との間でオブジェクト データの読み取りと書き込みを行う方法について説明します。

[チュートリアル: オブジェクトの永続化 (Visual Studio) (C#)](walkthrough-persisting-an-object-in-visual-studio.md)  
シリアル化によってインスタンス間でオブジェクトのデータを永続化して値を保存しておき、次にそのオブジェクトをインスタンス化するときにその値を取得する方法を示します。

[XML ファイルからオブジェクト データを読み取る方法 (C#)](how-to-read-object-data-from-an-xml-file.md)  
<xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、XML ファイルに以前に書き込まれたオブジェクト データを読み込む方法を示します。

[XML ファイルにオブジェクト データを書き込む方法 (C#)](how-to-write-object-data-to-an-xml-file.md)  
<xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、クラスから XML ファイルにオブジェクトを書き込む方法を示します。
