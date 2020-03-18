---
title: クラスと構造体 - C# プログラミング ガイド
description: C# でのクラスと構造体の使用について説明します。
ms.date: 01/17/2016
helpviewer_keywords:
- structs [C#], about structs
- classes [C#], overview
- C# language, structs
- C# language, objects
- objects [C#]
- C# language, classes
ms.assetid: cc39dbda-8754-423e-b5b1-16a1db0734c0
ms.openlocfilehash: afd9e688bd716375bafb370fad4af082a9498411
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397660"
---
# <a name="classes-and-structs-c-programming-guide"></a>クラスと構造体 (C# プログラミング ガイド)
クラスと構造体は、.NET Framework の共通型システムの 2 つの基本構成です。 クラスと構造体は、どちらも基本的にはデータと動作のセットを 1 つの論理単位としてカプセル化するデータ構造です。 データと動作はクラスまたは構造体の "*メンバー*" です。このトピックで後述するように、メソッド、プロパティ、イベントなどが含まれます。  
  
 クラスまたは構造体の宣言は、実行時にインスタンスやオブジェクトを作成するために使用する設計図のようなものです。 `Person` というクラスまたは構造体を定義すると、`Person` は型の名前になります。 型 `p` の変数 `Person` を宣言して初期化すると、`p` は `Person` のオブジェクトまたはインスタンスになります。 同じ `Person` 型のインスタンスを複数作成し、各インスタンスのプロパティとフィールドに異なる値を設定することができます。  
  
 クラスは参照型です。 クラスのオブジェクトが作成されると、オブジェクトが割り当てられている変数にはそのメモリへの参照だけが設定されます。 オブジェクト参照が新しい変数に割り当てられると、新しい変数は元のオブジェクトを参照します。 いずれの変数も同じデータを参照しているため、1 つの変数に加えられた変更は他の変数にも反映されます。  
  
 構造体は値の型です。 構造体が作成されると、構造体が割り当てられている変数にはその構造体の実際のデータが設定されます。 構造体が新しい変数に割り当てられると、そのデータがコピーされます。 したがって、新しい変数と元の変数には、同じデータのコピーが別個に含まれることになります。 一方のコピーに対して行われた変更は、もう一方のコピーには影響しません。  
  
 一般に、クラスは、より複雑な動作、つまりクラス オブジェクトの作成後に変更されることを意図されたデータをモデル化するために使用されます。 構造体は、主として構造体の作成後に変更されることを意図しないデータを含む、小規模なデータ構造に最適です。  
  
 詳細については、[クラス](./classes.md)、[オブジェクト](./objects.md)、[構造体型](../../language-reference/builtin-types/struct.md)に関する記事をご覧ください。  
  
## <a name="example"></a>例  
 次の例では、`ProgrammingGuide` 名前空間の `CustomClass` に 3 つのメンバー (インスタンス コンストラクター、`Number` という名前のプロパティ、`Multiply` という名前のメソッド) があります。 `Program` クラスの `Main` メソッドによって `CustomClass` のインスタンス (オブジェクト) が作成され、オブジェクトのメソッドとプロパティにはドット表記を使用してアクセスします。
  
 [!code-csharp[csProgGuideObjects#1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/class1.cs#1)]  
  
## <a name="encapsulation"></a>カプセル化  
 "*カプセル化*" は、オブジェクト指向プログラミングの原理と言われることもあります。 カプセル化の原理に基づくと、クラスまたは構造体は、クラスや構造体の外部のコードに対してメンバーがアクセスする方法を指定できます。 クラスやアセンブリの外部からの使用を意図されていないメソッドや変数は非表示にして、コーディング エラーや悪意のある攻略が生じる潜在性を軽減できます。  
  
 クラスの詳細については、「[クラス](./classes.md)」および「[オブジェクト](./objects.md)」を参照してください。  
  
### <a name="members"></a>メンバー  
 すべてのメソッド、フィールド、定数、プロパティ、およびイベントは型で宣言する必要があります。これらは、型の*メンバー*と呼ばれます。 C# にはグローバル変数やグローバル メソッドはありません (言語によっては、グローバル変数やグローバル メソッドが存在する場合もあります)。 プログラムのエントリ ポイントである `Main` メソッドでも、クラスまたは構造体で宣言する必要があります。 クラスまたは構造体で宣言できるすべてのメンバーを次に示します。  
  
- [フィールド](./fields.md)  
  
- [定数](./constants.md)  
  
- [プロパティ](./properties.md)  
  
- [メソッド](./methods.md)  
  
- [コンストラクター](./constructors.md)  
  
- [イベント](../events/index.md)  
  
- [ファイナライザー](./destructors.md)  
  
- [インデクサー](../indexers/index.md)  
  
- [演算子](../../language-reference/operators/index.md)  
  
- [入れ子にされた型](./nested-types.md)  
  
### <a name="accessibility"></a>ユーザー補助  
 メソッドやプロパティの中には、クラスまたは構造体の外部にあるコード ("*クライアント コード*" と呼ばれます) から呼び出されたりアクセスされたりするように用意されているものがあります。 その他のメソッドやプロパティは、クラスまたは構造体それ自体でのみ使用されるようになっています。 意図したクライアント コードだけがアクセスできるように、コードのアクセシビリティを制限することが重要です。 型やメンバーがクライアント コードにアクセスする方法を指定するには、アクセス修飾子 [public](../../language-reference/keywords/public.md)、[protected](../../language-reference/keywords/protected.md)、[internal](../../language-reference/keywords/internal.md)、[protected internal](../../language-reference/keywords/protected-internal.md)、[private](../../language-reference/keywords/private.md)、および [private protected](../../language-reference/keywords/private-protected.md) を使用します。 既定のアクセシビリティは `private` です。 詳細については、「[アクセス修飾子](./access-modifiers.md)」を参照してください。  
  
### <a name="inheritance"></a>継承  
 クラスでは、継承の概念がサポートされます (構造体ではサポートされません)。 他のクラス (*基底クラス*) から派生するクラスには、コンストラクターとファイナライザーを除く基底クラスのすべてのパブリック メンバー、プロテクト メンバー、および内部メンバーが自動的に含まれます。 詳細については、「[継承](./inheritance.md)」および「[ポリモーフィズム](./polymorphism.md)」を参照してください。  
  
 クラスは、[abstract](../../language-reference/keywords/abstract.md) として宣言することもできます。このようなクラスは、1 つ以上のメソッドの実装を持っていないことを意味します。 抽象クラスを直接インスタンス化することはできませんが、他のクラスの基底クラスとして抽象クラスを使用し、不足している実装を提供することができます。 また、クラスを [sealed](../../language-reference/keywords/sealed.md) として宣言して、他のクラスがそのクラスを継承しないようにすることもできます。 詳細については、「[抽象クラスとシール クラス、およびクラス メンバー](./abstract-and-sealed-classes-and-class-members.md)」を参照してください。  
  
### <a name="interfaces"></a>インターフェイス  
 クラスと構造体は、複数のインターフェイスを継承できます。 インターフェイスの継承は、そのインターフェイスで定義されたすべてのメソッドを型が実装することを意味します。 詳細については、「[インターフェイス](../interfaces/index.md)」を参照してください。  
  
### <a name="generic-types"></a>ジェネリック型  
 クラスと構造体は、1 つ以上の型パラメーターを指定して定義できます。 クライアント コードでは、型のインスタンスの作成時に型を指定します。 たとえば、<xref:System.Collections.Generic.List%601> 名前空間の <xref:System.Collections.Generic> クラスは、1 つの型パラメーターを指定して定義されています。 クライアント コードでは、`List<string>` または `List<int>` のインスタンスを作成して、リストに保持する型を指定します。 詳細については、「[ジェネリック](../generics/index.md)」を参照してください。  
  
### <a name="static-types"></a>静的な型  
 クラス (構造体ではありません) は、[static](../../language-reference/keywords/static.md) として宣言できます。 静的クラスには含めることができるのは静的メンバーだけであり、静的クラスを new キーワードでインスタンス化することはできません。 プログラムが読み込まれると、クラスの 1 つのコピーがメモリに読み込まれます。そのメンバーには、クラス名を使用してアクセスします。 クラスと構造体は、いずれも静的メンバーを含むことができます。 詳細については、「[静的クラスと静的クラス メンバー](./static-classes-and-static-class-members.md)」を参照してください。  
  
### <a name="nested-types"></a>入れ子にされた型  
 クラスまたは構造体は、他のクラスまたは構造体内で入れ子にすることができます。 詳細については、「[入れ子にされた型](./nested-types.md)」を参照してください。  
  
### <a name="partial-types"></a>部分型  
 あるコード ファイルにクラス、構造体、またはメソッドの一部を定義して、別のコード ファイルに他の部分を定義することができます。 詳細については、「[部分クラスと部分メソッド](./partial-classes-and-methods.md)」を参照してください。  
  
### <a name="object-initializers"></a>オブジェクト初期化子  
 明示的にコンストラクターを呼び出さずに、クラス オブジェクトまたは構造体オブジェクト、およびオブジェクトのコレクションのインスタンス化と初期化を行うことができます。 詳細については、「[オブジェクト初期化子とコレクション初期化子](./object-and-collection-initializers.md)」を参照してください。  
  
### <a name="anonymous-types"></a>匿名型  
 永続的に保持したり他のメソッドに渡したりする必要のないデータ構造体のリストを作成する場合など、名前付きクラスを作成することが適さない状況または不要な状況では、匿名型を使用します。 詳細については、「[匿名型](./anonymous-types.md)」を参照してください。  
  
### <a name="extension-methods"></a>拡張メソッド  
 別の型を作成し、元の型に属しているかのようにそのメソッドを呼び出せるようにすることで、派生クラスを作成せずにクラスを "拡張" できます。 詳細については、「[拡張メソッド](./extension-methods.md)」を参照してください。  
  
### <a name="implicitly-typed-local-variables"></a>暗黙的に型指定されるローカル変数  
 クラス メソッドや構造体メソッド内では、暗黙の型指定を使用して、コンパイル時に適切な型を判断するようにコンパイラに指示できます。 詳細については、「[暗黙的に型指定されるローカル変数](./implicitly-typed-local-variables.md)」を参照してください。  
  
## <a name="c-language-specification"></a>C# 言語仕様  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
