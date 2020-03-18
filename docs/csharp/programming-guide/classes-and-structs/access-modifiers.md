---
title: アクセス修飾子 - C# プログラミング ガイド
ms.date: 03/08/2020
helpviewer_keywords:
- C# Language, access modifiers
- access modifiers [C#], about
ms.assetid: 6e81ee82-224f-4a12-9baf-a0dca2656c5b
ms.openlocfilehash: 749d53344a2518966cfa5d937069ba73dfd6be8f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77628229"
---
# <a name="access-modifiers-c-programming-guide"></a>アクセス修飾子 (C# プログラミング ガイド)

すべての型と型メンバーにアクセシビリティ レベルがあります。 同じアセンブリまたは他のアセンブリに他のコードからそれらの型やそのメンバーを利用できるかどうかは、アクセシビリティ レベルによって制御されます。 型またはメンバーにはその宣言時に、以下のアクセス修飾子を使ってアクセシビリティを指定します。

- [public](../../language-reference/keywords/public.md):この型またはメンバーには、同じアセンブリ内の他のコードや、そのアセンブリを参照する別のアセンブリ内の任意のコードからアクセスできます。
- [private](../../language-reference/keywords/private.md):この型またはメンバーには、同じ `class` 内または同じ `struct` 内のコードからのみアクセスできます。
- [protected](../../language-reference/keywords/protected.md):この型またはメンバーには、同じ `class` 内のコードか、その `class` から派生した `class` 内のコードからのみアクセスできます。
- [internal](../../language-reference/keywords/internal.md):この型またはメンバーには、同じアセンブリ内の任意のコードからアクセスできますが、別のアセンブリからはアクセスできません。
- [protected internal](../../language-reference/keywords/protected-internal.md):この型またはメンバーには、それが宣言されているアセンブリ内の任意のコードからアクセスできるほか、別のアセンブリの派生 `class` 内からアクセスすることができます。
- [private protected](../../language-reference/keywords/private-protected.md):この型またはメンバーには、同じ `class` のコードか、その `class` から派生した型のコードによって、その宣言アセンブリ内でのみ、アクセスできます。

次の例は、型とメンバーにアクセス修飾子を指定する方法を示しています。

[!code-csharp[PublicAccess](~/samples/snippets/csharp/objectoriented/accessmodifiers.cs#PublicAccess)]

一部のコンテキスト、型、メンバーでは、アクセス修飾子が無効になります。 場合によっては、ある型のメンバーのアクセシビリティが、それが含まれる型のアクセシビリティによって制約されることがあります。

## <a name="class-and-struct-accessibility"></a>クラスと構造体のアクセシビリティ  

名前空間に直接宣言されている (つまり、他のクラスや構造体の入れ子にされていない) クラスと構造体には、`public` または `internal` を指定できます。 アクセス修飾子が指定されなかった場合は、既定で `Internal` が適用されます。  

構造体のメンバー (入れ子にされているクラスや構造体も含む) は `public`、`internal`、`private` のいずれかとして宣言できます。 クラスのメンバー (入れ子にされているクラスや構造体も含む) は `public`、`protected internal`、`protected`、`internal`、`private protected`、`private` のいずれかになります。 クラスのメンバーと構造体のメンバー (入れ子にされているクラスや構造体も含む) には、既定で `private` のアクセスが与えられます。 入れ子にされた型のうち、private が指定されているものには、それを含んでいる型の外部からはアクセスできません。

派生クラスに、その基本型を超えるアクセシビリティを割り当てることはできません。 内部クラス `A` から派生した public クラス `B` を宣言することはできません。 許可される場合は、`A` を public にする効果が与えられるでしょう。`A` のすべての `protected` または `internal` メンバーに派生クラスからアクセスできるためです。

`InternalsVisibleToAttribute` を使用すると、internal 型へのアクセスを他の特定のアセンブリに許可できます。 詳細については、[Friend アセンブリ](../../../standard/assembly/friend.md)に関するページを参照してください。

## <a name="class-and-struct-member-accessibility"></a>クラスと構造体のメンバーのアクセシビリティ  

クラスのメンバー (入れ子にされているクラスや構造体も含む) は、6 種類あるアクセス修飾子をどれでも使って宣言できます。 構造体のメンバーを `protected`、`protected internal`、`private protected` として宣言することはできません。構造体は継承をサポートしていないためです。

通常、メンバーのアクセシビリティが、それを含んでいる型のアクセシビリティを超えることはありません。 ただし、internal クラスの `public` メンバーには、そのアセンブリの外部からアクセスできる場合もあります。そのメンバーがインターフェイスのメソッドを実装している場合や public な基本クラスに定義されている仮想メソッドをオーバーライドしている場合がそれに該当します。

あらゆるメンバー フィールド、プロパティ、イベントの型には、メンバー自体と同じかそれ以上のアクセシビリティが必要です。 同様に、あらゆるメソッド、インデクサー、デリゲートの戻り値の型とパラメーターの型には、メンバー自体と同じかそれ以上のアクセシビリティが必要です。 たとえば、`public` メソッド `M` でクラス `C` を返すには、`C` が `public` にもなっている必要があります。 同様に、`A` が `private` として宣言されている場合、`A` 型のプロパティを `protected` にすることはできません。

ユーザー定義の演算子は、必ず `public` と `static` として宣言する必要があります。 詳細については、「[演算子のオーバーロード](../../language-reference/operators/operator-overloading.md)」を参照してください。

アクセシビリティ修飾子をファイナライザーに割り当てることはできません。

`class` または `struct` のメンバーにアクセス レベルを設定するには、該当するキーワードをメンバーの宣言に追加します。その例を次に示します。

[!code-csharp[MethodAccess](~/samples/snippets/csharp/objectoriented/accessmodifiers.cs#MethodAccess)]

## <a name="other-types"></a>その他の型

名前空間内に直接宣言されたインターフェイスは、`public` または `internal` にすることができます。クラスや構造体と同様、インターフェイスの既定のアクセス レベルは `internal` になります。 インターフェイスのメンバーは既定で `public` です。他の型がクラスや構造体にアクセスできるようにすることがインターフェイスの目的であるためです。 インターフェイス メンバー宣言には、あらゆるアクセス修飾子を含めることができます。 そのことは、クラスを実装するあらゆるもので必要になる共通実装を静的メソッドから与えるときに最も役に立ちます。

列挙型のメンバーは常に `public` となり、アクセス修飾子を適用することはできません。

デリゲートの振る舞いは、クラスおよび構造体と似ています。 既定では、名前空間内に直接宣言されているときには `internal` アクセスが、入れ子にされているときは `private` アクセスが適用されます。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [クラスと構造体](./index.md)
- [インターフェイス](../interfaces/index.md)
- [private](../../language-reference/keywords/private.md)
- [public](../../language-reference/keywords/public.md)
- [internal](../../language-reference/keywords/internal.md)
- [protected](../../language-reference/keywords/protected.md)
- [protected internal](../../language-reference/keywords/protected-internal.md)
- [private protected](../../language-reference/keywords/private-protected.md)
- [class](../../language-reference/keywords/class.md)
- [struct](../../language-reference/builtin-types/struct.md)
- [interface](../../language-reference/keywords/interface.md)
