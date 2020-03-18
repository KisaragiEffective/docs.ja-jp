---
title: '方法: コンピューター上に存在するタイムゾーンを列挙する'
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET Framework], enumerating
- enumerating time zones [.NET Framework]
ms.assetid: bb7a42ab-6bd9-4c5c-b734-5546d51f8669
ms.openlocfilehash: aa8962c8aea208778983610041937dc3f75c1f1e
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159443"
---
# <a name="how-to-enumerate-time-zones-present-on-a-computer"></a>方法: コンピューター上に存在するタイムゾーンを列挙する

指定されたタイム ゾーンを正しく処理するには、そのタイム ゾーンに関する情報がシステムで使用できる必要があります。 Windows XP および Windows Vista オペレーティングシステムでは、この情報はレジストリに格納されます。 しかし、世界中に存在するタイム ゾーンの合計数は大きいものの、レジストリにはそれらの一部に関する情報しか含まれません。 さらに、レジストリ自体は、内容が意図的に、および誤って変更される可能性がある動的な構造です。 その結果、アプリケーションは、特定のタイム ゾーンがシステムで定義され、使用できると常に想定することができません。 タイム ゾーン情報のアプリケーションを使用する多くのアプリケーションの最初の手順は、必要なタイム ゾーンがローカル システムで使用できるかどうかを判断する、またはタイム ゾーンの一覧をユーザーに提供して選択させることです。 これには、アプリケーションがローカル システムで定義されているタイム ゾーンを列挙する必要があります。

> [!NOTE]
> アプリケーションが、ローカルシステムで定義されていない特定のタイムゾーンの存在に依存している場合は、タイムゾーンに関する情報をシリアル化および逆シリアル化することによって、アプリケーションの存在を確認できます。 タイムゾーンは、アプリケーションユーザーが選択できるように、リストコントロールに追加できます。 詳細については、「[方法: 埋め込みリソースにタイムゾーンを保存](../../../docs/standard/datetime/save-time-zones-to-an-embedded-resource.md)する」および「[方法: 埋め込みリソースからタイムゾーンを復元](../../../docs/standard/datetime/restore-time-zones-from-an-embedded-resource.md)する」を参照してください。

### <a name="to-enumerate-the-time-zones-present-on-the-local-system"></a>ローカル システムに存在するタイム ゾーンを列挙するには

1. <xref:System.TimeZoneInfo.GetSystemTimeZones%2A?displayProperty=nameWithType> メソッドを呼び出します。 メソッドは、<xref:System.TimeZoneInfo> オブジェクトのジェネリック <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> コレクションを返します。 コレクション内のエントリは、<xref:System.TimeZoneInfo.DisplayName%2A> プロパティによって並べ替えられます。 次に例を示します。

   [!code-csharp[System.TimeZone2.Concepts#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#1)]
   [!code-vb[System.TimeZone2.Concepts#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#1)]

2. `foreach` ループ (でC#は) または `For Each`...`Next` を使用して、コレクション内の個々の <xref:System.TimeZoneInfo> オブジェクトを列挙します。 ループ (Visual Basic) を実行し、各オブジェクトに対して必要な処理を実行します。 たとえば、次のコードは、手順1で返された <xref:System.TimeZoneInfo> オブジェクトの <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> コレクションを列挙し、各タイムゾーンの表示名をコンソールに表示します。

   [!code-csharp[System.TimeZone2.Concepts#12](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#12)]
   [!code-vb[System.TimeZone2.Concepts#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#12)]

### <a name="to-present-the-user-with-a-list-of-time-zones-present-on-the-local-system"></a>ローカルシステム上に存在するタイムゾーンの一覧をユーザーに表示するには

1. <xref:System.TimeZoneInfo.GetSystemTimeZones%2A?displayProperty=nameWithType> メソッドを呼び出します。 メソッドは、<xref:System.TimeZoneInfo> オブジェクトのジェネリック <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> コレクションを返します。

2. 手順 1. で返されたコレクションを、Windows フォームまたは ASP.NET リストコントロールの `DataSource` プロパティに割り当てます。

3. ユーザーが選択した <xref:System.TimeZoneInfo> オブジェクトを取得します。

この例は、Windows アプリケーションの図解を示しています。

## <a name="example"></a>例

この例では、システムで定義されているタイムゾーンをリストボックスに表示する Windows アプリケーションを起動します。 この例では、ユーザーが選択したタイムゾーンオブジェクトの <xref:System.TimeZoneInfo.DisplayName%2A> プロパティの値を含むダイアログボックスが表示されます。

[!code-csharp[System.TimeZone2.Concepts#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#2)]
[!code-vb[System.TimeZone2.Concepts#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#2)]

ほとんどのリストコントロール (<xref:System.Windows.Forms.ListBox?displayProperty=nameWithType> や <xref:System.Web.UI.WebControls.BulletedList?displayProperty=nameWithType> コントロールなど) を使用すると、そのコレクションが <xref:System.Collections.IEnumerable> インターフェイスを実装している限り、オブジェクト変数のコレクションを `DataSource` プロパティに割り当てることができます。 (ジェネリック <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> クラスはこれを行います)。コレクション内の個々のオブジェクトを表示するために、コントロールはそのオブジェクトの `ToString` メソッドを呼び出して、オブジェクトを表すために使用される文字列を抽出します。 <xref:System.TimeZoneInfo> オブジェクトの場合、`ToString` メソッドは <xref:System.TimeZoneInfo> オブジェクトの表示名 (<xref:System.TimeZoneInfo.DisplayName%2A> プロパティの値) を返します。

> [!NOTE]
> リストコントロールはオブジェクトの `ToString` メソッドを呼び出すため、<xref:System.TimeZoneInfo> オブジェクトのコレクションをコントロールに割り当て、各オブジェクトに対してわかりやすい名前をコントロールに表示し、ユーザーが選択した <xref:System.TimeZoneInfo> オブジェクトを取得することができます。 これにより、コレクション内の各オブジェクトに対して文字列を抽出し、その文字列をコントロールの `DataSource` プロパティに割り当てられたコレクションに割り当て、ユーザーが選択した文字列を取得して、この文字列を使用して記述されたオブジェクトを抽出する必要がなくなります。

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- 次の名前空間がインポートされます。

  <xref:System> (コードC#内)

  <xref:System.Collections.ObjectModel>

## <a name="see-also"></a>参照

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
- [方法: 埋め込みリソースにタイム ゾーンを保存する](../../../docs/standard/datetime/save-time-zones-to-an-embedded-resource.md)
- [方法: 埋め込みリソースからタイム ゾーンを復元する](../../../docs/standard/datetime/restore-time-zones-from-an-embedded-resource.md)
