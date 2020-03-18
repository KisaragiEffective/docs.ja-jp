---
title: '軽減策: パスの正規化'
ms.date: 03/30/2017
ms.assetid: 158d47b1-ba6d-4fa6-8963-a012666bdc31
ms.openlocfilehash: 61c8eec2043aa2fb9309ee6052e27fc2c91c6c6a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181229"
---
# <a name="mitigation-path-normalization"></a>軽減策: パスの正規化
.NET Framework 4.6.2 以降を対象とするアプリから、.NET Framework のパスの正規化が変更されています。  
  
## <a name="what-is-path-normalization"></a>パスの正規化とは  
 パスの正規化では、パスまたはファイルを識別する文字列を変更し、対象のオペレーティング システムの有効なパスに準拠するようにします。 通常、正規化では次のことを行います。  
  
- コンポーネントとディレクトリの区切り記号を正規化する。  
  
- 現在のディレクトリを相対パスに適用する。  
  
- パスの相対ディレクトリ (`.`) または親ディレクトリ (`..`) を評価する。  
  
- 指定した文字をトリミングする。  
  
## <a name="the-changes"></a>変更点  
 .NET Framework 4.6.2 以降を対象とするアプリから、次のようにパスの正規化が変更されています。  
  
- ランタイムはオペレーティング システムの [GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) 関数に従って、パスを正規化します。  
  
- 正規化では、ディレクトリ セグメントの末尾 (ディレクトリ名の末尾のスペースなど) がトリミングされなくなりました。  
  
- `\\.\` や `\\?\` (mscorlib.dll のファイル I/O API の場合) を含む、完全に信頼できるデバイス パス構文がサポートされます。  
  
- ランタイムではデバイス構文パスは検証されません。  
  
- 代替データ ストリームにアクセスするためのデバイス構文の使用はサポートされています。  
  
## <a name="impact"></a>影響  

.NET Framework 4.6.2 以降を対象とするアプリでは、これらの変更は既定で有効になります。 パフォーマンスが向上すると同時に、以前はアクセス不可だったパスにメソッドでアクセスできるようになります。  
  
.NET Framework 4.6.1 以前のバージョンを対象とするアプリが .NET Framework 4.6.2 以降で実行される場合、この変更の影響は受けません。  
  
## <a name="mitigation"></a>対応策  
 .NET Framework 4.6.2 以降を対象とするアプリの場合、アプリケーション構成ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに次を追加することでこの変更を無効にし、従来の正規化を使用できます。  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />
</runtime>  
```  
  
.NET Framework 4.6.1 以前を対象とするが、.NET Framework 4.6.2 以降で実行されるアプリの場合、アプリケーション構成ファイルの [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) セクションに次を追加することで、パス正規化の変更を有効にできます。  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=false" />
</runtime>  
```  
  
## <a name="see-also"></a>参照

- [アプリケーションの互換性](application-compatibility.md)
