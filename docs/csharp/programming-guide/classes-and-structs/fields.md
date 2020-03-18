---
title: フィールド - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- fields [C#]
ms.assetid: 3cbb2f61-75f8-4cce-b4ef-f5d1b3de0db7
ms.openlocfilehash: 46d4f77a4a490b2acdb5da20b9a477f27c38d410
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77628242"
---
# <a name="fields-c-programming-guide"></a>フィールド (C# プログラミング ガイド)

*フィールド*とは、[クラス](../../language-reference/keywords/class.md)または[構造体](../../language-reference/builtin-types/struct.md)で直接宣言される任意の型の変数です。 フィールドは、それを含んでいる型の*メンバー*です。

クラスまたは構造体には、インスタンス フィールドと静的フィールドのいずれか、またはその両方が含まれる場合があります。 インスタンス フィールドは、型のインスタンスに固有です。 たとえば、クラス T と、そのクラスのインスタンス フィールド F があるとします。型 T のオブジェクトを 2 つ作成した場合、他方のオブジェクトの値に影響を与えることなく、各オブジェクトの F の値を変更できます。 これとは対照的に、クラス自体に属する静的フィールドは、そのクラスのすべてのインスタンスで共有されます。 静的フィールドにアクセスするには、クラス名を使用する必要があります。 インスタンス名を使用して静的フィールドにアクセスすると、[CS0176](../../misc/cs0176.md) コンパイル時エラーが発生します。

通常、フィールドは、プライベートまたは保護されたアクセシビリティを持つ変数に対してのみ使用します。 クラスからクライアント コードに公開するデータは、[メソッド](./methods.md)、[プロパティ](./properties.md)、および[インデクサー](../indexers/index.md)を使用して提供する必要があります。 これらの構成要素を使用して、内部フィールドに間接的にアクセスすることで、無効な値が入力されることを防止できます。 パブリック プロパティによって公開されるデータを格納するプライベート フィールドは、*バッキング ストア*または*バッキング フィールド*と呼ばれます。

一般に、フィールドは、複数のクラス メソッドからアクセスできるようにする必要があり、かつ 1 つのメソッドの有効期間より長く保持する必要があるデータを格納します。 たとえば、暦の日付を表すクラスには、月、日、年を表す 3 つの整数フィールドが存在します。 1 つのメソッドのスコープ外で使用されることのない変数は、メソッド本体内で*ローカル変数*として宣言する必要があります。

フィールドは、フィールドのアクセス レベル、フィールドの型、フィールドの名前の順に指定して、クラス ブロック内で宣言します。 次に例を示します。

[!code-csharp[csProgGuideObjects#61](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#61)]

オブジェクト内のフィールドにアクセスするには、`objectname.fieldname` のように、オブジェクト名の後にピリオドを追加し、その後にフィールド名を続けます。 次に例を示します。

[!code-csharp[csProgGuideObjects#62](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#62)]

フィールドには、フィールドの宣言時に代入演算子を使用して初期値を指定できます。 たとえば、`day` フィールドに `"Monday"` を自動的に代入するには、次の例のように `day` を宣言します。

[!code-csharp[csProgGuideObjects#63](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#63)]

フィールドは、オブジェクト インスタンスのコンストラクターが呼び出される直前に初期化されます。 コンストラクターがフィールドの値を代入すると、フィールドの宣言時に指定された値はすべて上書きされます。 詳細については、「[コンストラクターの使用](./using-constructors.md)」を参照してください。

> [!NOTE]
> フィールドの初期化子は、他のインスタンス フィールドを参照できません。

フィールドは、[public](../../language-reference/keywords/public.md)、[private](../../language-reference/keywords/private.md)、[protected](../../language-reference/keywords/protected.md)、[internal](../../language-reference/keywords/internal.md)、[protected internal](../../language-reference/keywords/protected-internal.md)、または [private protected](../../language-reference/keywords/private-protected.md) としてマークできます。 これらのアクセス修飾子により、クラスのユーザーがフィールドにアクセスする方法が定義されます。 詳細については、「[アクセス修飾子](./access-modifiers.md)」を参照してください。

必要に応じて、フィールドを[静的](../../language-reference/keywords/static.md)に宣言することもできます。 その場合、クラスのインスタンスが存在しなくても、呼び出し元がいつでもフィールドを使用できるようになります。 詳細については、「[静的クラスと静的クラス メンバー](./static-classes-and-static-class-members.md)」を参照してください。

フィールドを [readonly](../../language-reference/keywords/readonly.md) として宣言することもできます。 読み取り専用フィールドには、初期化時またはコンストラクターによる以外は、値を代入できません。 `static readonly` フィールドは基本的に定数と同じですが、C# コンパイラが静的な読み取り専用フィールドの値にアクセスできるのは実行時のみで、コンパイル時はアクセスできない点が異なります。 詳細については、「[定数](./constants.md)」を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [コンストラクターの使用](./using-constructors.md)
- [継承](./inheritance.md)
- [アクセス修飾子](./access-modifiers.md)
- [抽象クラスとシール クラス、およびクラス メンバー](./abstract-and-sealed-classes-and-class-members.md)
