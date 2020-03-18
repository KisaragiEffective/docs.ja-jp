---
title: C# インターフェイス - C# 言語のツアー
description: C# の型によって実装されるコントラクトを定義するインターフェイス
ms.date: 02/27/2020
ms.assetid: a9bf82f4-efd1-4216-bd34-4ef0fa48c968
ms.openlocfilehash: 62d94462fa481379cf70d63a598deb7f36be204f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159131"
---
# <a name="interfaces"></a>インターフェイス

***インターフェイス***は、クラスと構造体によって実装できるコントラクトを定義します。 1 つのインターフェイスには、メソッド、プロパティ、イベント、およびインデクサーが含まれる場合があります。 インターフェイスでは、定義するメンバーの実装は行いません。インターフェイスを実装するクラスまたは構造体によって提供される必要があるメンバーを指定するだけです。

インターフェイスは***多重継承***を使用する場合があります。 次の例では、インターフェイス `IComboBox` は `ITextBox` と `IListBox` の両方から継承します。

[!code-csharp[InterfacesOne](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L5-L17)]

クラスと構造体は、複数のインターフェイスを実装できます。 次の例では、クラス `EditBox` は `IControl` と `IDataBound` の両方を実装しています。

[!code-csharp[InterfacesTwo](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L19-L27)]

クラスまたは構造体が特定のインターフェイスを実装する場合、そのクラスまたは構造体のインスタンスはそのインターフェイス型に暗黙的に変換できます。 次に例を示します。

[!code-csharp[InterfacesThree](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L33-L35)]

インスタンスが特定のインターフェイスを静的に実装しないとわかっている場合は、動的な型キャストを使用できます。 たとえば、次のステートメントは動的な型キャストを使用して、オブジェクトの `IControl` および `IDataBound` のインターフェイス実装を取得します。 オブジェクトの実行時の実際の型が `EditBox` であるため、キャストは成功します。

[!code-csharp[InterfacesFour](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L40-L42)]

前述の `EditBox` クラスで、`IControl` インターフェイスからの `Paint` メソッドおよび `IDataBound` インターフェイスからの `Bind` メソッドは、パブリック メンバーを使用して実装されています。 C# ではまた、明示的な***インターフェイス メンバーの実装***をサポートしており、クラスまたは構造体がメンバーをパブリックにするのを回避するようにできます。 明示的なインターフェイス メンバーの実装は、完全修飾のインターフェイス メンバー名を使用して書き込まれます。 たとえば、`EditBox` クラスは、次のように明示的なインターフェイス メンバーの実装を使用して、`IControl.Paint` メソッドおよび `IDataBound.Bind` メソッドを実装できます。

[!code-csharp[InterfacesFive](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L60-L64)]

明示的なインターフェイス メンバーは、インターフェイス型を経由してのみアクセスできます。 たとえば、前の EditBox クラスで提供された `IControl.Paint` の実装は、最初に `EditBox` 参照を `IControl` インターフェイス型に変換することでしか呼び出しできません。

[!code-csharp[InterfacesFive](../../../samples/snippets/csharp/tour/interfaces/Program.cs#L71-L74)]

>[!div class="step-by-step"]
>[前へ](arrays.md)
>[次へ](delegates.md)
