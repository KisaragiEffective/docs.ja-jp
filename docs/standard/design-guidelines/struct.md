---
title: 構造体のデザイン
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- class library design guidelines [.NET Framework], structures
- deallocating structures
- allocating structures
- value types, structures
- structure design
- type design guidelines, structures
- structures [.NET Framework], design guidelines
ms.assetid: 1f48b2d8-608c-4be6-9ba4-d8f203ed9f9f
ms.openlocfilehash: b6d06bc8a1e8535f1452af0726138abaebfd4951
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743606"
---
# <a name="struct-design"></a>構造体のデザイン
一般的な目的の値型は、ほとんどの場合、構造体であるC#キーワードと呼ばれます。 このセクションでは、一般的な構造体のデザインに関するガイドラインを示します。

 ❌ は、構造体のパラメーターなしのコンストラクターを提供しません。

 このガイドラインに従うことで、配列の各項目に対してコンストラクターを実行しなくても、構造体の配列を作成できます。 ではC# 、構造体にパラメーターなしのコンストラクターを含めることはできないことに注意してください。

 ❌ は、変更可能な値の型を定義しません。

 変更可能な値の型にはいくつかの問題があります。 たとえば、プロパティ getter が値型を返す場合、呼び出し元はコピーを受け取ります。 コピーは暗黙的に作成されるため、開発者は元の値ではなくコピーを変更することを認識していない可能性があります。 また、一部の言語 (特に動的言語) では、変更可能な値型を使用した場合に問題が発生します。これは、ローカル変数でも逆参照した場合にコピーが作成されるためです。

 ✔️、すべてのインスタンスデータが0、false、または null (必要に応じて) に設定されている状態であることを確認します。

 これにより、構造体の配列が作成されたときに無効なインスタンスが誤って作成されるのを防ぐことができます。

 ✔️値型に <xref:System.IEquatable%601> を実装します。

 値型の <xref:System.Object.Equals%2A?displayProperty=nameWithType> メソッドでは、ボックス化が発生しますが、リフレクションを使用するため、既定の実装はあまり効率的ではありません。 <xref:System.IEquatable%601.Equals%2A> のパフォーマンスは大幅に向上し、ボックス化が行われないように実装できます。

 ❌ 明示的に <xref:System.ValueType>を拡張しないでください。 実際、ほとんどの言語ではこれを回避できます。

 一般に、構造体は非常に便利ですが、頻繁にボックス化されない小さな単一の変更できない値に対してのみ使用してください。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [型デザインのガイドライン](../../../docs/standard/design-guidelines/type.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [クラスまたは構造体の選択](../../../docs/standard/design-guidelines/choosing-between-class-and-struct.md)
