---
title: ジェネリックとリフレクション - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], reflection
- reflection [C#], generic types
ms.assetid: 162fd9b4-dd5b-4abb-8c9b-e44e21e2f451
ms.openlocfilehash: 4893bf5ebe73988bb6535cc2a85591ff0dde6ebd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712170"
---
# <a name="generics-and-reflection-c-programming-guide"></a>ジェネリックとリフレクション (C# プログラミング ガイド)
共通言語ランタイム (CLR) は実行時にジェネリック型の情報にアクセスできるため、非ジェネリック型の場合と同じように、リフレクションを使用してジェネリック型の情報を取得できます。 詳細については、「[ランタイムのジェネリック](./generics-in-the-run-time.md)」を参照してください。  
  
 .NET Framework 2.0 では、ジェネリック型の実行時の情報を有効にする新しいメンバーが <xref:System.Type> クラスにいくつか追加されています。 それらのメソッドとプロパティの使用方法の詳細については、そのクラスのドキュメントを参照してください。 <xref:System.Reflection.Emit> 名前空間にも、ジェネリックをサポートする新しいメンバーが追加されています。 「[方法: リフレクション出力を使用してジェネリック型を定義する](../../../framework/reflection-and-codedom/how-to-define-a-generic-type-with-reflection-emit.md)」を参照してください。  
  
 ジェネリック リフレクションで使用する用語に関する一定の条件の一覧については、<xref:System.Type.IsGenericType%2A> プロパティの解説を参照してください。  
  
|System.Type メンバー名|[説明]|  
|-----------------------------|-----------------|  
|<xref:System.Type.IsGenericType%2A>|型がジェネリックである場合に true を返します。|  
|<xref:System.Type.GetGenericArguments%2A>|構築された型に対して指定された型引数、またはジェネリック型定義の型パラメーターを表す `Type` オブジェクトの配列を返します。|  
|<xref:System.Type.GetGenericTypeDefinition%2A>|現在構築されている型の基になるジェネリック型定義を返します。|  
|<xref:System.Type.GetGenericParameterConstraints%2A>|現在のジェネリック型パラメーターの制約を表す `Type` オブジェクトの配列を返します。|  
|<xref:System.Type.ContainsGenericParameters%2A>|型 (またはそこに含まれているいずれかの型またはメソッド) に、具体的な型の指定されていない型パラメーターが含まれている場合に true を返します。|  
|<xref:System.Type.GenericParameterAttributes%2A>|現在のジェネリック型パラメーターの特殊な制約を説明する `GenericParameterAttributes` フラグの組み合わせを取得します。|  
|<xref:System.Type.GenericParameterPosition%2A>|型パラメーターを表す `Type` オブジェクトの場合、その型パラメーターが宣言されたジェネリック型定義またはジェネリック メソッド定義の型パラメーター リストにおけるその位置を取得します。|  
|<xref:System.Type.IsGenericParameter%2A>|現在の `Type` が、ジェネリック型定義またはジェネリック メソッド定義の型パラメーターを表すかどうかを示す値を取得します。|  
|<xref:System.Type.IsGenericTypeDefinition%2A>|現在の <xref:System.Type> が、他のジェネリック型を構築できるジェネリック型の定義を表しているかどうかを示す値を取得します。 その型がジェネリック型の定義を表している場合に true を返します。|  
|<xref:System.Type.DeclaringMethod%2A>|現在のジェネリック型パラメーターを定義したジェネリック メソッドを返します。その型パラメーターがジェネリック メソッドによって定義されたものではない場合は null を返します。|  
|<xref:System.Type.MakeGenericType%2A>|型の配列の要素を現在のジェネリック型定義の型パラメーターで置き換え、結果の構築型を表す <xref:System.Type> オブジェクトを返します。|  
  
 さらに、<xref:System.Reflection.MethodInfo> クラスのメンバーは、ジェネリック メソッドの実行時の情報を有効にします。 ジェネリック メソッドのリフレクションで使用する用語に関する一定の条件の一覧については、<xref:System.Reflection.MethodBase.IsGenericMethod%2A> プロパティの解説を参照してください。  
  
|System.Reflection.MemberInfo メンバー名|[説明]|  
|----------------------------------------------|-----------------|  
|<xref:System.Reflection.MethodBase.IsGenericMethod%2A>|メソッドがジェネリックである場合に true を返します。|  
|<xref:System.Reflection.MethodInfo.GetGenericArguments%2A>|構築されたジェネリック メソッドの型引数、またはジェネリック メソッド定義の型パラメーターを表す Type オブジェクトの配列を返します。|  
|<xref:System.Reflection.MethodInfo.GetGenericMethodDefinition%2A>|現在構築されているメソッドの基になるジェネリック メソッド定義を返します。|  
|<xref:System.Reflection.MethodBase.ContainsGenericParameters%2A>|メソッド (またはそこに含まれているいずれかの型) に、具体的な型の指定されていない型パラメーターが含まれている場合に true を返します。|  
|<xref:System.Reflection.MethodBase.IsGenericMethodDefinition%2A>|現在の <xref:System.Reflection.MethodInfo> がジェネリック メソッドの定義を表している場合に true を返します。|  
|<xref:System.Reflection.MethodInfo.MakeGenericMethod%2A>|現在のジェネリック メソッド定義の型パラメーターを型の配列要素に置き換え、その結果構築されるメソッドを表す <xref:System.Reflection.MethodInfo> オブジェクトを返します。|  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [ジェネリック](./index.md)
- [リフレクションとジェネリック型](../../../framework/reflection-and-codedom/reflection-and-generic-types.md)
- [ジェネリック](../../../standard/generics/index.md)
