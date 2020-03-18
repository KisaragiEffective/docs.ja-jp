---
title: イベントとコールバック
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- events [.NET Framework], extensibility
- methods [.NET Framework], callback
- callback methods
- callbacks
ms.assetid: 48b55c60-495f-4089-9396-97f9122bba7c
ms.openlocfilehash: 7dab759ba48104530fc41e46f6f2bba18d6c4456
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741657"
---
# <a name="events-and-callbacks"></a>イベントとコールバック
コールバックは、フレームワークがデリゲートを通じてユーザーコードにコールバックすることを可能にする拡張ポイントです。 これらのデリゲートは、通常、メソッドのパラメーターを使用してフレームワークに渡されます。

 イベントは、デリゲート (イベントハンドラー) を提供するための便利で一貫性のある構文をサポートする、特殊なコールバックのケースです。 さらに、Visual Studio のステートメント入力候補とデザイナーは、イベントベースの Api の使用に関するヘルプを提供します。 (「[イベントの設計](../../../docs/standard/design-guidelines/event.md)」を参照してください)。

 ✔️コールバックを使用して、ユーザーがフレームワークによって実行されるカスタムコードを提供できるようにすることを検討してください。

 ✔️イベントを使用して、ユーザーがオブジェクト指向設計を理解しなくても、フレームワークの動作をカスタマイズできるようにすることを検討してください。

 ✔️は、より幅広い開発者を対象とし、Visual Studio のステートメント入力候補と統合されるため、プレーンコールバックでイベントを優先します。

 パフォーマンスを重視する Api では、コールバックを使用しないように ❌ します。

 コールバックを使用して Api を定義するときは、カスタムデリゲートではなく、新しい `Func<...>`、`Action<...>`、または `Expression<...>` 型を使用✔️ます。

 `Func<...>` と `Action<...>` は汎用デリゲートを表します。 `Expression<...>` は、コンパイル後に実行時に呼び出すことができる関数定義を表しますが、シリアル化してリモートプロセスに渡すこともできます。

 ✔️は、`Func<...>` と `Action<...>` デリゲートを使用する代わりに、`Expression<...>`を使用した場合のパフォーマンスへの影響を測定して理解します。

 `Expression<...>` 型は、ほとんどの場合、`Func<...>` と `Action<...>` デリゲートと論理的に等価です。 これらの主な違いは、デリゲートはローカルプロセスシナリオで使用することを意図していることです。式は、リモートのプロセスまたはコンピューターで式を評価することが有益であるケースを対象としています。

 デリゲートを呼び出すことによって、任意のコードを実行し、セキュリティ、正確性、および互換性の影響を与える可能性があることを✔️理解することができます。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [機能拡張のデザイン](../../../docs/standard/design-guidelines/designing-for-extensibility.md)
- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
