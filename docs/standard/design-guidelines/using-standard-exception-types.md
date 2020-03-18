---
title: 標準例外型の使用
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- throwing exceptions, standard types
- catching exceptions
- exceptions, catching
- exceptions, throwing
ms.assetid: ab22ce03-78f9-4dca-8824-c7ed3bdccc27
ms.openlocfilehash: 3caa94d9a39966614161e4b19201dcf6065776a2
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743545"
---
# <a name="using-standard-exception-types"></a>標準例外型の使用
このセクションでは、フレームワークによって提供される標準の例外とその使用法の詳細について説明します。 リストは、完全な手段ではありません。 他のフレームワークの例外の種類の使用方法については、.NET Framework リファレンスドキュメントを参照してください。

## <a name="exception-and-systemexception"></a>Exception および SystemException
 ❌ <xref:System.Exception?displayProperty=nameWithType> または <xref:System.SystemException?displayProperty=nameWithType>をスローしません。

 を再スローする場合を除き、❌ はフレームワークコードで `System.Exception` または `System.SystemException` をキャッチしません。

 最上位レベルの例外ハンドラーを除き、`System.Exception` または `System.SystemException`をキャッチしないように ❌ します。

## <a name="applicationexception"></a>ApplicationException
 ❌ は <xref:System.ApplicationException>からスローまたは派生しません。

## <a name="invalidoperationexception"></a>InvalidOperationException
 オブジェクトが不適切な状態にある場合は、✔️ <xref:System.InvalidOperationException> をスローします。

## <a name="argumentexception-argumentnullexception-and-argumentoutofrangeexception"></a>ArgumentException、System.argumentnullexception、ArgumentOutOfRangeException
 無効な引数がメンバーに渡されると、<xref:System.ArgumentException> またはそのサブタイプの1つをスロー✔️ます。 最も派生した例外の種類を優先します (該当する場合)。

 ✔️ `ArgumentException`のサブクラスの1つをスローするときに `ParamName` プロパティを設定します。

 このプロパティは、例外がスローされる原因となったパラメーターの名前を表します。 プロパティは、コンストラクターのオーバーロードのいずれかを使用して設定できます。

 ✔️は、プロパティ setter の暗黙的な値パラメーターの名前に `value` を使用します。

## <a name="nullreferenceexception-indexoutofrangeexception-and-accessviolationexception"></a>NullReferenceException、IndexOutOfRangeException、および Access Ationexception
 ❌ は、<xref:System.NullReferenceException>、<xref:System.AccessViolationException>、または <xref:System.IndexOutOfRangeException>を明示的または暗黙的にスローするために、公開されている呼び出し可能な Api を許可しません。 これらの例外は、実行エンジンによって予約およびスローされ、ほとんどの場合、バグを示します。

 これらの例外をスローしないように引数をチェックします。 これらの例外をスローすると、時間の経過と共に変化する可能性のあるメソッドの実装の詳細が表示されます。

## <a name="stackoverflowexception"></a>StackOverflowException
 ❌ <xref:System.StackOverflowException>を明示的にスローしません。 例外は、CLR によってのみ明示的にスローされる必要があります。

 ❌ は `StackOverflowException`をキャッチしません。

 任意のスタックオーバーフローの有無に一貫性を持たせるマネージコードを記述することはほとんど不可能です。 CLR のアンマネージ部分は、プローブを使用してスタックオーバーフローを適切に定義された場所に移動することによって一貫性を保ちます。これは、任意のスタックオーバーフローからのバックアップではありません。

## <a name="outofmemoryexception"></a>OutOfMemoryException
 ❌ <xref:System.OutOfMemoryException>を明示的にスローしません。 この例外は、CLR インフラストラクチャによってのみスローされます。

## <a name="comexception-sehexception-and-executionengineexception"></a>ComException、SEHException、および System.executionengineexception
 ❌、<xref:System.Runtime.InteropServices.COMException>、<xref:System.ExecutionEngineException>、および <xref:System.Runtime.InteropServices.SEHException>を明示的にスローしません。 これらの例外は、CLR インフラストラクチャによってのみスローされます。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [例外のデザインのガイドライン](../../../docs/standard/design-guidelines/exceptions.md)
