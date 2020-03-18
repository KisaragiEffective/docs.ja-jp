---
title: .NET Framework による複数のプラットフォームの開発
ms.date: 07/18/2018
ms.technology: dotnet-standard
ms.assetid: b153baaa-130c-4169-860b-e580591de91e
ms.openlocfilehash: 0b7a2b4d234227a4901a461dba5362cada6ed4cf
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124378"
---
# <a name="developing-for-multiple-platforms-with-the-net-framework"></a>.NET Framework による複数のプラットフォームの開発

.NET Framework と Visual Studio を使用して、Microsoft プラットフォームと Microsoft 以外のプラットフォームの両方を対象としたアプリを開発できます。
  
## <a name="options-for-cross-platform-development"></a>クロスプラットフォーム開発のオプション

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]
  
 複数プラットフォームを対象として開発する場合は、ソース コードやバイナリを共有し、.NET Framework コードと Windows ランタイム API の間で呼び出しを実行できます。  
  
|目的|用途|  
|-----------------------|------------|  
|Windows Phone 8.1 アプリと Windows 8.1 アプリの間でソース コードを共有する|**共有プロジェクト**(Visual Studio 2013、Update 2 のユニバーサルアプリテンプレート)。<br /><br /> -現在 Visual Basic はサポートしていません。<br />-#`if` ステートメントを使用して、プラットフォーム固有のコードを分離することができます。<br /><br /> 詳細については、次の情報を参照してください。<br /><br /> [コーディングの開始](/windows/uwp/get-started/create-uwp-apps)-   <br />[Visual Studio を使用してユニバーサル XAML アプリをビルドする](https://devblogs.microsoft.com/visualstudio/using-visual-studio-to-build-universal-xaml-apps/)-   (ブログの投稿)<br />[Visual Studio を使用して XAML 収束アプリを構築](https://channel9.msdn.com/Events/Build/2014/3-591)-   (ビデオ)|  
|異なるプラットフォームを対象とするアプリ間でバイナリを共有する|プラットフォームに依存しないコードの**ポータブルクラスライブラリプロジェクト**。<br /><br /> -この方法は、通常、ビジネスロジックを実装するコードに使用されます。<br />-Visual Basic またはC#を使用できます。<br />-API のサポートはプラットフォームによって異なります。<br />-Windows 8.1 を対象とするポータブルクラスライブラリプロジェクトと Windows Phone 8.1 サポート Windows ランタイム Api および XAML。 これらの機能は、古いバージョンのポータブル クラス ライブラリでは使用できません。<br />-必要に応じて、インターフェイスまたは抽象クラスを使用して、プラットフォーム固有のコードを抽象化できます。<br /><br /> 詳細については、次の情報を参照してください。<br /><br /> [ポータブルクラスライブラリ](cross-platform-development-with-the-portable-class-library.md)の -   <br />[ポータブルクラスライブラリの機能](https://docs.microsoft.com/archive/blogs/dsplaisted/how-to-make-portable-class-libraries-work-for-you)を -   する方法 (ブログの投稿)<br />[MVVM でポータブルクラスライブラリを使用する](using-portable-class-library-with-model-view-view-model.md)-    <br />[複数のプラットフォームを対象とするライブラリのアプリリソースの](app-resources-for-libraries-that-target-multiple-platforms.md)-    <br />-   [.Net 移植性アナライザー](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) (Visual Studio 拡張機能)|  
|Windows 8.1 および Windows Phone 8.1 以外のプラットフォームを対象としたアプリの間でソース コードを共有する|**リンクとして追加**機能。<br /><br /> -この方法は、何らかの理由で、両方のアプリに共通のアプリロジックに適していますが、移植できません。 この機能は Visual Basic または Visual C# のコードに使用できます。<br />     たとえば Windows Phone 8 と Windows 8 は Windows ランタイム API を共有しますが、ポータブル クラス ライブラリはこれらのプラットフォームでは Windows ランタイムをサポートしていません。 Windows Phone 8 アプリと、Windows 8 を対象とした Windows ストア アプリの間で共通する Windows ランタイム コードを共有するには、`Add as link` を使用できます。<br /><br /> 詳細については、次の情報を参照してください。<br /><br /> [リンクとして追加 -   でコードを共有](https://docs.microsoft.com/previous-versions/windows/apps/jj714082(v=vs.105))する<br />-   [方法: プロジェクトに既存の項目を追加](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/9f4t9t92(v=vs.100))する|  
|.NET Framework を使用して Windows ストア アプリを作成するか、または .NET Framework コードから Windows ランタイム API を呼び出します。|.NET Framework C#または Visual Basic コードから api を Windows ランタイムし、.NET Framework を使用して Windows ストアアプリを作成します。 2 つのプラットフォーム間の API の違いについて注意してください。 ただし、これらの違いに対処するためのクラスがあります。<br /><br /> 詳細については、次の情報を参照してください。<br /><br /> [Windows ストアアプリと Windows ランタイムの -   .NET Framework サポート](support-for-windows-store-apps-and-windows-runtime.md) <br />[URI を Windows ランタイムに渡す](passing-a-uri-to-the-windows-runtime.md)-    <br />-   <xref:System.IO.WindowsRuntimeStreamExtensions><br />-    <xref:System.WindowsRuntimeSystemExtensions>|  
|Microsoft 以外のプラットフォームに対応した .NET Framework アプリの開発|.NET Framework の**ポータブルクラスライブラリ参照アセンブリ**と、Visual Studio 拡張機能または Xamarin などのサードパーティツール。<br /><br /> 詳細については、次の情報を参照してください。<br /><br /> [すべてのプラットフォームでポータブルクラスライブラリを使用できるようになりました -   。](https://devblogs.microsoft.com/dotnet/portable-class-library-pcl-now-available-on-all-platforms/) (ブログの投稿)<br />[Xamarin ドキュメント](/xamarin)の -   |  
|JavaScript および HTML を使用したクロスプラットフォーム開発|Visual Studio 2013、Update 2 の**ユニバーサルアプリテンプレート**は、Windows 8.1 および Windows Phone 8.1 の Windows ランタイム api に対して開発します。 現在、クロスプラットフォーム アプリを開発するときに .NET Framework API で JavaScript と HTML を使用することはできません。<br /><br /> 詳細については、次の情報を参照してください。<br /><br /> -   [JavaScript プロジェクトテンプレート](https://docs.microsoft.com/previous-versions/windows/apps/hh758331%28v=win.10%29)<br />[JavaScript を使用して Windows ランタイムアプリを Windows Phone に移植](https://docs.microsoft.com/previous-versions/windows/apps/dn636144%28v=win.10%29)-   |
