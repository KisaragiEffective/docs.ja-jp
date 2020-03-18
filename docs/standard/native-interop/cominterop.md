---
title: .NET での COM 相互運用
description: .NET で COM ライブラリを相互運用する方法について説明します。
ms.date: 07/11/2019
ms.openlocfilehash: 63205ae5c09d6c3929da13788eb781cd975ca814
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77627375"
---
# <a name="com-interop-in-net"></a>.NET での COM 相互運用

コンポーネント オブジェクト モデル (COM) では、オブジェクトがその機能を他のコンポーネントに公開し、Windows プラットフォームのアプリケーションをホストすることができます。 ユーザーが既存のコード ベースを使用して相互運用できるように支援するために、.NET Framework は、COM ライブラリとの相互運用に向けて常に強力なサポートを行っています。 .NET Core 3.0 では、このサポートの大部分が、Windows の .NET Core に追加されました。 このドキュメントでは、一般的な COM 相互運用テクノロジのしくみと、それらを使用して既存の COM ライブラリと相互運用する方法について説明します。

- [COM ラッパー](./com-wrappers.md)
- [COM 呼び出し可能ラッパー](./com-callable-wrapper.md)
- [ランタイム呼び出し可能ラッパー](./runtime-callable-wrapper.md)
- [COM 相互運用のための .NET 型の要件](./qualify-net-types-for-interoperation.md)
