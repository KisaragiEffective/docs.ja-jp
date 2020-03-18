---
title: フィールドのデザイン
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- fields, design guidelines
- read-only fields
- member design guidelines, fields
ms.assetid: 7cb4b0f3-7a10-4c93-b84d-733f7134fcf8
ms.openlocfilehash: c00929ca499e39bd4e24d482c9413beb9cccddc1
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741612"
---
# <a name="field-design"></a>フィールドのデザイン
カプセル化の原則は、オブジェクト指向設計で最も重要な概念の1つです。 この原則は、オブジェクト内に格納されているデータが、そのオブジェクトに対してのみアクセスできる必要があることを示しています。

 原則を解釈するには、型のメンバーに対して以外のコードを中断することなく、その型のフィールドに対する変更 (名前または型の変更) を行うことができるように、型を設計する必要があるという方法があります。 この解釈は、すべてのフィールドをプライベートにする必要があることを意味します。

 この厳格な制限から、定数および静的な読み取り専用フィールドを除外します。このようなフィールドは、ほとんどの場合定義によって変更する必要がないためです。

 ❌ は、パブリックまたは保護されているインスタンスフィールドを提供しません。

 フィールドにアクセスするためのプロパティは、パブリックまたは保護するのではなく、指定する必要があります。

 変化しない定数には定数フィールドを使用✔️ます。

 コンパイラは、const フィールドの値を直接呼び出しコードに焼きます。 そのため、互換性を損なうことなく const 値を変更することはできません。

 ✔️は、定義済みのオブジェクトインスタンスに対してパブリック静的 `readonly` フィールドを使用します。

 型の定義済みのインスタンスがある場合は、型自体のパブリック読み取り専用の静的フィールドとして宣言します。

 ❌、変更可能な型のインスタンスを `readonly` フィールドに割り当てないでください。

 変更可能な型は、インスタンス化された後に変更できるインスタンスを持つ型です。 たとえば、配列、ほとんどのコレクション、およびストリームは変更可能な型ですが、<xref:System.Int32?displayProperty=nameWithType>、<xref:System.Uri?displayProperty=nameWithType>、および <xref:System.String?displayProperty=nameWithType> はすべて不変です。 参照型フィールドの読み取り専用修飾子は、フィールドに格納されているインスタンスを置換しないようにします。ただし、インスタンスを変更する呼び出し元のメンバーによって、フィールドのインスタンスデータが変更されるのを防ぐことはできません。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [メンバーのデザインのガイドライン](../../../docs/standard/design-guidelines/member.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
