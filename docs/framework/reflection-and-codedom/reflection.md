---
title: .NET のリフレクション
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET], reflection
- EventInfo class, reflection
- common language runtime, reflection
- FieldInfo class, reflection
- ParameterInfo class, reflection
- ConstructorInfo class, reflection
- Assembly class, reflection
- value types, reflection
- reflection, about reflection
- modules, reflection
- runtime, reflection
- Module class, reflection
- MethodInfo class, reflection
- PropertyInfo class, reflection
- type browsers
- reflection
- discovering type information at run time
- type system, reflection
ms.assetid: d1a58e7f-fb39-4d50-bf84-e3b8f9bf9775
ms.openlocfilehash: 42944d8267d2e99fd9eb1a2cb28c0c81d3e9af75
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744568"
---
# <a name="reflection-in-net"></a>.NET のリフレクション

<xref:System.Reflection> 名前空間のクラスを <xref:System.Type?displayProperty=nameWithType>と共に使用すると、読み込まれた[アセンブリ](../../standard/assembly/index.md)およびその中に定義されている型 ([クラス](../../standard/base-types/common-type-system.md#classes)、[インターフェイス](../../standard/base-types/common-type-system.md#interfaces)、値型 (つまり、[構造体](../../standard/base-types/common-type-system.md#structures)と[列挙](../../standard/base-types/common-type-system.md#enumerations)体) など) に関する情報を取得できます。 また、リフレクションを使用すると、実行時に型インスタンスを作成したり、作成した型インスタンスを呼び出したり、アクセスしたりできます。 リフレクションのさまざまな面に関するトピックについては、この概要の最後の「[関連トピック](#related_topics)」を参照してください。
  
[共通言語ランタイム](../../standard/clr.md) ローダーは、[アプリケーション ドメイン](../app-domains/application-domains.md)を管理します。アプリケーション ドメインは、同じアプリケーション スコープのオブジェクトの周りに定義された境界を構成します。 アプリケーション ドメインの管理には、該当アプリケーション ドメインへの各アセンブリの読み込み、および各アセンブリ内の型階層のメモリ レイアウトの制御などが含まれます。  
  
[アセンブリ](../app-domains/index.md)には、モジュールが含まれています。モジュールには、型が含まれています。また、型にはメンバーが含まれています。 リフレクションは、アセンブリ、モジュール、および型をカプセル化するオブジェクトを提供します。 リフレクションを使用すると、動的に型のインスタンスを作成したり、その型を既存のオブジェクトにバインドしたり、既存のオブジェクトから型を取得できます。 その後で、型のメソッドを呼び出したり、型のフィールドやプロパティにアクセスしたりできます。 リフレクションの代表的な用途は、次のとおりです。  
  
- <xref:System.Reflection.Assembly> を使用して、アセンブリを定義または読み込み、アセンブリ マニフェストにリストされているモジュールを読み込み、そのアセンブリから型を検索し、そのインスタンスを作成します。  
  
- モジュールを含んでいるアセンブリや、モジュール内のクラスなどの情報を探索するには、<xref:System.Reflection.Module> を使用します。 また、すべてのグローバル メソッドも取得できます。その他、モジュールに定義されている固有の非グローバルなメソッドも取得できます。  
  
- 名前、パラメーター、アクセス修飾子 (<xref:System.Reflection.ConstructorInfo> または `public` など)、およびコンストラクターの実装詳細 (`private` または `abstract` など) の情報を探索するには、`virtual` を使用します。 特定のコンストラクターを呼び出すには、<xref:System.Type.GetConstructors%2A> の <xref:System.Type.GetConstructor%2A> メソッドまたは <xref:System.Type> メソッドを使用します。  
  
- 名前、戻り値の型、パラメーター、アクセス修飾子 (<xref:System.Reflection.MethodInfo> または `public` など)、およびメソッドの実装詳細 (`private` または `abstract` など) の情報を探索するには、`virtual` を使用します。 特定のメソッドを呼び出すには、<xref:System.Type.GetMethods%2A> の <xref:System.Type.GetMethod%2A> メソッドまたは <xref:System.Type> メソッドを使用します。  
  
- フィールドの名前、アクセス修飾子 (<xref:System.Reflection.FieldInfo> または `public` など)、フィールドの実装詳細 (`private` など) などの情報を探索したり、フィールドの値を取得または設定したりするには、`static` を使用します。  
  
- 名前、イベント ハンドラーのデータ型、カスタム属性、宣言型、イベントのリフレクションされた型などの情報を探索したり、イベント ハンドラーを追加および削除したりするには、<xref:System.Reflection.EventInfo> を使用します。  
  
- プロパティの名前、データ型、宣言型、リフレクションされた型、読み取り専用または書き込み可能のステータスなどの情報を探索したり、プロパティの値を取得または設定したりするには、<xref:System.Reflection.PropertyInfo> を使用します。  
  
- パラメーターの名前、データ型、パラメーターが入力パラメーターか出力パラメーターかなどの情報を探索したり、メソッド シグネチャ内のパラメーターの位置を探索したりするには、<xref:System.Reflection.ParameterInfo> を使用します。  
  
- アプリケーション ドメインのリフレクション専用のコンテキストで作業する際に、カスタム属性に関する情報を検索するには、<xref:System.Reflection.CustomAttributeData> を使用します。 <xref:System.Reflection.CustomAttributeData> を使用すると、それらのインスタンスを作成せずに属性を調べることができます。  
  
<xref:System.Reflection.Emit> 名前空間のクラスは、実行時に型を作成できる特殊な形態のリフレクションを提供します。  
  
また、リフレクションを使用すると、ユーザーが型を選択し、型についての情報を表示できる、型ブラウザーと呼ばれるアプリケーションを作成することもできます。  
  
リフレクションには、他の用途もあります。 JScript のような言語のコンパイラは、リフレクションを使用してシンボル テーブルを構築します。 <xref:System.Runtime.Serialization> 名前空間のクラスは、リフレクションを使用して、データにアクセスし、永続化するフィールドを決定します。 <xref:System.Runtime.Remoting> 名前空間のクラスは、シリアル化を通じて間接的にリフレクションを使用します。  
  
## <a name="runtime-types-in-reflection"></a>リフレクションのランタイム型  
リフレクションは <xref:System.Type> や <xref:System.Reflection.MethodInfo> などのクラスを提供して、型、メンバー、パラメーター、およびその他のコード エンティティを示します。 ただし、リフレクションを使用する場合は、これらのクラスを直接利用しません。これらの大部分は抽象 (Visual Basic では `MustInherit`) です。 代わりに、共通言語ランタイム (CLR) によって提供される型を利用します。  
  
たとえば、C# `typeof` 演算子 (Visual Basic では `GetType`) を使用して <xref:System.Type> オブジェクトを取得する場合、オブジェクトは実際には `RuntimeType` です。 `RuntimeType` は <xref:System.Type> から派生し、すべての抽象メソッドの実装を提供します。  
  
これらのランタイム クラスは `internal` (Visual Basic では `Friend`) です。 それらは基本クラスと別のドキュメントでは説明していません。それらの動作は基本クラスのドキュメントで説明されています。  
  
<a name="related_topics"></a>   

## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[型情報の表示](viewing-type-information.md)|<xref:System.Type> クラスについて説明します。<xref:System.Type> をさまざまなリフレクション クラスと共に使用して、コンストラクター、メソッド、フィールド、プロパティ、およびイベントについての情報を取得する方法を示すコード例を提供します。|  
|[リフレクションとジェネリック型](reflection-and-generic-types.md)|ジェネリック型およびジェネリック メソッドの型パラメーターと型引数がリフレクションでどのように処理されるかについて説明します。|  
|[リフレクションに関するセキュリティ上の考慮事項](security-considerations-for-reflection.md)|型情報を探索し、型にアクセスするためにどの程度までリフレクションを使用できるかを決定する規則を説明します。|  
|[型の動的な読み込みおよび使用](dynamically-loading-and-using-types.md)|遅延バインディングをサポートするリフレクション カスタム バインド インターフェイスについて説明します。|  
|[方法: リフレクションのみのコンテキストにアセンブリを読み込む](how-to-load-assemblies-into-the-reflection-only-context.md)|リフレクションのみの読み込みのコンテキストについて説明します。 アセンブリを読み込む方法、そのコンテキストをテストする方法、およびリフレクションのみのコンテキストでアセンブリに適用されている属性を調べる方法を紹介します。|  
|[カスタム属性へのアクセス](accessing-custom-attributes.md)|リフレクションを使用して、属性の有無および属性値を照会する方法を説明します。|  
|[完全修飾型名の指定](specifying-fully-qualified-type-names.md)|Backus-Naur form (BNF) に基づく完全修飾型名の書式と、特殊文字、アセンブリ名、ポインター、参照、および配列を指定する際に必要な構文を説明します。|  
|[方法 : リフレクションを使用してデリゲートをフックする](how-to-hook-up-a-delegate-using-reflection.md)|メソッドのデリゲートを作成し、デリゲートをイベントにフックする方法について説明します。 また、<xref:System.Reflection.Emit.DynamicMethod> を使用して実行時にイベント処理メソッドを作成する方法について説明します。|  
|[動的メソッドおよびアセンブリの出力](emitting-dynamic-methods-and-assemblies.md)|動的アセンブリや動的メソッドを生成する方法について説明します。|  
  
## <a name="reference"></a>リファレンス  

<xref:System.Type?displayProperty=nameWithType>  
  
<xref:System.Reflection>  
  
<xref:System.Reflection.Emit>  
