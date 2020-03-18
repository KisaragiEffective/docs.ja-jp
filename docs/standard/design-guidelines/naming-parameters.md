---
title: パラメーターに名前を付ける
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- parameters, names
- names [.NET Framework], parameters
ms.assetid: ca3c956e-725a-441b-b4e3-eab5d472f41c
ms.openlocfilehash: ebe2e2db4b109057bf576d4e18cfe511c657707e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743832"
---
# <a name="naming-parameters"></a>パラメーターに名前を付ける
読みやすくするために、パラメーター名のガイドラインに従うことが重要です。これは、ビジュアルデザインツールが Intellisense とクラス参照機能を提供する場合に、ドキュメントとデザイナーにパラメーターが表示されるためです。

 パラメーター名にキャメルを使用する✔️ます。

 ✔️、説明的なパラメーター名を使用します。

 パラメーターの型ではなく、パラメーターの意味に基づいて名前を使用することを✔️してください。

### <a name="naming-operator-overload-parameters"></a>演算子のオーバーロードパラメーターの名前付け
 パラメーターに意味がない場合は、バイナリ演算子のオーバーロードパラメーター名に `left` と `right` を使用✔️ます。

 パラメーターに意味がない場合は、単項演算子のオーバーロードパラメーター名に `value` を使用✔️ます。

 ✔️演算子を使用すると、有意な値が追加されます。

 ❌ 演算子のオーバーロードパラメーター名には、省略形または数値インデックスを使用しないでください。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [名前付けのガイドライン](../../../docs/standard/design-guidelines/naming-guidelines.md)
