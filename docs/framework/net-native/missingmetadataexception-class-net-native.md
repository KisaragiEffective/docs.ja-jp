---
title: MissingMetadataException クラス (.NET ネイティブ)
ms.date: 03/30/2017
ms.assetid: 408f25c4-6d60-475c-92b1-7b52b777c6db
ms.openlocfilehash: d73d66529bc30358c946eb0a7072f0cb8910b19a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128290"
---
# <a name="missingmetadataexception-class-net-native"></a>MissingMetadataException クラス (.NET ネイティブ)

**Windows アプリ用 .NET (Windows 10 用)、.NET ネイティブのみ**

この例外は、存在しないメタデータを取得するためにリフレクションが使用された場合にスローされます。

**名前空間:** System.Reflection

> [!IMPORTANT]
> `MissingMetadataException` クラスは、.NET ネイティブツールチェーンによる内部使用のみを目的としています。 サード パーティのコードで使用することを目的としていません。また、アプリケーション コードで、例外を処理する必要はありません。 代わりに、[ランタイム ディレクティブ ファイル](runtime-directives-rd-xml-configuration-file-reference.md)にエントリを追加することにより、例外を除去します。 詳細については、「解説」を参照してください。

## <a name="syntax"></a>構文

[!code-csharp[ProjectN#4](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/missingmetadataexception_syntax1.cs#4)]

`MissingMetadataException` クラスが <xref:System.TypeAccessException> から派生していることに注意してください。

`MissingMetadataException` クラスには次のメンバーがあります。

## <a name="constructors"></a>コンストラクター

|コンストラクター|説明|
|-----------------|-----------------|
|`public MissingMetadataException()`|エラーを説明するシステム提供のメッセージを使用して、`MissingMetadataException` クラスの新しいインスタンスを初期化します。<br /><br /> このコンストラクターは、.NET ネイティブツールチェーンのみによって内部で使用されます。|
|`public MissingMetadataException(String message)`|指定したエラー メッセージを使用して、`MissingMetadataException` クラスの新しいインスタンスを初期化します。<br /><br /> このコンストラクターは、.NET ネイティブツールチェーンのみによって内部で使用されます。|

## <a name="properties"></a>プロパティ

|property|説明|
|--------------|-----------------|
|`public IDictionary Data { get; }`|例外に関する追加のユーザー定義情報を提供する、キー/値ペアのコレクションを取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public string HelpLink { get; set; }`|この例外に関連付けられているヘルプ ファイルへのリンクを取得または設定します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public int HResult { get; protected set; }`|特定の例外に割り当てられた、コード化された数値である、`HRESULT` を取得または設定します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public Exception InnerException { get; }`|現在の例外を引き起こした例外を取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public string Message { get; }`|現在の例外を説明するメッセージを取得します。 (<xref:System.TypeLoadException> から継承。)|
|`public string Source { get; set; }`|エラーの原因になったアプリケーションまたはオブジェクトの名前を取得または設定します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public string StackTrace { get; }`|呼び出し履歴で直前のフレームの文字列形式を取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public MethodBase TargetSite { get; }`|現在の例外をスローしたメソッドを取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public string TypeName { get; ]`|メタデータが欠落している型の完全修飾名を取得します。 (<xref:System.TypeLoadException> から継承。)|

## <a name="methods"></a>メソッド

|メソッド|説明|
|------------|-----------------|
|`public bool Equals(Object obj)`|指定したオブジェクトが、現在のオブジェクトと等しいかどうかを判断します。  (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`protected void Finalize()`|オブジェクトが、ガベージ コレクションによって収集される前に、リソースの解放とその他のクリーンアップ操作の実行を試みることができるようにします。 (<xref:System.Object> から継承。)|
|`public Exception GetBaseException()`|1 つ以上の後続の例外の根本原因である例外を返します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`public int GetHashCode()`|`MissingMetadataException` インスタンスのハッシュ コードを返します。   (<xref:System.Object> から継承。)|
|`public void GetObjectData(SerializationInfo info, StreamingContext context)`|例外に関する情報を使用して、<xref:System.Runtime.Serialization.SerializationInfo> オブジェクトを設定します。  (<xref:System.TypeLoadException> から継承。)|
|`public Type GetType()`|現在のインスタンスのランタイム型を取得します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|
|`protected Object MemberwiseClone()`|現在のオブジェクトの簡易コピーを作成します。 (<xref:System.Object> から継承。)|
|`public string ToString()`|現在の例外の文字列形式を返します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|

## <a name="events"></a>イベント

|event|説明|
|-----------|-----------------|
|`protected event EventHandler<SafeSerializationEventArgs> SerializeObjectState`|例外がシリアル化され、例外に関するシリアル化されたデータを含む例外状態オブジェクトが作成されたときに発生します。 (<xref:System.Exception?displayProperty=nameWithType> から継承。)|

## <a name="usage-details"></a>使用方法の詳細

`MissingMetadataException` 例外は、アセンブリで使用できないメタデータにアクセスするためにリフレクションが使用されるとスローされます。

実行時にアプリで使用できるメタデータは、ランタイムディレクティブ (XML 構成) ファイル (\*) によって定義されます。 アプリからこの例外がスローされないようにするには、\*.rd.xml を変更して、実行時に存在する必要があるメタデータを定義する必要があります。 \*.rd.xml ファイルの形式の詳細については、「[Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)」(ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス) を参照してください。

> [!IMPORTANT]
> この例外はアプリケーションで必要なメタデータを実行時に使用できないことを示しているため、この例外に `try`/`catch` ブロックで対処しないでください。 代わりに、ランタイム ディレクティブ ファイルを使用して、例外の原因を診断して排除する必要があります。 例外を除去するためにランタイム ディレクティブ ファイルに追加できるエントリを取得するには、次の 2 つのトラブルシューティング ツールのいずれかを使用できます。
>
> - [MissingMetadataException トラブルシューティング ツール](https://dotnet.github.io/native/troubleshooter/type.html) (型の場合)。
> - [MissingMetadataException トラブルシューティング ツール](https://dotnet.github.io/native/troubleshooter/method.html) (メソッドの場合)。

`MissingMetadataException` クラスには一意のメンバーは含まれていません。メンバーはすべて基底クラスの <xref:System.TypeAccessException> から継承されます。

## <a name="see-also"></a>関連項目

- <xref:System.Exception?displayProperty=nameWithType>
- <xref:System.TypeAccessException>
- [MissingInteropDataException クラス](missinginteropdataexception-class-net-native.md)
- [MissingRuntimeArtifactException クラス](missingruntimeartifactexception-class-net-native.md)
- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
