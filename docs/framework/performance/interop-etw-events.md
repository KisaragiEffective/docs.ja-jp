---
title: 相互運用 ETW イベント
ms.date: 03/30/2017
helpviewer_keywords:
- interop events [.NET Framework]
- ETW, interop events (CLR)
ms.assetid: eb6eac2e-45f4-4923-a32c-38f203da66df
ms.openlocfilehash: 80fd1f7487dbe3925b875e728eaeddac86927ad4
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716018"
---
# <a name="interop-etw-events"></a>相互運用 ETW イベント
相互運用イベントは、Microsoft intermediate language (MSIL) のスタブ生成とキャッシュに関する情報をキャプチャします。  

## <a name="ilstubgenerated-event"></a>ILStubGenerated イベント

次の表に、キーワードとレベルを示します。 (詳細については、「 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)」を参照してください)。  
  
|イベントを発生させるキーワード|レベル|  
|-----------------------------------|-----------|  
|`InteropKeyword` (0x2000)|情報通知 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`ILStubGenerated`|88|MSIL スタブが生成された。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt16|モジュールの識別子。|  
|StubMethodID|win:UInt64|スタブのメソッド識別子。|  
|StubFlags|win:UInt64|スタブのフラグ:<br /><br /> 0x1 - 逆方向の相互運用。<br /><br /> 0x2 - COM 相互運用。<br /><br /> 0x4 - NGen.exe で生成されたスタブ。<br /><br /> 0x8 - デリゲート。<br /><br /> 0x10-可変個引数。<br /><br /> 0x20 - アンマネージ呼び出し先。|  
|ManagedInteropMethodToken|win:UInt32|マネージド相互運用メソッドのトークンです。|  
|ManagedInteropMethodNameSpace|win:UnicodeString|マネージド相互運用メソッドの名前空間。|  
|ManagedInteropMethodName|win:UnicodeString|マネージド相互運用メソッドの名前。|  
|ManagedInteropMethodSignature|win:UnicodeString|マネージド相互運用メソッドのシグネチャ。|  
|NativeMethodSignature|win:UnicodeString|ネイティブ メソッド シグネチャ。|  
|StubMethodSignature|win:UnicodeString|スタブ メソッド シグネチャ。|  
|StubMethodILCode|win:UnicodeString|スタブ メソッドの MSIL コード。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
## <a name="ilstubcachehit-event"></a>ILStubCacheHit イベント  

次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|レベル|  
|-----------------------------------|-----------|  
|`InteropKeyword` (0x2000)|情報通知 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`ILStubCacheHit`|89|MSIL のキャッシュにアクセスがあった。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt16|モジュールの識別子。|  
|StubMethodID|win:UInt64|スタブのメソッド識別子。|  
|ManagedInteropMethodToken|win:UInt32|マネージド相互運用メソッドのトークンです。|  
|ManagedInteropMethodNameSpace|win:UnicodeString|マネージド相互運用メソッドの名前空間。|  
|ManagedInteropMethodName|win:UnicodeString|マネージド相互運用メソッドの名前。|  
|ManagedInteropMethodSignature|win:UnicodeString|マネージド相互運用メソッドのシグネチャ。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
## <a name="see-also"></a>関連項目

- [CLR ETW イベント](clr-etw-events.md)
