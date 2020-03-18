---
title: 静的クラスと静的クラス メンバー - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, static members
- static members [C#]
- static classes [C#]
- C# language, static classes
- static class members [C#]
ms.assetid: 235614b5-1371-4dbd-9abd-b406a8b0298b
ms.openlocfilehash: 7add512b262afbabe996f752c083566a2c394dfd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75705432"
---
# <a name="static-classes-and-static-class-members-c-programming-guide"></a>静的クラスと静的クラス メンバー (C# プログラミング ガイド)

[静的](../../language-reference/keywords/static.md)クラスは基本的には非静的クラスと同じですが、静的クラスはインスタンス化できないという点が異なります。 つまり、[new](../../language-reference/operators/new-operator.md) 演算子を使用して、そのクラス型の変数を作成することはできません。 インスタンス変数がないため、静的クラスのメンバーにアクセスするには、クラス名自体を使用します。 たとえば、`UtilityClass` という静的クラスがあり、`MethodA` というパブリック静的メソッドが定義されている場合、このメソッドを呼び出すには次の例のようにします。  
  
```csharp  
UtilityClass.MethodA();  
```  
  
 静的クラスは、入力パラメーターに対してのみ処理を行い、内部のインスタンス フィールドを取得したり設定したりする必要のない一連のメソッドを格納する、便利なコンテナーとして使用できます。 たとえば、.NET Framework クラス ライブラリでは、静的クラス <xref:System.Math?displayProperty=nameWithType> に、数値演算を実行するメソッドが含まれており、<xref:System.Math> クラスの特定のインスタンスに固有のデータを格納または取得する必要はありません。 つまり、次の例に示すように、クラス名とメソッド名を指定して、クラスのメンバーを適用します。  
  
```csharp  
double dub = -3.14;  
Console.WriteLine(Math.Abs(dub));  
Console.WriteLine(Math.Floor(dub));  
Console.WriteLine(Math.Round(Math.Abs(dub)));  
  
// Output:  
// 3.14  
// -4  
// 3  
```  
  
 すべてのクラス型の場合と同様に、静的クラスの型情報は、.NET Framework の共通言語ランタイム (CLR) によって、そのクラスを参照しているプログラムが読み込まれるときに読み込まれます。 プログラムでは、クラスが読み込まれるタイミングを正確に指定することはできません。 ただし、クラスがプログラム内で最初に参照される前に、そのクラスが読み込まれ、そのフィールドが初期化され、その静的コンストラクターが呼び出されることが保証されます。 静的コンストラクターは一度だけ呼び出され、静的クラスは、プログラムが存在するアプリケーション ドメインの有効期間にわたってメモリに保持されます。  
  
> [!NOTE]
> インスタンスの作成を 1 つしか許可しない非静的クラスを作成する場合は、「[C# でのシングルトンの実装](https://docs.microsoft.com/previous-versions/msp-n-p/ff650316%28v=pandp.10%29)」を参照してください。  
  
 静的クラスの主な特徴を以下に示します。  
  
- 静的メンバーだけが含まれます。  
  
- インスタンス化できません。  
  
- シールされています。  
  
- [インスタンス コンストラクター](./instance-constructors.md)を含めることはできません。  
  
 したがって、静的クラスを作成することと、静的メンバーとプライベート コンストラクターのみを含むクラスを作成することは、基本的に同じです。 プライベート コンストラクターは、クラスのインスタンス化を防ぎます。 静的クラスを使用する利点は、インスタンス メンバーが誤って追加されないことをコンパイラで確認できるという点です。 コンパイラによって、このクラスのインスタンスを作成できないことが保証されます。  
  
 静的クラスはシールされるため、継承できません。 <xref:System.Object> 以外のクラスから継承することはできません。 静的クラスにインスタンス コンストラクターを含めることはできませんが、静的コンストラクターを含めることはできます。 特別な初期化を必要とする静的メンバーがクラスに含まれている場合は、非静的クラスであっても静的コンストラクターを定義する必要があります。 詳細については、「[静的コンストラクター](./static-constructors.md)」を参照してください。  
  
## <a name="example"></a>例  
 次に、摂氏と華氏の間で温度を変換する 2 つのメソッドを含む静的クラスの例を示します。  
  
 [!code-csharp[csProgGuideObjects#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#31)]  
  
## <a name="static-members"></a>静的メンバー  
 非静的クラスには、静的メソッド、フィールド、プロパティ、またはイベントを含めることができます。 静的メンバーは、クラスのインスタンスが作成されていない場合でも、クラスで呼び出すことです。 静的メンバーには、必ずインスタンス名ではなくクラス名でアクセスします。 クラスのインスタンスの作成数に関係なく、静的メンバーのコピーは 1 つしか存在しません。 静的メソッドと静的プロパティは、それを含んでいる型の非静的フィールドや非静的イベントにはアクセスできません。また、メソッド パラメーターに明示的に渡されない限り、どのオブジェクトのインスタンス変数にもアクセスできません。  
  
 クラス全体を静的として宣言するよりも、非静的クラスを宣言して、いくつかの静的メンバーを含める方が一般的です。 静的フィールドの一般的な用途として、インスタンス化されたオブジェクトの数を保持することと、すべてのインスタンスで共有する必要のある値を格納することの 2 つがあります。  
  
 静的メソッドのオーバーロードはできますが、オーバーライドはできません。これは、静的メソッドがクラスのインスタンスではなくクラスに属しているためです。  
  
 フィールドを `static const` として宣言することはできませんが、[const](../../language-reference/keywords/const.md) フィールドは、その動作において本質的に静的です。 これは、型のインスタンスではなく、型に属します。 そのため、const フィールドにアクセスするには、静的フィールドに対して使用するのと同じ `ClassName.MemberName` 表記法を使用します。 オブジェクト インスタンスは必要ありません。  
  
 C# では、静的なローカル変数 (メソッドのスコープで宣言された変数) はサポートされません。  
  
 静的クラスのメンバーを宣言するには、次の例に示すように、メンバーの戻り値の型の前で `static` キーワードを使用します。  
  
 [!code-csharp[csProgGuideObjects#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#29)]  
  
 静的メンバーは初めてアクセスされる前に初期化されます。また、静的コンストラクターがある場合は、それが呼び出される前に初期化されます。 静的クラスのメンバーにアクセスするには、次の例に示すように、変数名の代わりにクラス名を使用してメンバーの位置を指定します。  
  
 [!code-csharp[csProgGuideObjects#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#30)]  
  
 クラスに静的フィールドが含まれている場合は、クラスが読み込まれたときに静的フィールドを初期化する静的コンストラクターを用意します。  
  
 静的メソッドの呼び出しでは、Microsoft Intermediate Language (MSIL) の call 命令が生成されます。これに対して、インスタンス メソッドの呼び出しでは `callvirt` 命令が生成され、null オブジェクト参照もチェックされます。 ただし、ほとんどの場合、2 つの間にパフォーマンス上の違いはそれほどありません。  
  
## <a name="c-language-specification"></a>C# 言語仕様  

詳細については、「[C# 言語仕様](~/_csharplang/spec/classes.md#static-classes)」の[静的クラス](~/_csharplang/spec/classes.md#static-and-instance-members)と[静的およびインスタンス メンバー](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [static](../../language-reference/keywords/static.md)
- [クラス](./classes.md)
- [class](../../language-reference/keywords/class.md)
- [静的コンストラクター](./static-constructors.md)
- [インスタンス コンストラクター](./instance-constructors.md)
