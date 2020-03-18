---
title: クラスまたは構造体の選択
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], structures
- class library design guidelines [.NET Framework], classes
- structures [.NET Framework], vs. classes
- classes [.NET Framework], design guidelines
- type design guidelines, structures
- structures [.NET Framework], design guidelines
- classes [.NET Framework], vs. structures
- type design guidelines, classes
ms.assetid: f8b8ec9b-0ba7-4dea-aadf-a93395cd804f
ms.openlocfilehash: 76042bef1475f2fdf14e309390dcba6654ccfaa9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741754"
---
# <a name="choosing-between-class-and-struct"></a>クラスまたは構造体の選択
すべてのフレームワーク設計者が直面する基本的な設計上の決定の 1 つが、ある型をクラス (参照型) として設計するか、構造体 (値型) として設計するかという点です。 この決定を下すためには、参照型と値の型の動作の違いをよく理解しておく必要があります。

 取り上げる最初の違いは、メモリー割り当てと解放に関するものです。参照型はヒープ メモリに割り当てられガベージ コレクションの対象となります。一方、値型はスタック メモリか、それを含む型の内部に割り当てられ、スタックがアンワインドされるかそれを含む型が解放される時に解放されます。 そのため、一般的に値型の割り当てと解放は、参照型よりも低コストです。

 次の違いは配列に関するものです。参照型の配列では、インスタンスは配列外に割り当てられます。 つまり、配列要素はヒープ上にあるインスタンスへの参照であるということです。 値型の配列はインラインです。つまり、配列要素は値型の実際のインスタンスであるということです。 したがって、値型の配列の割り当てと解放は、参照型の配列と比べてかなり低コストになります。

 さらに、ほとんどのケースでは、値型の配列では参照の局所性がとても良くなります。 値型は、参照型やそれが実装するインターフェイスにキャストされる時にボックス化されます。 そして、値型に再キャストされたときにボックス化解除されます。 ボックス化されたオブジェクトはヒープに割り当てられ、ガベージ コレクションの対象となるため、ボックス化とボックス化解除があまりに頻繁に行われると、ヒープ メモリ、ガベージ コレクター、そして最終的にはアプリケーション全体のパフォーマンスに悪影響を及ぼす可能性があります。  これに対し、参照型はキャストされるため、このようなボックス化は行われません。 (詳細については、「[ボックス化とボックス化解除](../../csharp/programming-guide/types/boxing-and-unboxing.md)」を参照してください)。

 次の点は、コピーに関するものです。参照型が参照のみをコピーする一方、値型は値全体をコピーします。 そのため、大きな参照型の割り当ては、大きな値型の割り当てよりも低コストです。

 最後の点です。値型は値渡しされますが、参照型は参照渡しされます。 参照型インスタンスへの変更は、そのインスタンスを指すすべての参照に影響します。 値型インスタンスは、値渡しされるときにコピーされます。 値型インスタンスが変更されたとしても、その他のコピーにはもちろん影響しません。 ユーザーが明示的にそうしなくても、引数を渡したり戻り値を返したりするときに暗黙的にコピーが作成されるため、変更可能な値型は、多くのユーザーにとって混乱を招く可能性があります。 そのため、値型は変更不可能であるべきです。

 一般に、フレームワーク内の型の大部分は、クラスであるべきです。 ただし、状況によっては値型の特性により構造体を使用する方が適切な場合もあります。

 型のインスタンスが小さく、通常は短い有効期間であるか、または他のオブジェクトに埋め込まれている場合は、クラスではなく構造体を定義する✔️ます。

 型に次の特性がすべて含まれている場合を除き、構造体を定義しないように ❌ します。

- これは、プリミティブ型 (`int`、`double`など) と同様に、単一の値を論理的に表します。

- インスタンスのサイズは 16 バイト未満である。

- 変更不可である。

- 頻繁にボックス化することはない。

 他のすべてのケースでは、クラスとして、型を定義する必要があります。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [型デザインのガイドライン](../../../docs/standard/design-guidelines/type.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
