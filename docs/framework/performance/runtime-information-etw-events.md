---
title: ランタイム情報 ETW イベント
ms.date: 03/30/2017
helpviewer_keywords:
- runtime information events [.NET Framework]
- ETW, runtime information events
ms.assetid: 68b4edbc-7f3b-45f6-ab75-4fd066d6af9a
ms.openlocfilehash: 2927ed088ba6c9e46b9676d55d0046575e23cfb1
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75715951"
---
# <a name="runtime-information-etw-events"></a>ランタイム情報 ETW イベント
これらの ETW イベントは、SKU、バージョン番号、ランタイムのアクティブ化の方法、起動時に使用されたコマンド ライン パラメーター、GUID (該当する場合) などのランタイムに関する情報をログに記録します。 1 つのプロセスで複数のランタイムが実行されている場合は、これらのイベントの情報 (ClrInstanceID) によって、ランタイムのあいまいさを解消できます。  
  
 次の表に、2 つのランタイム情報イベントを示します。 これらのイベントは、任意のキーワードまたはマスクで発生させることができます (詳細については、「 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)」を参照してください)。  
  
|Event|イベント ID|プロバイダー|説明|  
|-----------|--------------|--------------|-----------------|  
|`RuntimeInformationEvent`|187|CLRRuntime|ランタイムが読み込まれたときに発生します。|  
|`RuntimeInformationDCStart`|187|CLRRundown|読み込まれているランタイムを列挙します。|  
  
 次の表にイベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
|Sku|win:UInt16|1 – デスクトップ CLR。<br /><br /> 2 – CoreCLR。|  
|BclVersion – メジャー バージョン|win:UInt16|mscorlib.dll のメジャー バージョン。|  
|BclVersion – マイナー バージョン|win:UInt16|mscorlib.dll のマイナー バージョン番号。|  
|BclVersion – ビルド番号|win:UInt16|mscorlib.dll のビルド番号。|  
|BclVersion – QFE|win:UInt16|mscorlib.dll の修正プログラムのバージョン番号。|  
|VMVersion – メジャー バージョン|win:UInt16|clr.dll または coreclr.dll (SKU によって決まる) のバージョン。|  
|VMVersion – マイナー バージョン|win:UInt16|clr.dll または coreclr.dll (SKU によって決まる) のマイナー バージョン。|  
|VMVersion – ビルド番号|win:UInt16|clr.dll または coreclr.dll のビルド番号。|  
|VMVersion – QFE|win:UInt16|clr.dll または coreclr.dll の修正プログラムのバージョン番号。|  
|StartupFlags|win:UInt32|mscoree.h で定義された起動フラグ。|  
|StartupMode|win:UInt8|0x01 - マネージド実行可能ファイル。<br /><br /> 0x02 - ホストされた CLR。<br /><br /> 0x04 - C++ マネージド相互運用。<br /><br /> 0x08 - COM アクティブ化。<br /><br /> 0x10 - その他。|  
|CommandLine|win:UnicodeString|StartupMode=0x01 の場合のみ null 以外。|  
|ComObjectGUID|win:GUID|StartupMode=0x08 の場合のみ null 以外。|  
|RuntimeDLLPath|win:UnicodeString|プロセスに読み込まれた CLR .dll ファイルへのパス。|  
  
## <a name="see-also"></a>関連項目

- [CLR ETW イベント](clr-etw-events.md)
