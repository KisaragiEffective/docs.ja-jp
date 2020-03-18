---
title: MissingInteropDataException クラス (.NET ネイティブ)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: eab4bcf8-9f5f-4731-87d8-842748a6062a
ms.openlocfilehash: faf14245cd9dd7aa4bf8e89d5a05901279956509
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128279"
---
# <a name="missinginteropdataexception-class-net-native"></a>MissingInteropDataException クラス (.NET ネイティブ)
**Windows アプリ用 .NET (Windows 10 用)、.NET ネイティブのみ**  
  
 この例外は、手動マーシャリング メソッドが呼び出されたが、型のメタデータがスタティック分析でも、ランタイム ディレクティブ ファイルにも見つからない場合にスローされます。  
  
 **名前空間:** System.Runtime.CompilerServices  
  
> [!IMPORTANT]
> `MissingInteropDataException` クラスは、.NET ネイティブツールチェーンによる内部使用のみを目的としています。 サード パーティのコードで使用することを目的としていません。また、アプリケーション コードで、例外を処理する必要はありません。 代わりに、[ランタイム ディレクティブ ファイル](runtime-directives-rd-xml-configuration-file-reference.md)にエントリを追加することにより、例外を除去します。 詳細については、「解説」を参照してください。  
  
## <a name="syntax"></a>構文  
 [!code-csharp[ProjectN#21](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/missinginteropdataexception_syntax1.cs#21)]
 [!code-vb[ProjectN#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/projectn/vb/missinginteropdataexception_syntax1.vb#21)]  
  
 `MissingInteropDataException` クラスには次のメンバーがあります。  
  
## <a name="constructors"></a>コンストラクター  
  
|コンストラクター|説明|  
|-----------------|-----------------|  
|`public MissingInteropDataException(String resourceId, Type pertinentType)`|エラーとデータが欠落している型について説明するシステム提供のメッセージの ID を使用して、`MissingInteropDataException` クラスの新しいインスタンスを初期化します。 このコンストラクターは、.NET ネイティブツールチェーンのみによって内部で使用されます。|  
  
## <a name="properties"></a>プロパティ  
  
|property|説明|  
|--------------|-----------------|  
|`public IDictionary Data { get; }`|例外に関する追加のユーザー定義情報を提供する、キー/値ペアのコレクションを取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public string HelpLink { get; set; }`|この例外に関連付けられているヘルプ ファイルへのリンクを取得または設定します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public int HResult { get; protected set; }`|特定の例外に割り当てられた、コード化された数値である、`HRESULT` を取得または設定します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public Exception InnerException { get; }`|現在の例外を引き起こした例外を取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public string Message { get; }`|現在の例外を説明するメッセージを取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public Type MissingType { get; private set; }`|データが欠落している型を取得または設定します。|  
|`public string Source { get; set; }`|エラーの原因になったアプリまたはオブジェクトの名前を取得または設定します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public string StackTrace { get; }`|呼び出し履歴で直前のフレームの文字列形式を取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public MethodBase TargetSite { get; }`|現在の例外をスローしたメソッドを取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`public bool Equals(Object obj)`|指定したオブジェクトが、現在のオブジェクトと等しいかどうかを判断します。  (<xref:System.Object> から継承。)|  
|`protected void Finalize()`|オブジェクトが、ガベージ コレクションによって収集される前に、リソースの解放とその他のクリーンアップ操作の実行を試みることができるようにします。 (<xref:System.Object> から継承。)|  
|`public Exception GetBaseException()`|1 つ以上の後続の例外の根本原因である例外を返します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public int GetHashCode()`|`MissingInteropDataException` インスタンスのハッシュ コードを返します。   (<xref:System.Object> から継承。)|  
|`public void GetObjectData(SerializationInfo info, StreamingContext context)`|例外に関する情報を使用して、<xref:System.Runtime.Serialization.SerializationInfo> オブジェクトを設定します。  (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`public Type GetType()`|現在のインスタンスのランタイム型を取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
|`protected Object MemberwiseClone()`|現在のオブジェクトの簡易コピーを作成します。 (<xref:System.Object> から継承。)|  
|`public string ToString()`|現在の例外の文字列形式を返します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
  
## <a name="events"></a>イベント  
  
|event|説明|  
|-----------|-----------------|  
|`protected event EventHandler<SafeSerializationEventArgs> SerializeObjectState`|例外がシリアル化され、例外に関するシリアル化されたデータを含む例外状態オブジェクトが作成されたときに発生します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|  
  
## <a name="usage-details"></a>使用方法の詳細  
 `MissingInteropDataException` 例外は、型情報が使用できないために COM または Windows ランタイム コンポーネントへのメソッド呼び出しが正常に行えない場合にスローされます。  
  
 実行時にアプリで使用できるメタデータは、ランタイムディレクティブ (XML 構成) ファイル (\*) によって定義されます。 アプリからこの例外がスローされないようにするには、このファイルを変更して、実行時に存在する必要があるメタデータを定義する必要があります。 このエラーに対する最も一般的な対処法は、ランタイム ディレクティブ ファイルの適切なプログラム要素に `MarshalObject`、`MarshalDelegate`、または `MarshalStructure` 属性を追加することです。 このファイルの形式の詳細については、「[Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)」(ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス) を参照してください。  
  
> [!IMPORTANT]
> この例外はアプリケーションで必要なメタデータを実行時に使用できないことを示しているため、この例外に `try`/`catch` ブロックで対処しないでください。 代わりに、例外の原因を診断し、その原因を解消するために、ランタイム ディレクティブ ファイルに適切なエントリを追加します。  
  
 `MissingInteropDataException` クラスには、正常なメソッド呼び出しのためにどの型のメタデータが必要かを示す、1 つの一意メンバーである `MissingType` プロパティが含まれています。 その他のメンバーはすべて基底クラス <xref:System.Exception?displayProperty=nameWithType> から継承されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Exception?displayProperty=nameWithType>
- [MissingMetadataException クラス](missingmetadataexception-class-net-native.md)
- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
