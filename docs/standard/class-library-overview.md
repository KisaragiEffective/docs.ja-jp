---
title: .NET クラス ライブラリの概要
ms.date: 02/08/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- classes [.NET Framework], library overview
- classes [.NET Core], library overview
- .NET, library overview
- class objects value type
- naming conventions [.NET Framework]
- types, .NET Framework
- user-defined types
- Visual Basic, data types
- data types [.NET Framework], C++
- Visual C#, data types
- libraries, .NET Framework class library
- data types [.NET Framework], F#
- System namespace
- F#, data types
- .NET Framework, class library
- type system [.NET Framework]
- data types [.NET Framework]
- value types
- data types [.NET Framework], Visual Basic
- Common Language Specification
- namespaces [.NET Framework]
- floating point value type
- class library [.NET Framework]
- CLS
- logical value type
- .NET Framework class library, about
- built-in types
- namespaces [.NET Framework], about namespaces
- Visual C++, data types
- members [.NET Framework], type
- data types [.NET Framework], C#
- integer value type
- base types, class library
ms.assetid: 7e4c5921-955d-4b06-8709-101873acf157
ms.openlocfilehash: 596c0fd8fec8f59d977f1db445f9000df23ad5ce
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132856"
---
# <a name="net-class-library-overview"></a>.NET クラス ライブラリの概要

.NET 実装には、開発プロセスを高速化および最適化し、システム機能へのアクセスを提供する、クラス、インターフェイス、デリゲート、および値の型が含まれます。 言語間での相互運用性を確保するために、.NET のほとんどの型は CLS (共通言語仕様) に準拠しています。そのため、コンパイラが CLS に準拠しているすべてのプログラミング言語でこれらの型を使用できます。  
  
 .NET の型は、.NET アプリケーション、.NET コンポーネント、およびコントロールを構築するときの基礎となります。 .NET 実装には、次の機能を実行する型が含まれます。  
  
- 基本データ型と例外を表す。  
  
- データ構造をカプセル化する。  
  
- 入出力を実行する。  
  
- 読み込まれた型についての情報にアクセスする。  
  
- .NET Framework セキュリティ チェックを呼び出す。  
  
- データ アクセス、豊富なクライアント側 GUI、およびサーバー制御式のクライアント側 GUI を提供する。  
  
 .NET には、豊富なインターフェイスのセットに加えて、抽象クラスと具象 (抽象ではない) クラスが用意されています。 具象クラスはそのまま使用できますが、多くの場合は、具象クラスから独自のクラスを派生させます。 インターフェイスの機能を使用するには、インターフェイスを実装するクラスを作成するか、またはインターフェイスを実装する .NET クラスの 1 つからクラスを派生させます。  
  
## <a name="naming-conventions"></a>名前付け規則

 .NET の型では、階層構造を伴うドット構文の名前付けスキームが使用されます。 この方法では、関連する型が名前空間にグループ化されるため、検索と参照を簡単に行うことができます。 フルネームのうち右端のドットまでの最初の部分は、名前空間の名前です。 名前の最後の部分は型名です。 たとえば、`System.Collections.Generic.List<T>` は `System.Collections.Generic` 名前空間に属する `List<T>` 型を表します。 <xref:System.Collections.Generic> の型は、ジェネリック コレクションの操作で使用できます。  
  
 この名前付け方法によって、.NET Framework を拡張するライブラリ開発者は、型の階層構造のグループを簡単に作成し、一貫性のあるわかりやすい名前を付けることができます。 また、型の完全名 (つまり名前空間と型名) によって型を明確に特定できるため、型名の競合を防ぐことができます。 ライブラリ開発者は、次の規則に従って名前空間の名前を付けてください。  
  
 *CompanyName*.*TechnologyName*  
  
 たとえば、名前空間 `Microsoft.Word` はこのガイドラインに従っています。  
  
 名前付けパターンを使用して関連する型を名前空間にグループ化する方法は、クラス ライブラリを構築および文書化するのに便利です。 ただし、この名前付け方法は、参照可能範囲、メンバー アクセス、継承、セキュリティ、バインディングには影響しません。 名前空間は複数のアセンブリにまたがって分割でき、また 1 つのアセンブリに複数の名前空間からの型を含めることができます。 アセンブリは、共通言語ランタイムにおけるバージョン管理、配置、セキュリティ、読み込み、および参照可能範囲のための構造を提供します。  
  
 名前空間と型の名前の詳細については、「[共通型システム](../../docs/standard/base-types/common-type-system.md)」(共通型システム) を参照してください。  
  
## <a name="system-namespace"></a>System 名前空間

 <xref:System> 名前空間は、.NET における基本的な型のルート名前空間です。 この名前空間には、<xref:System.Object> (継承階層構造のルート)、<xref:System.Byte>、<xref:System.Char>、<xref:System.Array>、<xref:System.Int32>、<xref:System.String> など、すべてのアプリケーションで使用される基本データ型を表すクラスが含まれます。 これらの型の多くは、プログラミング言語で使われるプリミティブなデータ型に対応します。 .NET Framework の型を使用してコードを記述するときには、.NET Framework の基本データ型が使用されるところに、プログラミング言語側の対応するキーワードを使用できます。  
  
 次の表では、.NET に用意されているそれぞれの基本型の一覧を示し、各型について簡単に説明し、Visual Basic、C#、C++、F# での対応する型を示します。  
  
|カテゴリ|クラス名|説明|Visual Basic のデータ型|C# のデータ型|C++/CLI のデータ型|F# のデータ型|  
|--------------|----------------|-----------------|----------------------------|-------------------|---------------------|-----------------------|  
|整数型|<xref:System.Byte>|8 ビット符号なし整数。|**Byte**|**byte**|**unsigned char**|**byte**|  
||<xref:System.SByte>|8 ビット符号付き整数。<br /><br /> 非 CLS 準拠|**SByte**|**sbyte**|**char**<br /> または<br /> **signed** **char**|**sbyte**|  
||<xref:System.Int16>|16 ビット符号付き整数。|**Short**|**short**|**short**|**int16**|  
||<xref:System.Int32>|32 ビット符号付き整数。|**Integer**|**int**|**int**<br /><br /> または<br /><br /> **long**|**int**|  
||<xref:System.Int64>|64 ビット符号付き整数。|**Long**|**long**|**__int64**|**int64**|  
||<xref:System.UInt16>|16 ビット符号なし整数。<br /><br /> 非 CLS 準拠|**UShort**|**ushort**|**unsigned short**|**uint16**|  
||<xref:System.UInt32>|32 ビット符号なし整数<br /><br /> 非 CLS 準拠|**UInteger**|**uint**|**unsigned int**<br /> または<br /> **unsigned long**|**uint32**|  
||<xref:System.UInt64>|64 ビット符号なし整数。<br /><br /> 非 CLS 準拠|**ULong**|**ulong**|**unsigned __int64**|**uint64**|  
|浮動小数点数|<xref:System.Single>|単精度 (32 ビット) 浮動小数点数|**Single**|**float**|**float**|**float32**<br> or<br>**single**|  
||<xref:System.Double>|倍精度 (64 ビット) 浮動小数点数|**Double**|**double**|**double**|**float**<br> or <br> **double**|  
|論理|<xref:System.Boolean>|ブール値 (true または false)|**Boolean**|**bool**|**bool**|**bool**|  
|その他|<xref:System.Char>|Unicode (16 ビット) 文字|**Char**|**char**|**wchar_t**|**char**|  
||<xref:System.Decimal>|十進数 (128 ビット) の値です。|**Decimal**|**decimal**|**Decimal**|**decimal**|  
||<xref:System.IntPtr>|基になるプラットフォームによってサイズが決まる符号付き整数 (32 ビットのプラットフォームでは 32 ビット値、64 ビットのプラットフォームでは 64 ビット値)|**IntPtr**<br /><br /> 非組み込み型|**IntPtr**<br /><br /> 非組み込み型|**IntPtr**<br /><br /> 非組み込み型|**unativeint**|  
||<xref:System.UIntPtr>|基になるプラットフォームによってサイズが決まる符号なし整数 (32 ビットのプラットフォームでは 32 ビット値、64 ビットのプラットフォームでは 64 ビット値)<br /><br /> 非 CLS 準拠|**UIntPtr**<br /><br /> 非組み込み型|**UIntPtr**<br /><br /> 非組み込み型|**UIntPtr**<br /><br /> 非組み込み型|**unativeint**|  
||<xref:System.Object>|オブジェクト階層構造のルート|**オブジェクト**|**object**|**Object^**|**obj**|  
||<xref:System.String>|Unicode 文字の不変固定長文字列|**String**|**string**|**String^**|**string**|  
  
 基本データ型に加えて、<xref:System> 名前空間には、例外を処理するクラスから、核となるランタイム概念 (アプリケーション ドメインやガベージ コレクターなど) を扱うクラスまで、100 以上のクラスが含まれます。 <xref:System> 名前空間には、2 次レベルの名前空間も数多く含まれています。  
  
 名前空間の詳細については、「[.NET API ブラウザー](https://docs.microsoft.com/dotnet/api)」を使用して .NET クラス ライブラリを参照してください。 API リファレンス ドキュメントでは、各名前空間、その種類、および各メンバーに関するドキュメントが提供されます。  
  
## <a name="see-also"></a>関連項目

- [共通型システム](../../docs/standard/base-types/common-type-system.md)
- [.NET API ブラウザー](../../api/index.md)
- [概要](../../docs/framework/get-started/overview.md)
