---
title: プロトブーフ メッセージ - WCF 開発者向け gRPC
description: プロトブーフ メッセージが IDL で定義され、C# で生成される方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 5b3d4383de39a3785ef804fec21939a740f54669
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147985"
---
# <a name="protobuf-messages"></a>Protobuf メッセージ

このセクションでは、プロトコル バッファー (Protobuf) メッセージ`.proto`をファイルで宣言する方法について説明します。 フィールドの数値と型の基本的な概念について説明し、コンパイラが生成する C#`protoc`コードを調べています。

この章の残りの部分では、Protobuf でのデータの種類の詳細について説明します。

## <a name="declaring-a-message"></a>メッセージの宣言

Windows コミュニケーション ファウンデーション`Stock`(WCF) では、株式市場取引アプリケーションのクラスは、次の例のように定義できます。

```csharp
namespace TraderSys
{
    [DataContract]
    public class Stock
    {
        [DataMember]
        public int Id { get; set;}
        [DataMember]
        public string Symbol { get; set;}
        [DataMember]
        public string DisplayName { get; set;}
        [DataMember]
        public int MarketId { get; set; }
    }
}
```

Protobuf で同等のクラスを実装するには、ファイル内で宣言`.proto`する必要があります。 コンパイラ`protoc`は、ビルド プロセスの一部として .NET クラスを生成します。

```protobuf
syntax "proto3";

option csharp_namespace = "TraderSys";

message Stock {

    int32 id = 1;
    string symbol = 2;
    string display_name = 3;
    int32 market_id = 4;

}  
```

最初の行は、使用されている構文バージョンを宣言します。 バージョン3の言語は2016年にリリースされました。 gRPC サービスに推奨されるバージョンです。

この`option csharp_namespace`行は、生成された C# 型に使用される名前空間を指定します。 このオプションは、ファイルが他の`.proto`言語用にコンパイルされる場合は無視されます。 Protobuf ファイルには、多くの場合、複数の言語の言語固有のオプションが含まれています。

メッセージ`Stock`定義は、4 つのフィールドを指定します。 それぞれにタイプ、名前、およびフィールド番号があります。

## <a name="field-numbers"></a>フィールド番号

フィールド番号はプロトブーフの重要な部分です。 バイナリエンコードデータのフィールドを識別するために使用されるため、サービスのバージョン間で変更することはできません。 利点は、下位互換性と上位互換性が可能です。 クライアントとサービスは、欠損値の可能性が処理される限り、知らないフィールド番号を無視します。

バイナリ形式では、フィールド番号は型識別子と組み合わされます。 1 から 15 までのフィールド番号は、その型を 1 バイトとしてエンコードできます。 16 から 2,047 までの数値は 2 バイトです。 メッセージに何らかの理由で 2,047 を超えるフィールドが必要な場合は、高く設定できます。 フィールド番号 1 から 15 の単一バイト ID はパフォーマンスが向上するため、最も基本的で頻繁に使用されるフィールドに使用する必要があります。

## <a name="types"></a>型

型宣言では、Protobuf のネイティブ スカラー データ型を使用[します](protobuf-data-types.md)。 この章の残りの部分では、Protobuf の組み込み型について説明し、一般的な .NET 型との関係を示します。

> [!NOTE]
> Protobuf は型を`decimal`ネイティブにサポートしないので`double`、代わりに使用されます。 完全な 10 進精度を必要とするアプリケーションについては、この章の次の[部分の小数点以下のセクション](protobuf-data-types.md#decimals)を参照してください。

## <a name="the-generated-code"></a>生成されたコード

アプリケーションをビルドすると、Protobuf は各メッセージのクラスを作成し、ネイティブ型を C# 型にマップします。 生成された`Stock`型には、次のシグネチャが付けられます。

```csharp
public class Stock
{
    public int Id { get; set; }
    public string Symbol { get; set; }
    public string DisplayName { get; set; }
    public int MarketId { get; set; }
}
```

実際に生成されるコードはこれよりはるかに複雑です。 その理由は、各クラスに、自身をシリアル化し、バイナリ ワイヤ形式に逆シリアル化するために必要なすべてのコードが含まれているためです。

### <a name="property-names"></a>プロパティ名

Protobuf コンパイラは、ファイル`PascalCase`に含まれている`snake_case`が、プロパティ名に適用されることに`.proto`注意してください。 [Protobuf スタイル ガイド](https://developers.google.com/protocol-buffers/docs/style)では、`snake_case`他のプラットフォームのコード生成によって、その規則に対して期待される大文字と小文字が生成されるように、メッセージ定義で使用することをお勧めします。

>[!div class="step-by-step"]
>[前次](protocol-buffers.md)
>[Next](protobuf-data-types.md)
