---
title: .NET Framework への COM コンポーネントの公開
ms.date: 03/30/2017
helpviewer_keywords:
- exposing COM components to .NET Framework
- interoperation with unmanaged code, exposing COM components
- COM interop, exposing COM components
ms.assetid: e78b14f1-e487-43cd-9c6d-1a07483f1730
ms.openlocfilehash: 914f72b5e4840555541943620ca2df1f629b0604
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123525"
---
# <a name="exposing-com-components-to-the-net-framework"></a>.NET Framework への COM コンポーネントの公開
このセクションでは、既存の COM コンポーネントをマネージド コードに公開するために必要なプロセスをまとめています。 .NET Framework と緊密に統合される COM サーバーの詳細については、「[Design Considerations for Interoperation](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/61aax4kh(v=vs.100))」(相互運用のためのデザインの考慮事項) を参照してください。
  
 既存の COM コンポーネントは、中間層のビジネス アプリケーション、または独立した機能として、マネージド コードでの貴重なリソースです。 理想的なコンポーネントは、プライマリ相互運用機能アセンブリがあり、COM で課せられるプログラミング標準に厳密に準拠しています。  
  
#### <a name="to-expose-com-components-to-the-net-framework"></a>.NET Framework に COM コンポーネントを公開するには  
  
1. [タイプ ライブラリをアセンブリとしてインポートする](importing-a-type-library-as-an-assembly.md)。  
  
     共通言語ランタイムでは、COM 型を含め、すべてのタイプにメタデータが必要です。 メタデータとしてインポートされた COM 型を含むアセンブリを取得するには、複数の方法があります。  
  
2. [マネージド コード内で COM 型を使用する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y76b69k(v=vs.100))。  
  
     任意のマネージド型で行うのと同じ方法で、COM オブジェクトで COM 型の検査、インスタンスのアクティブ化、およびメソッドの呼び出しができます。  
  
3. [相互運用プロジェクトをコンパイルする](compiling-an-interop-project.md)。  
  
     Windows SDK には、共通言語仕様 (CLS) に準拠するいくつかの言語 (Visual Basic、C#、C++ など) のコンパイラが用意されています。  
  
4. [相互運用アプリケーションを展開する](deploying-an-interop-application.md)。  
  
     相互運用アプリケーションは、[厳密な名前を付けた](../../standard/assembly/strong-named.md)、署名されたアセンブリとして、グローバル アセンブリ キャッシュに最適に展開されます。  
  
## <a name="see-also"></a>関連項目

- [アンマネージ コードとの相互運用](index.md)
- [相互運用のためのデザインの考慮事項](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/61aax4kh(v=vs.100))
- [COM 相互運用機能のサンプル: .NET クライアントおよび COM サーバー](com-interop-sample-net-client-and-com-server.md)
- [言語への非依存性、および言語非依存コンポーネント](../../standard/language-independence-and-language-independent-components.md)
- [Gacutil.exe (グローバル アセンブリ キャッシュ ツール)](../tools/gacutil-exe-gac-tool.md)
