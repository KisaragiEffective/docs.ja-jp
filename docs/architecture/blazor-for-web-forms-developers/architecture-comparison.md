---
title: ASP.NET Web Forms と Blazor のアーキテクチャの比較
description: ASP.NET Web Forms と Blazor のアーキテクチャの違いについて説明します。
author: danroth27
ms.author: daroth
ms.date: 09/11/2019
ms.openlocfilehash: 8ff733178e684666b69859bfab8b79fbad1fdff6
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "73841241"
---
# <a name="architecture-comparison-of-aspnet-web-forms-and-blazor"></a>ASP.NET Web Forms と Blazor のアーキテクチャの比較

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ASP.NET の Web フォームと Blazor には多くの類似した概念がありますが、そのしくみには違いがあります。 この章では、ASP.NET Web Forms と Blazor の内部の動作とアーキテクチャについて説明します。

## <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

ASP.NET Web Forms framework は、ページ中心のアーキテクチャに基づいています。 アプリ内の場所に対する各 HTTP 要求は、ASP.NET が応答する別のページです。 ページが要求されると、ブラウザーのコンテンツは要求されたページの結果に置き換えられます。

ページは次のコンポーネントで構成されています。

- HTML マークアップ
- C# または Visual Basic コード
- ロジックおよびイベント処理機能を含む分離コードクラス
- Controls

コントロールは、ページ上でプログラムによって配置および操作できる web UI の再利用可能な単位です。 ページは、マークアップ、コントロール、および一部のコードを含む *.aspx*で終わるファイルで構成されます。 分離コードクラスは、使用するプログラミング言語に応じて、同じ基本名と*aspx.cs*または *.aspx*拡張子を持つファイルにあります。 興味深いことに、web サーバーは .aspx ファイルの内容を解釈し、変更されるたびにコンパイルし*ます*。 この再コンパイルは、web サーバーが既に実行されている場合でも発生します。

コントロールは、マークアップを使用して作成し、ユーザーコントロールとして配信できます。 ユーザーコントロールは `UserControl` クラスから派生し、ページと同様の構造を持ちます。 ユーザーコントロールのマークアップは *.ascx*ファイルに格納されます。 付随する分離コードクラスは、 *ascx.cs*ファイルまたは *.ascx*ファイルに存在します。 コントロールは、`WebControl` または `CompositeControl` 基底クラスから継承することにより、コードを使用して完全にビルドすることもできます。

ページには、広範なイベントライフサイクルもあります。 各ページは、ASP.NET ランタイムが各要求に対してページのコードを実行するときに発生する、初期化、読み込み、prerender、およびアンロードイベントに対してイベントを発生させます。

通常、ページ上のコントロールは、コントロールが表示されたのと同じページにポストバックされ、`ViewState`と呼ばれる非表示のフォームフィールドからのペイロードと共に使用されます。 `ViewState` フィールドには、コントロールがページに表示および表示されたときのコントロールの状態に関する情報が含まれています。これにより、ASP.NET ランタイムは、サーバーに送信されたコンテンツの変更を比較および識別できます。

## <a name="blazor"></a>Blazor

Blazor は、クライアント側の web UI フレームワークであり、角度や反応などの JavaScript フロントエンドフレームワークに似ています。 Blazor はユーザーとの対話を処理し、必要な UI の更新をレンダリングします。 Blazor は、要求/応答モデルに基づいてい*ませ*ん。 ユーザー操作は、特定の HTTP 要求のコンテキストに含まれていないイベントとして処理されます。

Blazor アプリは、HTML ページにレンダリングされる1つ以上のルートコンポーネントで構成されます。

![HTML での Blazor コンポーネント](./media/architecture-comparison/blazor-components-in-html.png)

ユーザーがコンポーネントを表示する場所を指定する方法と、ユーザーの操作のためにコンポーネントを接続する方法は、モデル固有の[ホスト](hosting-models.md)となります。

Blazor[コンポーネント](components.md)は、再利用可能な UI を表す .net クラスです。 各コンポーネントは、独自の状態を保持し、独自のレンダリングロジックを指定します。これには、他のコンポーネントのレンダリングを含めることができます。 コンポーネントは、コンポーネントの状態を更新するために、特定のユーザー操作のイベントハンドラーを指定します。

コンポーネントがイベントを処理した後、Blazor はコンポーネントをレンダリングし、レンダリングされた出力の変更内容を追跡します。 コンポーネントは、ドキュメントオブジェクトモデル (DOM) に直接はレンダリングされません。 代わりに、Blazor が変更を追跡できるように、`RenderTree` と呼ばれる DOM のメモリ内表現にレンダリングします。 Blazor は、新しくレンダリングされた出力を前の出力と比較して、UI の相違を計算します。これは、DOM に効率よく適用されます。

![Blazor DOM の相互作用](./media/architecture-comparison/blazor-dom-interaction.png)

また、コンポーネントの状態が通常の UI イベントの外部で変更された場合に、それらのコンポーネントがレンダリングされることを手動で示すこともできます。 Blazor は、1つの論理スレッドの実行を強制するために `SynchronizationContext` を使用します。 Blazor によって発生するコンポーネントのライフサイクルメソッドとイベントコールバックは、この `SynchronizationContext`で実行されます。

>[!div class="step-by-step"]
>[前へ](introduction.md)
>[次へ](hosting-models.md)
