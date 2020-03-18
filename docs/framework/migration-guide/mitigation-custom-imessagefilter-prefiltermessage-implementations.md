---
title: '軽減策: カスタムの IMessageFilter.PreFilterMessage 実装'
ms.date: 03/30/2017
ms.assetid: 9cf47c5b-0bb2-45df-9437-61cd7e7c2f4d
ms.openlocfilehash: 7757e8d1fd0258ab2d972b7321082e4afa37f710
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398647"
---
# <a name="mitigation-custom-imessagefilterprefiltermessage-implementations"></a>軽減策: カスタムの IMessageFilter.PreFilterMessage 実装

.NET Framework の .NET Framework 4.6.1 以降のバージョンを対象とする Windows フォーム アプリでは、<xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> メソッドが呼び出されると、カスタムの <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 実装は <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> 実装が次のような場合に、メッセージを安全にフィルター処理できます。

- 次の操作のいずれか、または両方を行う場合:

  - <xref:System.Windows.Forms.Application.AddMessageFilter%2A> メソッドを呼び出してメッセージ フィルターを追加する。

  - <xref:System.Windows.Forms.Application.RemoveMessageFilter%2A> メソッドを呼び出してメッセージ フィルターを削除する。 メソッドをオーバーライドします。

- **なおかつ**、<xref:System.Windows.Forms.Application.DoEvents%2A?displayProperty=nameWithType> メソッドを呼び出してメッセージをポンプする場合。

## <a name="impact"></a>影響

この変更によって影響を受けるのは、.NET Framework の .NET Framework 4.6.1 以降のバージョンを対象とする Windows フォーム アプリのみです。

.NET Framework の以前のバージョンを対象とする Windows フォーム アプリの場合、このような実装で、<xref:System.IndexOutOfRangeException> メソッドが呼び出されると <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> 例外がスローされることがあります。

## <a name="mitigation"></a>対応策

この変更が望ましくない場合は、.NET Framework 4.6.1 以降のバージョンを対象とするアプリでこの変更を無効にできます。これは、そのアプリの構成ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに次の構成設定を追加して行います。

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=true" />
</runtime>
```

また、以前のバージョンの .NET Framework を対象とするものの、.NET Framework 4.6.1 以降のバージョンで実行されているアプリでは、そのアプリの構成ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに構成設定を追加して、この動作を有効にすることができます。

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=false" />
</runtime>
```

## <a name="see-also"></a>参照

- [アプリケーションの互換性](application-compatibility.md)
