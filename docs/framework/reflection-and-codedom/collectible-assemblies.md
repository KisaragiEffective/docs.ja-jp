---
title: 動的な型生成のための収集可能なアセンブリ
description: ''
ms.date: 08/29/2017
helpviewer_keywords:
- reflection, dynamic assembly
- assemblies, collectible
- collectible assemblies, retrieving
ms.openlocfilehash: 85eacff22cf2e1c0b8c3d74a4971de035dfafbe4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130292"
---
# <a name="collectible-assemblies-for-dynamic-type-generation"></a>動的な型生成のための収集可能なアセンブリ

*収集可能なアセンブリ*とは、それらが作成されたアプリケーション ドメインをアンロードせずにアンロードできる動的アセンブリです。 収集可能アセンブリとそれに含まれる型によって使用されているすべてのマネージド メモリとアンマネージド メモリは、再利用することができます。 アセンブリ名などの情報は、内部テーブルから削除されます。

アンロードを有効にするには、動的アセンブリを作成するときに <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndCollect?displayProperty=nameWithType> フラグを使用します。 このアセンブリは一時的なもので (つまり、保存できません)、「[収集可能なアセンブリに対する制限](#restrictions-on-collectible-assemblies)」セクションで説明されている制限事項が該当します。 収集可能なアセンブリは、そのアセンブリに関連付けられているすべてのオブジェクトが解放されると、共通言語ランタイム (CLR) によって自動的にアンロードされます。 その他のすべての点では、収集可能なアセンブリは他の動的アセンブリと同じように作成および使用されます。

## <a name="lifetime-of-collectible-assemblies"></a>収集可能なアセンブリの有効期間

収集可能な動的アセンブリの有効期間は、そのアセンブリに含まれる型への参照があるかどうかと、それらの型から作成されたオブジェクトがあるかどうかによって決まります。 次の 1 つ以上が存在する場合、共通言語ランタイムはアセンブリをアンロードしません (`T` は、アセンブリで定義されている任意の型です)。 

- `T` のインスタンス。

- `T` の配列のインスタンス。
 
- `T` を型引数の 1 つとして含んでいるジェネリック型のインスタンス。 これには、そのコレクションが空の場合も含め、`T` のジェネリック コレクションを含みます。

- <xref:System.Type> を表す <xref:System.Reflection.Emit.TypeBuilder> または `T` のインスタンス。 

   > [!IMPORTANT]
   > アセンブリの一部を表すオブジェクトは、すべて解放する必要があります。 <xref:System.Reflection.Emit.ModuleBuilder> を定義した `T` は <xref:System.Reflection.Emit.TypeBuilder> への参照を保持し、<xref:System.Reflection.Emit.AssemblyBuilder> オブジェクトは <xref:System.Reflection.Emit.ModuleBuilder> への参照を保持します。したがって、これらのオブジェクトへの参照は解放する必要があります。 <xref:System.Reflection.Emit.LocalBuilder> の構築に使用された <xref:System.Reflection.Emit.ILGenerator> や `T` が存在している場合も、アンロードは行われません。

- 実行コードから到達できる動的に定義された別の型 `T` による、`T1` への静的参照。 たとえば、`T1` が `T` から派生している場合や、`T` が `T1` のメソッドのパラメーターの型である場合があります。
 
- **に属する静的フィールドに対する**ByRef`T`。

- <xref:System.RuntimeTypeHandle> または <xref:System.RuntimeFieldHandle> のコンポーネントを参照する <xref:System.RuntimeMethodHandle>、`T`、または `T`。

- <xref:System.Type> を表す `T` オブジェクトへのアクセスに間接または直接に使用できるリフレクション オブジェクトのインスタンス。 たとえば、<xref:System.Type> の `T` オブジェクトは、要素型が `T` である配列型や、`T` を型引数として持つジェネリック型から取得できます。 

- 任意のスレッドの呼び出し履歴にあるメソッド `M`。この `M` は、`T` のメソッド、またはアセンブリで定義されているモジュール レベルのメソッドです。

- アセンブリのモジュール内で定義されている静的メソッドに対するデリゲート。

アセンブリ内の 1 つの型または 1 つのメソッドについてだけでも、この一覧の項目が 1 つでも存在する場合、ランタイムはそのアセンブリをアンロードできません。

> [!NOTE]
> 実際には、この一覧にあるすべての項目に対してファイナライザーが実行されるまで、アセンブリはアンロードされません。

有効期間の追跡では、収集可能なアセンブリの生成時に作成および使用される (C# の) `List<int>` または (Visual Basic の) `List(Of Integer)` などの構築ジェネリック型は、そのジェネリック型定義を含むアセンブリか、その型引数のいずれかの定義を含む別のアセンブリで定義されていると見なされます。 使用される正確なアセンブリは実装の詳細であり、変更されることがあります。
 
## <a name="restrictions-on-collectible-assemblies"></a>収集可能なアセンブリに対する制限

収集可能なアセンブリには、次の制限があります。 

- **静的参照**   
  通常の動的アセンブリ内の型では、収集可能なアセンブリで定義されている型への静的参照を使用できません。 たとえば、収集可能なアセンブリ内の型を継承する通常の型を定義すると、<xref:System.NotSupportedException> 例外がスローされます。 収集可能なアセンブリ内の型では、別の収集可能なアセンブリ内の型への静的参照を使用できますが、この場合は、参照アセンブリの有効期間が参照元アセンブリの有効期間に延長されます。

- **COM 相互運用**   
   収集可能なアセンブリ内では COM インターフェイスを定義できません。また、収集可能なアセンブリ内の型のインスタンスを COM オブジェクトに変換することもできません。 収集可能なアセンブリ内の型は、COM 呼び出し可能ラッパー (CCW) またはランタイム呼び出し可能ラッパー (RCW) として機能できません。 ただし、収集可能なアセンブリ内の型では、COM インターフェイスを実装するオブジェクトを使用できます。

- **プラットフォーム呼び出し**   
   <xref:System.Runtime.InteropServices.DllImportAttribute> 属性を持つメソッドを収集可能なアセンブリ内で宣言しても、コンパイルはできません。 <xref:System.Reflection.Emit.OpCodes.Calli?displayProperty=nameWithType> 命令は、収集可能なアセンブリ内の型の実装では使用できません。このような型をアンマネージ コードにマーシャリングすることはできません。 ただし、収集可能でないアセンブリ内で宣言されているエントリ ポイントを使用すれば、ネイティブ コードを呼び出すことができます。
 
- **マーシャリング**   
   収集可能なアセンブリで定義されているオブジェクト (特にデリゲート) はマーシャリングできません。 この制限は、一時的に生成されるすべての型にあります。

- **アセンブリの読み込み**   
   収集可能なアセンブリを読み込むためにサポートされる機構は、リフレクション出力のみです。 それ以外の形式のアセンブリ読み込みによってロードされたアセンブリは、アンロードできません。
 
- **コンテキスト バインド オブジェクト**    
   コンテキストの静的変数はサポートされません。 収集可能なアセンブリ内の型は、<xref:System.ContextBoundObject> を拡張できません。 ただし、別の場所で定義されているコンテキスト バインド オブジェクトを、収集可能なアセンブリのコードで使用することはできます。

- **スレッドの静的データ**       
   スレッドの静的変数はサポートされません。

## <a name="see-also"></a>参照

- [動的メソッドおよびアセンブリの出力](emitting-dynamic-methods-and-assemblies.md)
