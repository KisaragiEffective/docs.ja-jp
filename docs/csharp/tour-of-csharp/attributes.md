---
title: C# の属性 - C# 言語のツアー
description: C# で属性を使用した宣言型のプログラミングについて
ms.date: 02/27/2020
ms.assetid: 753bcfe2-7ddd-4487-9513-ba70937fc8e9
ms.openlocfilehash: dc5b194c22fc2746ff8b0ab3e550e560a3666bbe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159209"
---
# <a name="attributes"></a>属性

C# プログラムにおける型、メンバー、およびその他のエンティティは、動作の特定の側面を制御する修飾子をサポートします。 たとえばメソッドのアクセシビリティは、`public`、`protected`、`internal`、および `private` 修飾子を使用して制御されます。 C# はこの機能を一般化し、宣言情報のユーザー定義型をプログラム エンティティに追加して実行時に取得できるようにします。 プログラムでは、***属性***を定義して使用することにより、この追加の宣言情報を指定します。

次の例では、プログラムのエンティティに配置して関連するドキュメントへのリンクを提供することができる `HelpAttribute` 属性を宣言しています。

[!code-csharp[AttributeDefined](../../../samples/snippets/csharp/tour/attributes/Program.cs#L3-L20)]

すべての属性クラスは、標準ライブラリによって提供される <xref:System.Attribute> 基底クラスから派生します。 属性は、関連付けられた宣言の直前に、名前を任意の変数とともに角かっこで囲んで与えることにより、適用できます。 属性の名前が `Attribute` 内で終わる場合、属性の参照時に、名前のその部分は省略可能です。 たとえば、`HelpAttribute` は次のように使用できます。

[!code-csharp[AttributeApplied](../../../samples/snippets/csharp/tour/attributes/Program.cs#L22-L28)]

この例では `HelpAttribute` を `Widget` クラスにアタッチしています。 別の `HelpAttribute` をクラス内の `Display` メソッドに追加しています。 属性クラスのパブリック コンストラクターは、属性がプログラム エンティティにアタッチされたときに提供する必要がある情報を制御します。 その属性クラスのパブリックの読み取り/書き込みプロパティを参照することにより (`Topic` プロパティへの参照のような)、追加情報を提供することができます。

属性によって定義されるメタデータは、実行時にリフレクションを使用して読み取り、操作することができます。 この手法で特定の属性が要求されると、プログラム ソースで提供される情報で属性クラスのコンストラクターが呼び出され、作成された属性インスタンスが返されます。 追加情報がプロパティを通じて提供された場合、属性インスタンスが返される前に、これらのプロパティは指定された値に設定されます。

`HelpAttribute` クラスとその `Widget` メソッドに関連付けられた `Display` インスタンスを取得する方法を、次のコード サンプルに示します。

[!code-csharp[AttributeRead](../../../samples/snippets/csharp/tour/attributes/Program.cs#ReadAttributes)]

>[!div class="step-by-step"]
>[[戻る]](delegates.md)
