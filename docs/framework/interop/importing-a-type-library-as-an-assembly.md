---
title: タイプ ライブラリのアセンブリとしてのインポート
ms.date: 03/30/2017
helpviewer_keywords:
- importing type library
- type metadata
- custom wrappers
- metadata
- exposing COM components to .NET Framework
- Type Library Importer
- interoperation with unmanaged code, importing type library
- interoperation with unmanaged code, exposing COM components
- type libraries
- TypeLibConverter class, importing type library as assembly
- COM interop, importing type library
- COM interop, exposing COM components
ms.assetid: d1898229-cd40-426e-a275-f3eb65fbc79f
ms.openlocfilehash: e1a21175bcabc72b86a328d4f73ecec37140c304
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107597"
---
# <a name="importing-a-type-library-as-an-assembly"></a>タイプ ライブラリのアセンブリとしてのインポート

通常、COM 型の定義は、タイプ ライブラリに存在します。 これに対し、CLS 準拠のコンパイラはアセンブリ内に型のメタデータを生成します。 型情報の 2 つのソースは大きく異なります。 このトピックでは、タイプ ライブラリからメタデータを生成する方法について説明します。 結果のアセンブリは相互運用機能アセンブリと呼ばれ、含まれる型情報により、.NET Framework アプリケーションで COM 型を使用できます。

この型情報をアプリケーションに使用できるようにする 2 つの方法があります。

- デザイン時専用の相互運用機能アセンブリの使用: .NET Framework 4 以降では、相互運用機能アセンブリから実行可能ファイルに型情報を埋め込むようにコンパイラに指示できます。 コンパイラは、アプリケーションで使用する型情報だけを埋め込みます。 アプリケーションを使用して相互運用機能アセンブリを展開する必要はありません。 この手法を使用することをお勧めします。

- 相互運用機能アセンブリの展開: 相互運用機能アセンブリへの標準の参照を作成できます。 この場合、アプリケーションで相互運用機能アセンブリを配置する必要があります。 この手法を採用し、プライベートの COM コンポーネントを使用しない場合は、常に、マネージド コードに組み込む予定の COM コンポーネントの作成者によって発行されたプライマリ相互運用機能アセンブリ (PIA) を参照します。 プライマリ相互運用機能アセンブリの生成と使用の詳細については、「[プライマリ相互運用機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aax7sdch(v=vs.100))」を参照してください。

設計時専用の相互運用機能アセンブリを使用する場合は、COM コンポーネントの作成者によってパブリッシュされたプライマリ相互運用機能アセンブリから型情報を埋め込むことができます。 ただし、アプリケーションを使用してプライマリ相互運用機能アセンブリを展開する必要はありません。

設計時専用の相互運用機能アセンブリを使用すると、ほとんどのアプリケーションは、COM コンポーネントのすべての機能を使用しないため、アプリケーションのサイズが小さくなります。 型情報を埋め込む場合に、コンパイラは非常に効率的です。アプリケーションが、COM インターフェイスでメソッドの一部のみを使用する場合、コンパイラは、使用されないメソッドを埋め込みません。 型情報が埋め込まれているアプリケーションが、そのような別のアプリケーションと対話するか、プライマリ相互運用機能アセンブリを使用するアプリケーションと対話する場合、共通言語ランタイムは、型等価性の規則を使用して、同じ名前の 2 つの型が同じ COM 型を表すかどうかを判断します。 COM オブジェクトを使用するためにこれらの規則を理解する必要はありません。 ただし、規則に興味がある場合は、「[Type Equivalence and Embedded Interop Types](type-equivalence-and-embedded-interop-types.md)」(型の等価性と埋め込まれた相互運用機能型) を参照してください。

## <a name="generating-metadata"></a>メタデータ ファイルの生成

COM タイプ ライブラリには、Loanlib.tlb などの .tlb 拡張子を持つスタンドアロンのファイルを指定できます。 一部のタイプ ライブラリは、.dll または .exe ファイルのリソース セクションに埋め込まれます。 タイプ ライブラリ情報の他のソースは、.olb ファイルおよび .ocx ファイルです。

対象とする COM 型の実装を格納するタイプ ライブラリを特定した後は、型のメタデータを含む相互運用機能アセンブリを生成するための次のオプションがあります。

- Visual Studio

  Visual Studio は、タイプ ライブラリ内の COM 型をアセンブリ内のメタデータに自動的に変換します。 手順については、「[方法: タイプライブラリへの参照を追加する](how-to-add-references-to-type-libraries.md)」を参照してください。

- [タイプ ライブラリ インポーター (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md)

  タイプ ライブラリ インポーターは、結果の相互運用機能のファイルのメタデータを調整するコマンド ライン オプションを提供し、既存のタイプ ライブラリから型をインポートし、相互運用機能アセンブリと名前空間を生成します。 手順については、「[方法: 相互運用機能アセンブリをタイプ ライブラリから生成する](how-to-generate-interop-assemblies-from-type-libraries.md)」を参照してください。

- <xref:System.Runtime.InteropServices.TypeLibConverter?displayProperty=nameWithType> クラス

  このクラスは、コクラスとタイプ ライブラリ内のインターフェイスをアセンブリ内のメタデータに変換するメソッドを提供します。 これは Tlbimp.exe と同じメタデータ出力を生成します。 ただし、Tlbimp.exe とは異なり、<xref:System.Runtime.InteropServices.TypeLibConverter> クラスは、メモリ内のタイプ ライブラリをメタデータに変換できます。

- カスタム ラッパー

  タイプ ライブラリが使用できないか正しくない場合、1 つのオプションは、マネージド ソース コードでクラスまたはインターフェイスの重複する定義を作成することです。 その後で、アセンブリ内のメタデータを生成するためにランタイムを対象とするコンパイラでソース コードをコンパイルします。

  COM 型を手動で定義するには、次の項目へのアクセスが必要です。

  - 定義されているコクラスとインターフェイスの正確な説明。

  - C# コンパイラなど、適切な .NET Framework のクラス定義を生成できるコンパイラ。

  - タイプ ライブラリからアセンブリへの変換規則の知識。

  カスタム ラッパーの作成は、高度な手法です。 カスタム ラッパーを生成する方法の詳細については、「[標準ラッパーのカスタマイズ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/h7hx9abd(v=vs.100))」を参照してください。

 COM 相互運用機能のインポート処理の詳細については、「[Type Library to Assembly Conversion Summary](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))」(タイプ ライブラリからアセンブリへの変換の要約) を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.InteropServices.TypeLibConverter>
- [.NET Framework への COM コンポーネントの公開](exposing-com-components.md)
- [タイプ ライブラリからアセンブリへの変換の要約](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))
- [Tlbimp.exe (タイプ ライブラリ インポーター)](../tools/tlbimp-exe-type-library-importer.md)
- [標準ラッパーのカスタマイズ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/h7hx9abd(v=vs.100))
- [マネージド コードでの COM 型の使用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))
- [相互運用プロジェクトのコンパイル](compiling-an-interop-project.md)
- [相互運用アプリケーションの配置](deploying-an-interop-application.md)
- [方法: タイプ ライブラリへの参照を追加する](how-to-add-references-to-type-libraries.md)
- [方法: 相互運用機能アセンブリをタイプ ライブラリから生成する](how-to-generate-interop-assemblies-from-type-libraries.md)
