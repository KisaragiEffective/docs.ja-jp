---
title: COM 相互運用の概要
ms.date: 07/20/2015
helpviewer_keywords:
- interop assemblies
- COM interop [Visual Basic], about COM interop
ms.assetid: 8bd62e68-383d-407f-998b-29aa0ce0fd67
ms.openlocfilehash: c7909b3b6a2c9f0b397b9621b7e5125c232be313
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353201"
---
# <a name="introduction-to-com-interop-visual-basic"></a>COM 相互運用の概要 (Visual Basic)
コンポーネントオブジェクトモデル (COM) を使用すると、オブジェクトはその機能を他のコンポーネントに公開したり、アプリケーションをホストしたりできます。 COM オブジェクトは長年にわたって Windows プログラミングの基本となっていますが、共通言語ランタイム (CLR) 用に設計されたアプリケーションには多くの利点があります。  
  
 .NET Framework アプリケーションは、最終的には COM で開発したものと置き換えます。 それまでは、Visual Studio を使用して COM オブジェクトを使用するか、作成する必要があります。 COM または*com 相互運用機能*との相互運用性により、既存の com オブジェクトを使用して、自分のペースで .NET Framework に移行することができます。  
  
 .NET Framework を使用して COM コンポーネントを作成することにより、登録を必要としない COM 相互運用機能を使用できます。 これにより、1 台のコンピューターに複数のバージョンがインストールされている場合に有効にする DLL バージョンを制御できます。エンドユーザーは、XCOPY または FTP を使用して、アプリケーションを実行可能なコンピューター上の適切なディレクトリにコピーできます。 詳細については、「[登録を使用しないCOM 相互運用機能](../../../framework/interop/registration-free-com-interop.md)」を参照してください。
 

## <a name="managed-code-and-data"></a>マネージコードとデータ  
 .NET Framework 用に開発されたコードは*マネージコード*と呼ばれ、CLR によって使用されるメタデータが含まれます。 .NET Framework アプリケーションで使用されるデータは*マネージデータ*と呼ばれます。これは、ランタイムがメモリの割り当てや解放、型チェックの実行などのデータ関連タスクを管理するためです。 既定では Visual Basic .NET はマネージコードとデータを使用しますが、相互運用機能アセンブリを使用して COM オブジェクトのアンマネージコードとデータにアクセスすることができます (このページで後ほど説明します)。  
  
## <a name="assemblies"></a>アセンブリ  
 アセンブリは、.NET Framework アプリケーションの主な構成要素です。 これは、1つ以上のファイルを含む1つの実装単位としてビルド、バージョン管理、および展開される機能のコレクションです。 各アセンブリには、アセンブリマニフェストが含まれています。  
  
## <a name="type-libraries-and-assembly-manifests"></a>タイプライブラリとアセンブリマニフェスト  
 タイプライブラリは、メンバー名やデータ型など、COM オブジェクトの特性を記述します。 アセンブリマニフェストは、.NET Framework アプリケーションに対して同じ機能を実行します。 これらには、次の情報が含まれます。  
  
- アセンブリ id、バージョン、カルチャ、およびデジタル署名。  
  
- アセンブリの実装を構成するファイル。  
  
- アセンブリを構成する型とリソース。 これには、エクスポートされたものも含まれます。  
  
- 他のアセンブリに対するコンパイル時の依存関係。  
  
- アセンブリを正常に実行するために必要なアクセス許可。  
  
 アセンブリとアセンブリマニフェストの詳細については、「[.NET のアセンブリ](../../../standard/assembly/index.md)」を参照してください。  
  
### <a name="importing-and-exporting-type-libraries"></a>タイプライブラリのインポートとエクスポート  
 Visual Studio には、Tlbimp というユーティリティが含まれています。これを使用すると、タイプライブラリから .NET Framework アプリケーションに情報をインポートできます。 アセンブリからタイプライブラリを生成するには、Tlbexp.exe ユーティリティを使用します。  
  
 Tlbimp と Tlbexp.exe の詳細については、「[tlbimp.exe (タイプライブラリインポーター)](../../../framework/tools/tlbimp-exe-type-library-importer.md) 」および「[Tlbexp.exe (タイプライブラリエクスポーター)](../../../framework/tools/tlbexp-exe-type-library-exporter.md)」を参照してください。  
  
## <a name="interop-assemblies"></a>相互運用機能アセンブリ  
 相互運用機能アセンブリは、マネージコードとアンマネージコードの間を橋渡しし、COM オブジェクトメンバーを同等のマネージメンバー .NET Framework にマップする .NET Framework アセンブリです。 Visual Basic .NET によって作成された相互運用アセンブリは、相互運用性のマーシャリングなど、COM オブジェクトの操作に関する多くの詳細を処理します。  
  
## <a name="interoperability-marshaling"></a>相互運用性のマーシャリング  
 すべての .NET Framework アプリケーションは、使用されるプログラミング言語に関係なく、オブジェクトの相互運用性を実現する一連の共通型を共有します。 COM オブジェクトのパラメーターと戻り値は、マネージコードで使用されるものとは異なるデータ型を使用する場合があります。 *相互運用性のマーシャリング*は、COM オブジェクトとの間で移動するときに、パラメーターと戻り値を同等のデータ型にパッケージ化するプロセスです。 詳細については、「[相互運用マーシャリング](../../../framework/interop/interop-marshaling.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md)
- [チュートリアル : COM オブジェクトによる継承の実装](../../../visual-basic/programming-guide/com-interop/walkthrough-implementing-inheritance-with-com-objects.md)
- [アンマネージ コードとの相互運用](../../../framework/interop/index.md)
- [相互運用性のトラブルシューティング](../../../visual-basic/programming-guide/com-interop/troubleshooting-interoperability.md)
- [.NET のアセンブリ](../../../standard/assembly/index.md)
- [Tlbimp.exe (タイプ ライブラリ インポーター)](../../../framework/tools/tlbimp-exe-type-library-importer.md)
- [Tlbexp.exe (タイプ ライブラリ エクスポーター)](../../../framework/tools/tlbexp-exe-type-library-exporter.md)
- [相互運用マーシャリング](../../../framework/interop/interop-marshaling.md)
- [登録を必要としない COM 相互運用機能](../../../framework/interop/registration-free-com-interop.md)
