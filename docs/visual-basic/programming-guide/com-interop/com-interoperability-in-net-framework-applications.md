---
title: .NET Framework アプリケーションにおける COM 相互運用性
ms.date: 07/20/2015
helpviewer_keywords:
- interoperability, COM and .NET Framework objects
- COM interop [Visual Basic]
- shared components
ms.assetid: f5a72143-c268-4dff-a019-974ad940e17d
ms.openlocfilehash: c3567464616d3b0b3f91ff57e8a169aca046c866
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452293"
---
# <a name="com-interoperability-in-net-framework-applications-visual-basic"></a>.NET Framework アプリケーションにおける COM 相互運用性 (Visual Basic)

COM オブジェクトと .NET Framework オブジェクトを同じアプリケーションで使用する場合は、オブジェクトがメモリ内に存在する方法の違いに対処する必要があります。 .NET Framework オブジェクトはマネージメモリ (共通言語ランタイムによって制御されるメモリ) にあり、必要に応じてランタイムによって移動される場合があります。 COM オブジェクトはアンマネージメモリに配置されており、別のメモリ位置に移動することは想定されていません。 Visual Studio と .NET Framework には、これらのマネージコンポーネントとアンマネージコンポーネントの相互作用を制御するツールが用意されています。 マネージコードの詳細については、「[共通言語ランタイム](../../../standard/clr.md)」を参照してください。

.NET アプリケーションで COM オブジェクトを使用するだけでなく、Visual Basic を使用して、COM を介してアンマネージコードからアクセスできるオブジェクトを開発することもできます。

このページのリンクを使用すると、COM オブジェクトと .NET Framework オブジェクトの間の相互作用についての詳細が表示されます。

## <a name="related-sections"></a>関連項目

| | |
|---------|---------|
| [COM 相互運用](../../../visual-basic/programming-guide/com-interop/index.md) | Com オブジェクト、ActiveX コントロール、Win32 Dll、マネージオブジェクト、COM オブジェクトの継承など、Visual Basic の COM 相互運用性に関するトピックへのリンクを示します。 |
| [アンマネージ コードとの相互運用](../../../framework/interop/index.md) | マネージコードとアンマネージコードの間の相互作用の問題について簡単に説明し、さらに学習するためのリンクを示します。 |
| [COM ラッパー](../../../standard/native-interop/com-wrappers.md) | マネージコードが COM メソッドを呼び出すことができるランタイム呼び出し可能ラッパー、および com クライアントが .NET オブジェクトメソッドを呼び出すことができるようにする COM 呼び出し可能ラッパーについて説明します。 |
| [高度な COM 相互運用性](../../../framework/interop/index.md) | ラッパー、例外、継承、スレッド処理、イベント、変換、およびマーシャリングに関して、COM の相互運用性に関するトピックへのリンクを示します。 |
| [Tlbimp.exe (タイプ ライブラリ インポーター)](../../../framework/tools/tlbimp-exe-type-library-importer.md) | COM タイプライブラリ内の型定義を共通言語ランタイムアセンブリで等価な定義に変換するために使用できるツールについて説明します。 |
