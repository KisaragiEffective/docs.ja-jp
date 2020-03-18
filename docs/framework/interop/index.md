---
title: アンマネージ コードとの相互運用
ms.date: 01/17/2018
helpviewer_keywords:
- unmanaged code, interoperation
- managed code, interoperation with unmanaged code
- .NET Framework, interoperation with unmanaged code
- unmanaged code
- interoperation with unmanaged code
- interoperation with unmanaged code, about interoperation
- components [.NET Framework], interoperation with unmanaged code
ms.assetid: ccb68ce7-b0e9-4ffb-839d-03b1cd2c1258
ms.openlocfilehash: 12183f390a5178f038c6dd2122a72a33e31ae0ee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73457968"
---
# <a name="interoperating-with-unmanaged-code"></a>アンマネージ コードとの相互運用

.NET Framework は、COM コンポーネント、COM+ サービス、外部のタイプ ライブラリ、各種のオペレーティング システム サービスとの相互運用を促進します。 データ型、メソッド署名、およびエラー処理機構は、マネージド オブジェクト モデルとアンマネージド オブジェクト モデルでは異なります。 .NET Framework コンポーネントとアンマネージ コードの間の相互運用を簡素化し、移行パスを簡単にするために、共通言語ランタイムがクライアントとサーバーの両方からこれらのオブジェクト モデルの違いを隠します。

ランタイムの制御下で実行されるコードは、マネージド コードと呼ばれます。 逆に、ランタイムの外部で実行されるコードはアンマネージ コードと呼ばれます。 アンマネージ コードの例としては、COM コンポーネント、ActiveX インターフェイス、Windows API 関数があります。

## <a name="in-this-section"></a>このセクションの内容

[.NET Framework への COM コンポーネントの公開](exposing-com-components.md)  
.NET Framework アプリケーションから COM コンポーネントを使用する方法について説明します。

[COM への .NET Framework コンポーネントの公開](exposing-dotnet-components-to-com.md)  
COM アプリケーションから .NET Framework コンポーネントを使用する方法について説明します。

[アンマネージ DLL 関数の処理](consuming-unmanaged-dll-functions.md)  
プラットフォーム呼び出しを使用して、アンマネージ DLL 関数を呼び出す方法について説明します。

[相互運用マーシャリング](interop-marshaling.md)  
COM 相互運用機能とプラットフォーム呼び出しのマーシャリングについて説明します。

[方法: HRESULT に例外を割り当てる](how-to-map-hresults-and-exceptions.md)  
例外と HRESULT の間のマッピングについて説明します。

[型の等価性と埋め込まれた相互運用機能型](type-equivalence-and-embedded-interop-types.md)  
COM 型の型情報をアセンブリに埋め込む方法と、共通言語ランタイムが埋め込みの COM 型の等価性を決定する方法について説明します。

[方法: Tlbimp.exe を使用してプライマリ相互運用機能アセンブリを生成する](how-to-generate-primary-interop-assemblies-using-tlbimp-exe.md)  
*Tlbimp.exe* (タイプ ライブラリ インポーター) を使用して、プライマリ相互運用機能アセンブリを生成する方法について説明します。

[方法: プライマリ相互運用機能アセンブリを登録する](how-to-register-primary-interop-assemblies.md)  
プロジェクトで参照する前に、プライマリ相互運用機能アセンブリを登録する方法について説明します。

[登録を必要としない COM 相互運用機能](registration-free-com-interop.md)  
COM 相互運用機能によって Windows レジストリを使用せずにコンポーネントをアクティブ化する方法を説明します。

[方法: 登録を必要としないアクティベーション用の .NET Framework ベースの COM コンポーネントを構成する](configure-net-framework-based-com-components-for-reg.md)  
アプリケーション マニフェストを作成する方法と、コンポーネント マニフェストを作成して埋め込む方法について説明します。

## <a name="related-sections"></a>関連項目

[COM ラッパー](../../standard/native-interop/com-wrappers.md)  
COM 相互運用機能が提供するラッパーをについて説明します。
