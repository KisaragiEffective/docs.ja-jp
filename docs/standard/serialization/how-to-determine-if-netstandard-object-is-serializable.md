---
title: .NET Standard オブジェクトがシリアル化可能かどうかを判断する方法
description: 実行時に .NET Standard 型をシリアル化できるかどうかを判断する方法について説明します。
ms.date: 10/20/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serializing objects
- objects, serializing steps
ms.openlocfilehash: 4037dee36aeb619eb2757016904fd877158e57cf
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159898"
---
# <a name="how-to-determine-if-a-net-standard-object-is-serializable"></a>.NET Standard オブジェクトがシリアル化可能かどうかを判断する方法

.NET Standard は、そのバージョンの標準に準拠する特定の .NET 実装に存在する必要がある型とメンバーを定義する仕様です。 ただし、.NET Standard では、型をシリアル化できるかどうかは定義されません。 .NET Standard ライブラリで定義されている型は、<xref:System.SerializableAttribute> 属性でマークされていません。 代わりに、特定の型をシリアル化できるかどうかは、.NET Framework や .NET Core などの特定の .NET 実装で自由に決定できます。

.NET Standard を対象とするライブラリを開発した場合は、.NET Standard をサポートするすべての .NET 実装でライブラリを使用できます。 これは、特定の型をシリアル化できるかどうかを事前に知ることができないことを意味します。実行時にシリアル化できるかどうかのみを判断できます。

オブジェクトの型を表す <xref:System.Type> オブジェクトの <xref:System.Type.IsSerializable> プロパティの値を取得することによって、実行時にオブジェクトをシリアル化できるかどうかを判断できます。 次の例では、1つの実装を示します。 <xref:System.Object> インスタンスをシリアル化できるかどうかを示す `IsSerializable(Object)` 拡張メソッドを定義します。

[!code-csharp[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#2)]
[!code-vb[is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/library.vb#2)]

次の例に示すように、任意のオブジェクトをメソッドに渡して、現在の .NET 実装でシリアル化および逆シリアル化できるかどうかを判断できます。

[!code-csharp[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/csharp/program.cs#1)]
[!code-vb[test-is-a-type-serializable](~/samples/snippets/standard/serialization/is-serializable/vb/program.vb#1)]

## <a name="see-also"></a>参照

- [バイナリシリアル化](binary-serialization.md)
- <xref:System.SerializableAttribute?displayProperty=nameWithType>
- <xref:System.Type.IsSerializable?displayProperty=nameWithType>
