---
title: <bindings>
ms.date: 01/22/2018
ms.assetid: b62cd369-5409-4030-8490-9759a462dd3a
ms.openlocfilehash: fe8f620668e35183890b8bba1f254a74c962f8d3
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74139662"
---
# <a name="bindings"></a>\<バインド >

`bindings` 要素を使用して、Windows Communication Foundation (WCF) の標準バインディングとカスタムバインディングのコレクションを構成できます。 各エントリは、その一意の `binding` 属性で識別できる `name` 要素です。 サービスは、`name` を使用してバインディングをリンクすることにより、バインディングを使用します。 .NET Framework 4 以降では、バインドと動作に名前を付ける必要はありません。 既定の構成と無名のバインドおよび動作の詳細については、「 [WCF サービスの](../../../wcf/samples/simplified-configuration-for-wcf-services.md)構成と簡略化された構成の[簡略化](../../../wcf/simplified-configuration.md)」を参照してください。

## <a name="system-provided-bindings"></a>システム指定のバインド

システム指定のバインディングは、WCF メッセージ スタックの複雑さを隠蔽します。 システム指定のバインディングを使用しているアプリケーションは、スタックを完全に制御する必要はありません。 システム指定の各バインディングに公開される属性は、バインディングが処理する使用シナリオに最適な属性です。

システム指定の各バインディングの構成セクションは、バインディングの構成に使用される複数の構成を定義できます。 各構成は、一意の名前によって識別されます。

システム指定のバインディングに要素または属性を追加することはできません。 これを行うには、「[カスタムバインド](#custom-bindings)」セクションの説明に従って、カスタムバインディングを実装する必要があります。 システム指定のバインディングを完全に模倣し、ユーザーアプリケーションが制御する必要がある設定をいくつか追加するカスタムバインディングを定義することができます。  

システム指定のバインディングの一覧については、「[システム指定のバインディング](../../../wcf/system-provided-bindings.md)」を参照してください。

## <a name="custom-bindings"></a>カスタムバインド

カスタム バインディングを使用すると、WCF メッセージ スタックのフル コントロールが可能になります。 個別のバインディングは、メッセージ スタックを、スタック要素の構成要素をスタックに出現する順序で指定することにより定義します。 各要素は、スタックの1つの要素を定義して構成します。 各カスタム バインディング内の `transport` 要素は 1 つだけにする必要があります。 この要素がない場合、メッセージ スタックは不完全です。

要素がスタックに出現する順序は重要です。それは、その順序で操作がメッセージに適用されるためです。 スタック要素で必要な順序を次に示します。  

1. トランザクション (省略可能)  

2. 信頼できるメッセージング (オプション)  

3. セキュリティ (省略可能)  

4. エンコーダー  

5. Transport  

 カスタム バインドは、`name` 属性によって識別されます。 カスタムバインディングの詳細については、「[カスタムバインド](../../../wcf/extending/custom-bindings.md)」を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Configuration.BindingsSection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.Binding?displayProperty=nameWithType>
- <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType>
- [バインディング](../../../wcf/bindings.md)
- [カスタム バインディング](../../../wcf/extending/custom-bindings.md)
- [\<customBinding >](custombinding.md)
