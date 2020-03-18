---
title: 動的メソッドおよびアセンブリの出力
ms.date: 08/30/2017
helpviewer_keywords:
- reflection emit
- dynamic assemblies
- metadata, emit interfaces
- reflection emit, overview
- assemblies [.NET Framework], emitting dynamic assemblies
ms.openlocfilehash: 578851bed188921324e3c25e533b3466068dee3d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446786"
---
# <a name="emitting-dynamic-methods-and-assemblies"></a>動的メソッドおよびアセンブリの出力

ここでは、<xref:System.Reflection.Emit> 名前空間のマネージド型のセットについて説明します。これらのマネージド型を使用すると、コンパイラやツールでメタデータおよび Microsoft Intermediate Language (MSIL) を実行時に出力できます。また、ポータブル実行可能 (PE) ファイルをディスク上に生成することもできます。 この名前空間を使用する主な機能は、スクリプト エンジンとコンパイラです。 ここでは、リフレクション出力と呼ばれる <xref:System.Reflection.Emit> 名前空間の機能について説明します。  
  
リフレクション出力は、以下の機能を提供します。  
  
- 実行時に <xref:System.Reflection.Emit.DynamicMethod> クラスを使用して軽量のグローバル メソッドを定義し、デリゲートを使用してそのメソッドを実行します。  
  
- 実行時にアセンブリを定義し、次に、それらを実行するか、ディスクに保存します。  
  
- 実行時にアセンブリを定義し、それらを実行した後、それらをアンロードし、ガベージ コレクションがリソースを再利用できるようにします。  
  
- 実行時に新しいアセンブリのモジュールを定義し、次に、モジュールを実行するか、ディスクに保存します。  
  
- 実行時にモジュール内の型を定義し、その型のインスタンスを作成してメソッドを呼び出します。  
  
- デバッガーまたはコード プロファイラなどのツールで使用できる、定義されたモジュールのシンボリック情報を定義します。  
  
<xref:System.Reflection.Emit> 名前空間のマネージド型に加えて、アンマネージド メタデータ インターフェイスもあります。これについては、[メタデータ インターフェイス](../unmanaged-api/metadata/metadata-interfaces.md)に関するリファレンス ドキュメントを参照してください。 マネージド リフレクション出力は、アンマネージド メタデータ インターフェイスよりも強力なセマンティック エラー チェック機能、より高水準なメタデータの抽象化クラスを提供します。  
  
メタデータと MSIL を使用する際に役立つリソースとしては、他に、共通言語基盤 (CLI: Common Language Infrastructure) のドキュメント、特に「Partition II: Metadata Definition and Semantics」と「Partition III: CIL Instruction Set」などもあります。 ドキュメントは、 [Ecma Web サイト](https://www.ecma-international.org/publications/standards/Ecma-335.htm)からオンラインで入手できます。  
  
## <a name="in-this-section"></a>このセクションの内容
  
[リフレクション出力のセキュリティ関連事項](security-issues-in-reflection-emit.md)  
リフレクション出力を使用した動的アセンブリの作成時のセキュリティ関連事項について説明します。  

[方法: 動的メソッドを定義および実行する](how-to-define-and-execute-dynamic-methods.md)   
単純な動的メソッドおよびクラスのインスタンスにバインドされる動的メソッドを実行する方法を示します。

[方法: リフレクション出力を使用してジェネリック型を定義する](how-to-define-a-generic-type-with-reflection-emit.md)   
2 つの型パラメーターを持つ単純なジェネリック型を作成する方法、クラス制約、インターフェイス制約、特殊な制約を型パラメーターに適用する方法、パラメーターの型や戻り値の型としてクラスの型パラメーターを使用するメンバーを作成する方法を紹介します。

[方法: リフレクション出力を使用してジェネリック メソッドを定義する](how-to-define-a-generic-method-with-reflection-emit.md)   
単純なジェネリック メソッドを作成、出力および呼び出す方法を示します。

[動的な型生成のための収集可能なアセンブリ](collectible-assemblies.md)   
そのアセンブリが作成されたアプリケーション ドメインをアンロードせずにアンロードできる動的アセンブリである、収集可能なアセンブリについて説明します。
  
## <a name="reference"></a>参照  

<xref:System.Reflection.Emit.OpCodes>  
メソッド本体の構築に使用できる MSIL 命令コードのカタログを作成します。  
  
<xref:System.Reflection.Emit>  
動的メソッド、アセンブリ、および型の出力に使用するマネージド クラスが含まれます。  
  
<xref:System.Type>  
<xref:System.Type> クラスについて説明します。このクラスは、マネージド リフレクションとリフレクション出力の型を表し、これらのテクノロジを使用するうえで重要なクラスです。  
  
<xref:System.Reflection>  
メタデータとマネージド コードの探索に使用するマネージド クラスが含まれます。  
  
## <a name="related-sections"></a>関連項目  

[リフレクション](reflection.md)  
メタデータとマネージド コードの探索方法について説明します。  
  
[.NET のアセンブリ](../../standard/assembly/index.md)  
.NET 実装のアセンブリの概要を説明します。
