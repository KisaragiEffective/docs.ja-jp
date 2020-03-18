---
title: シリアル化
ms.date: 07/20/2015
ms.assetid: 67379a76-5465-4af8-a781-0b0b25a62d9a
ms.openlocfilehash: 9ce97e541cb204b92663464e36d9e8f221ccc3f2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351914"
---
# <a name="serialization-visual-basic"></a>シリアル化 (Visual Basic)
シリアル化は、オブジェクトを格納するか、メモリ、データベース、またはファイルに転送するためにバイト ストリームに変換するプロセスです。 その主な目的は、必要なときに再作成できるように、オブジェクトの状態を保存しておくことです。 逆のプロセスは、逆シリアル化と呼ばれます。  
  
## <a name="how-serialization-works"></a>シリアル化のしくみ  
 この図は、シリアル化の全体的なプロセスを示しています。  
  
![シリアル化グラフィック](./media/index/serialization-process.gif)
  
 オブジェクトは、データだけでなく、バージョン、カルチャ、アセンブリ名などのオブジェクトの型に関する情報も伝達するストリームにシリアル化されます。 そのストリームから、オブジェクトをデータベース、ファイル、またはメモリに格納できます。  
  
### <a name="uses-for-serialization"></a>シリアル化の使用方法  
 開発者は、シリアル化を使用して、オブジェクトの状態を保存し、必要に応じて、オブジェクトのストレージとデータ交換を指定することで、オブジェクトを再作成することができます。 シリアル化を通じて、開発者は、Web サービスによるリモート アプリケーションへのオブジェクトの送信、ドメイン間のオブジェクトの受け渡し、XML 文字列としてのオブジェクトのファイアウォールの通過、アプリケーション間でのセキュリティまたはユーザー固有情報の維持などの操作を実行できます。  
  
### <a name="making-an-object-serializable"></a>オブジェクトをシリアル化可能にする  
 オブジェクトをシリアル化するには、シリアル化するオブジェクト、シリアル化したオブジェクトを格納するストリーム、および <xref:System.Runtime.Serialization.Formatter> が必要です。 <xref:System.Runtime.Serialization> には、オブジェクトのシリアル化と逆シリアル化に必要なクラスが含まれます。  
  
 この型のインスタンスをシリアル化できることを示すには、<xref:System.SerializableAttribute> 属性を適用します。 型に <xref:System.Runtime.Serialization.SerializationException> 属性が適用されていない状態でシリアル化しようとすると、<xref:System.SerializableAttribute> 例外がスローされます。  
  
 クラス内のフィールドをシリアル化しない場合は、<xref:System.NonSerializedAttribute> 属性を適用します。 シリアル化できる型のフィールドに、特定の環境に固有のポインター、ハンドル、その他のデータ構造が含まれているときに、そのフィールドを別の環境で意味があるように再構成できない場合は、シリアル化不可にすることができます。  
  
 シリアル化されたクラスに、<xref:System.SerializableAttribute> とマークされている他のクラスのオブジェクトへの参照が含まれている場合は、これらのオブジェクトもシリアル化されます。  
  
## <a name="binary-and-xml-serialization"></a>バイナリ シリアル化と XML シリアル化  
 バイナリ シリアル化と XML シリアル化の両方を使用できます。 バイナリ シリアル化では、読み取り専用メンバーも含め、すべてのメンバーがシリアル化され、パフォーマンスが向上します。 XML シリアル化では、コードの読みやすさだけではなく、オブジェクトの共有と相互運用を行うための柔軟性が増加します。  
  
### <a name="binary-serialization"></a>バイナリ シリアル化  
 バイナリ シリアル化では、バイナリ エンコードを使用して、ストレージやソケット ベースのネットワーク ストリームなどのためのコンパクトなシリアル化を生成します。  
  
### <a name="xml-serialization"></a>XML シリアル化  
 XML シリアル化では、オブジェクトのパブリック フィールドやパブリック プロパティ、またはメソッドのパラメーターや戻り値を、特定の XML スキーマ定義言語 (XSD) ドキュメントに準拠する XML ストリームにシリアル化します。 XML シリアル化では、XML に変換されるパブリック プロパティとパブリック フィールドによって厳密に型指定されたクラスが生成されます。 <xref:System.Xml.Serialization> には、XML のシリアル化と逆シリアル化に必要なクラスが含まれます。  
  
 属性をクラスおよびクラス メンバーに適用すると、<xref:System.Xml.Serialization.XmlSerializer> がそのクラスのインスタンスをシリアル化または逆シリアル化する方法を制御できます。  
  
## <a name="basic-and-custom-serialization"></a>基本的なシリアル化とカスタムのシリアル化  
 シリアル化は、2 つの方法で実行できます (基本およびカスタム)。 基本的なシリアル化では、.NET Framework を使用して、オブジェクトが自動的にシリアル化されます。  
  
### <a name="basic-serialization"></a>基本的なシリアル化  
 基本的なシリアル化の唯一の要件は、オブジェクトに <xref:System.SerializableAttribute> 属性が適用されていることです。 <xref:System.NonSerializedAttribute> を使用して、特定のフィールドがシリアル化されないようにすることができます。  
  
 基本的なシリアル化を使用した場合、オブジェクトのバージョン管理で問題が発生することがあります。この場合は、カスタムのシリアル化を使用することをお勧めします。 基本的なシリアル化は、シリアル化を実行する最も簡単な方法ですが、プロセスを細かく制御することはできません。  
  
### <a name="custom-serialization"></a>カスタムのシリアル化  
 カスタムのシリアル化では、シリアル化するオブジェクトとシリアル化の方法を正確を指定できます。 クラスを <xref:System.SerializableAttribute> でマークし、<xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装する必要があります。  
  
 カスタムの方法でオブジェクトを逆シリアル化する場合は、カスタム コンストラクターを使用する必要があります。  
  
## <a name="designer-serialization"></a>デザイナーのシリアル化  
 デザイナーのシリアル化はシリアル化の特殊な形式であり、通常は開発ツールに関連付けられているオブジェクトの永続性の種類を含みます。 デザイナーのシリアル化は、後でオブジェクト グラフを復元できるように、オブジェクト グラフをソース ファイルに変換するプロセスです。 ソース ファイルには、コードとマークアップを含めることができますが、SQL テーブル情報を含めることもできます。  
  
## <a name="BKMK_RelatedTopics">関連トピックと例</a>  
 [チュートリアル: Visual Studio でのオブジェクトの永続化 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/walkthrough-persisting-an-object-in-visual-studio.md)  
 シリアル化によってインスタンス間でオブジェクトのデータを永続化して値を保存しておき、次にそのオブジェクトをインスタンス化するときにその値を取得する方法を示します。  
  
 [方法: XML ファイルからオブジェクト データを読み込む (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-read-object-data-from-an-xml-file.md)  
 <xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、XML ファイルに以前に書き込まれたオブジェクト データを読み込む方法を示します。  
  
 [方法: XML ファイルにオブジェクト データを書き込む (Visual Basic)](../../../../visual-basic/programming-guide/concepts/serialization/how-to-write-object-data-to-an-xml-file.md)  
 <xref:System.Xml.Serialization.XmlSerializer> クラスを使用して、クラスから XML ファイルにオブジェクトを書き込む方法を示します。
