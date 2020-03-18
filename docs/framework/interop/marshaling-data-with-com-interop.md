---
title: COM 相互運用機能によるデータのマーシャリング
ms.date: 09/07/2017
helpviewer_keywords:
- COM interop, data marshaling
- marshaling data, COM interop
ms.openlocfilehash: 24fa390c94baaa0fe009ebe513f2eb7aa34d34fa
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73113995"
---
# <a name="marshaling-data-with-com-interop"></a>COM 相互運用機能によるデータのマーシャリング
COM 相互運用は、マネージド コードから COM オブジェクトを使用すること、およびマネージド オブジェクトを COM に公開することの、両方の操作をサポートします。 COM との間でデータをマーシャリングする操作のサポートは広範で、ほとんどの場合、正しいマーシャリング動作が提供されます。  
  
 Windows SDK には、以下の COM 相互運用ツールが含まれています。  
  
- [タイプ ライブラリ インポーター (Tlbimp.exe)](../tools/tlbimp-exe-type-library-importer.md)、これは COM タイプ ライブラリを相互運用アセンブリに変換します。 このアセンブリから、相互運用マーシャリング サービスは、マネージド メモリとアンマネージド メモリ間のデータ マーシャリングを実行するラッパーを生成します。  
  
- [タイプ ライブラリ エクスポーター (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md)、これは COM タイプ ライブラリをアセンブリから生成して、メソッド呼び出しの際にマーシャリングを実行するラッパーを生成します。  
  
 以下のセクションは、マーシャラーに追加の型情報を提供できる (またはその必要がある) ときに、相互運用ラッパーをカスタマイズするためのプロセスについて説明するトピックにリンクしています。  
  
## <a name="in-this-section"></a>このセクションの内容  
[方法: ラッパを手動で作成する](how-to-create-wrappers-manually.md)   
マネージド ソース コードにおいて COM ラッパーを手動で作成する方法について説明します。 
 
 [方法: マネージ コード DCOM を WCF に移行する](how-to-migrate-managed-code-dcom-to-wcf.md)  
 最も安全なソリューションのために、マネージド DCOM コードを WCF に移行する方法について説明します。  
  
## <a name="related-sections"></a>関連項目  
 [COM のデータ型](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sak564ww(v=vs.100))  
 対応するマネージドとアンマネージドのデータ型を提供します。  
  
 [COM 呼び出し可能ラッパーのカスタマイズ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3bwc828w(v=vs.100))  
 デザイン時に <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性を使用して、データ型を明示的にマーシャリングする方法について説明します。  
  
 [ランタイム呼び出し可能ラッパーのカスタマイズ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/e753eftz(v=vs.100))  
 相互運用アセンブリで型のマーシャリング動作を調整する方法、および COM 型を手動で定義する方法について説明します。  
  
 [高度な COM 相互運用性](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bd9cdfyx(v=vs.100))  
 .NET Framework アプリケーションに COM コンポーネントを組み込む方法についての詳細情報へのリンクを示します。  
  
 [アセンブリからタイプ ライブラリへの変換の要約](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))  
 アセンブリからタイプ ライブラリにエクスポート変換するプロセスについて説明します。  
  
 [タイプ ライブラリからアセンブリへの変換の要約](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/k83zzh38(v=vs.100))  
 タイプ ライブラリからアセンブリにインポート変換するプロセスについて説明します。  
  
 [ジェネリック型を使用する相互運用](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229590(v=vs.100))  
 COM 相互運用性のジェネリック型を使用するとき、どのアクションがサポートされるかについて説明します。
