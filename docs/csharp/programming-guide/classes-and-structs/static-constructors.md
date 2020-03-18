---
title: 静的コンストラクター - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- static constructors [C#]
- constructors [C#], static
ms.assetid: 151ec95e-3c4d-4ed7-885d-95b7a3be2e7d
ms.openlocfilehash: 744bcacbc38299c0ef7d16e814c415ec5caf95dd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170118"
---
# <a name="static-constructors-c-programming-guide"></a>静的コンストラクター (C# プログラミング ガイド)
静的コンストラクターは、任意の [static](../../language-reference/keywords/static.md) データを初期化するため、または 1 回だけ実行する必要がある特定のアクションを実行するために使います。 最初のインスタンスが作成され前、または静的メンバーが参照される前に、自動的に呼び出されます。  
  
 [!code-csharp[csProgGuideObjects#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#14)]  

## <a name="remarks"></a>Remarks
静的コンストラクターには、次の特徴があります。  
  
- 静的コンストラクターはアクセス修飾子を取らず、パラメーターはありません。  

- クラスまたは構造体は、静的コンストラクターを 1 つだけ持つことができます。

- 静的コンストラクターを継承またはオーバーロードすることはできません。

- 静的コンストラクターは、直接呼び出すことはできず、共通言語ランタイム (CLR) によって呼び出されることだけが意図されています。 自動的に呼び出されます。

- ユーザーは、プログラムで静的コンストラクターが実行されるタイミングを制御できません。
  
- 静的コンストラクターは、最初のインスタンスが作成され前、または静的メンバーが参照される前に、[クラス](../../language-reference/keywords/class.md)を初期化するために自動的に呼び出されます。 静的コンストラクターは、インスタンス コンストラクターの前に実行されます。 イベントまたはデリゲートに割り当てられている静的メソッドが呼び出されるときは型の静的コンストラクターが呼び出されますが、割り当てられるときは呼び出されません。 静的フィールド変数初期化子が静的コンストラクターのクラスに存在する場合、初期化子は、静的コンストラクターの実行の直前に、クラス宣言に出現するテキストの順序で実行されます。

- 静的フィールドを初期化するための静的コンストラクターを指定しないと、すべての静的フィールドは、「[C# 型の既定値](../../language-reference/builtin-types/default-values.md)」で示されている既定値に初期化されます。
  
- 静的コンストラクターが例外をスローした場合、ランタイムがその静的コンストラクターを再度呼び出すことはなく、その型は、プログラムが実行しているアプリケーション ドメインの有効期間中、初期化されないままになります。 ほとんどの場合、静的コンストラクターで型をインスタンス化できないとき、または静的コンストラクター内でハンドルされない例外が発生したときは、<xref:System.TypeInitializationException> 例外がスローされます。 ソース コードで明示的に定義されていない暗黙的な静的コンストラクターでは、トラブルシューティングの際に中間言語 (IL) のコードを調べることが必要になる場合があります。

- 静的コンストラクターが存在すると、<xref:System.Reflection.TypeAttributes.BeforeFieldInit> 型属性を追加できません。 これにより、ランタイムの最適化が制限されます。

- `static readonly` として宣言されているフィールドは、その宣言の一部として、または静的コンストラクター内でのみ、割り当てることができます。 明示的な静的コンストラクターが必要ない場合は、ランタイムのより適切な最適化のため、静的コンストラクターではなく、宣言時に静的フィールドを初期化します。

> [!Note]
> 直接アクセスすることはできませんが、初期化の例外のトラブルシューティングを助けるため、明示的な静的コンストラクターが存在することを文書化する必要があります。

### <a name="usage"></a>使用方法

- 静的コンストラクターの一般的な用途は、クラスがログ ファイルを使っていて、このファイルにエントリを書き込むためにコンストラクターが使われる場合です。  
- コンストラクターが `LoadLibrary` メソッドを呼び出すことができる場合は、アンマネージ コードのラッパー クラスを作成するときにも静的コンストラクターが役に立ちます。  

- 静的コンストラクターは、制約 (型パラメーターの制約) によりコンパイル時にチェックできない型パラメーターに対して、実行時にチェックを強制するのに適した場所でもあります。

## <a name="example"></a>例
 この例では、`Bus` クラスに静的コンストラクターがあります。 `Bus` の最初のインスタンスが作成されるとき (`bus1`)、静的コンストラクターが呼び出されてクラスが初期化されます。 サンプルの出力では、`Bus` のインスタンスが 2 つでも静的コンストラクターは 1 回だけ実行されること、およびインスタンス コンストラクターの実行前に静的コンストラクターが実行されることがわかります。  
  
 [!code-csharp[csProgGuideObjects#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#15)]

## <a name="c-language-specification"></a>C# 言語仕様
詳しくは、[C# 言語仕様](~/_csharplang/spec/introduction.md)の「[Static constructors (静的コンストラクター)](~/_csharplang/spec/classes.md#static-constructors)」セクションをご覧ください。
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
- [クラスと構造体](./index.md)
- [コンストラクター](./constructors.md)
- [静的クラスと静的クラス メンバー](./static-classes-and-static-class-members.md)
- [ファイナライザー](./destructors.md)
- [コンストラクターのデザインのガイドライン](../../../standard/design-guidelines/constructor.md#type-constructor-guidelines)
- [セキュリティの警告 - CA2121: 静的コンストラクターはプライベートでなければなりません](https://docs.microsoft.com/visualstudio/code-quality/ca2121-static-constructors-should-be-private)
