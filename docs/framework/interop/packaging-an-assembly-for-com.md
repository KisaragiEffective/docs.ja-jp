---
title: COM 向け .NET Framework アセンブリのパッケージ化
ms.date: 03/30/2017
helpviewer_keywords:
- exposing .NET Framework components to COM
- COM interop, packaging assemblies
- packaging assemblies for COM
- Tlbexp.exe
- TypeLibConverter class
- .NET Services Installation tool
- Assembly Registration tool
- Type Library Exporter
- Reqsvcs.exe
- interoperation with unmanaged code, exposing .NET Framework components
- interoperation with unmanaged code, packaging assemblies
- COM interop, exposing COM components
- Reqasm.exe
ms.assetid: 39dc55aa-f2a1-4093-87bb-f1c0edb6e761
ms.openlocfilehash: 6866da283cc7cdd180aada252007d67bd72cd862
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124095"
---
# <a name="packaging-a-net-framework-assembly-for-com"></a>COM 向け .NET Framework アセンブリのパッケージ化

COM 開発者がアプリケーションに組み込むときに役立つ、マネージド型に関する情報を次に示します。

- COM アプリケーションで使用できる型の一覧

  マネージド型には、COM から参照できない型、参照可能だが作成できない型、および参照と作成の両方が可能な型があります。 アセンブリは、参照できない型、参照できる型、作成できない型、作成できる型を任意に組み合わせて構成できます。 完全を期すために、COM に公開するアセンブリ内の型を識別する必要があります。特に COM に公開するアセンブリ内の型が .NET Framework に公開されている型のサブセットである場合に型の識別が必要になります。

  追加情報については、「[相互運用のための .NET 型の要件](../../standard/native-interop/qualify-net-types-for-interoperation.md)」を参照してください。

- バージョン管理に関する注意事項

  クラス インターフェイス (COM 相互運用機能により生成されたインターフェイス) を実装したマネージド クラスには、バージョン管理に関する制約が生じる場合があります。

  クラス インターフェイスの使用に関するガイドラインについては、「[クラス インターフェイスの概要](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface)」をご覧ください。

- 配置に関する注意事項

  発行者により署名された厳密な名前のアセンブリは、グローバル アセンブリ キャッシュにインストールできます。 署名のないアセンブリは、プライベート アセンブリとしてユーザーのコンピューターにインストールする必要があります。

  詳細については、「[アセンブリのセキュリティに関する考慮事項](../../standard/assembly/security-considerations.md)」を参照してください。

- タイプ ライブラリのインクルード

  大部分の型は、COM アプリケーションで処理されるときにタイプ ライブラリが必要です。 タイプ ライブラリの生成は、自分で行うことも、COM 開発者に任せることもできます。 Windows SDK には、タイプ ライブラリを生成するための次のオプションが用意されています。

  - [タイプ ライブラリ エクスポーター](#cpconpackagingassemblyforcomanchor1)

  - [TypeLibConverter クラス](#cpconpackagingassemblyforcomanchor2)

  - [アセンブリ登録ツール](#cpconpackagingassemblyforcomanchor3)

  - [.NET サービス インストール ツール](#cpconpackagingassemblyforcomanchor4)

  どの機構を選択した場合でも、提供するアセンブリ内で定義されたパブリック型だけが、生成されるタイプ ライブラリに含まれます。

手順については、「[How to: Embed Type Libraries as Win32 Resources in .NET-Based Applications](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ww9a897z(v=vs.100))」(方法: タイプ ライブラリを Win32 リソースとして .NET ベースのアプリケーションに埋め込む) を参照してください。

<a name="cpconpackagingassemblyforcomanchor1"></a>

## <a name="type-library-exporter"></a>タイプ ライブラリ エクスポーター

[タイプ ライブラリ エクスポーター (Tlbexp.exe)](../tools/tlbexp-exe-type-library-exporter.md) は、アセンブリに含まれているクラスおよびインターフェイスを COM タイプ ライブラリに変換するコマンド ライン ツールです。 クラスの型情報が利用可能になると、COM クライアントは .NET クラスのインスタンスを生成し、COM オブジェクトの場合と同じ方法でインスタンスのメソッドを呼び出すことができます。 Tlbexp.exe により、一度に 1 つのアセンブリ全体が変換されます。 Tlbexp.exe を使用しても、アセンブリで定義されている型のサブセットに関する型情報は生成できません。

<a name="cpconpackagingassemblyforcomanchor2"></a>

## <a name="typelibconverter-class"></a>TypeLibConverter クラス

**System.Runtime.Interop** 名前空間にある <xref:System.Runtime.InteropServices.TypeLibConverter> クラスは、アセンブリに含まれているクラスおよびインターフェイスを COM タイプ ライブラリに変換します。 この API は、前のセクションで説明したタイプ ライブラリ エクスポーターと同じ型情報を生成します。

**TypeLibConverter クラス**は、<xref:System.Runtime.InteropServices.ITypeLibConverter> を実装しています。

<a name="cpconpackagingassemblyforcomanchor3"></a>

## <a name="assembly-registration-tool"></a>アセンブリ登録ツール

[アセンブリ登録ツール (Regasm.exe)](../tools/regasm-exe-assembly-registration-tool.md) は、 **/tlb:** オプションを適用すると、タイプ ライブラリを生成および登録できます。 COM クライアントでは、タイプ ライブラリを Windows のレジストリにインストールする必要があります。 このオプションを適用しない場合、Regasm.exe はアセンブリの型だけを登録し、タイプ ライブラリは登録しません。 アセンブリの型を登録することと、タイプ ライブラリを登録することは、まったく別の作業です。

<a name="cpconpackagingassemblyforcomanchor4"></a>

## <a name="net-services-installation-tool"></a>.NET サービス インストール ツール

[.NET サービス インストール ツール (Regsvcs.exe)](../tools/regsvcs-exe-net-services-installation-tool.md) は、Windows 2000 コンポーネント サービスにマネージド クラスを追加したり、1 つのツール内の複数のタスクを統合したりします。 Regsvcs.exe は、アセンブリの読み込みおよび登録だけでなく、タイプ ライブラリの生成、登録、および既存の COM+ 1.0 アプリケーションへのインストールができます。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.InteropServices.TypeLibConverter>
- <xref:System.Runtime.InteropServices.ITypeLibConverter>
- [COM への .NET Framework コンポーネントの公開](exposing-dotnet-components-to-com.md)
- [COM 相互運用のための .NET 型の要件](../../standard/native-interop/qualify-net-types-for-interoperation.md)
- [クラス インターフェイスの概要](../../standard/native-interop/com-callable-wrapper.md#introducing-the-class-interface)
- [アセンブリのセキュリティに関する考慮事項](../../standard/assembly/security-considerations.md)
- [Tlbexp.exe (タイプ ライブラリ エクスポーター)](../tools/tlbexp-exe-type-library-exporter.md)
- [COM へのアセンブリの登録](registering-assemblies-with-com.md)
- [方法: タイプ ライブラリを Win32 リソースとしてアプリケーションに埋め込む](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ww9a897z(v=vs.100))
