---
title: デバッグ構造体
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged structures [.NET Framework], debugging
- debugging structures [.NET Framework]
- structures [.NET Framework debugging]
ms.assetid: 173ba2c2-ab34-49ae-b6a8-e5c49882bf05
ms.openlocfilehash: a18094fb2621478dbdb4bbf672df436234112ed0
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793750"
---
# <a name="debugging-structures"></a>デバッグ構造体

このセクションでは、デバッグ API が使用するアンマネージ構造体について説明します。

## <a name="in-this-section"></a>このセクションの内容
 [CLRDATA_ADDRESS_RANGE 構造体](clrdata-address-range-structure.md)アドレス範囲を定義します。

 [CLRDATA_IL_ADDRESS_MAP 構造体](clrdata-il-address-map-structure.md)IL とアドレスのマッピングを定義します。

 [CLR_DEBUGGING_VERSION 構造体](clr-debugging-version-structure.md)デバッグのために共通言語ランタイム (CLR) の製品バージョンを定義します。

 [CodeChunkInfo 構造体](codechunkinfo-structure.md)メモリ内の1つのコードチャンクを表します。

 [COR_ACTIVE_FUNCTION](cor-active-function-structure.md)スレッドのフレームで現在アクティブになっている関数に関する情報を格納します。

 [COR_ARRAY_LAYOUT 構造体](cor-array-layout-structure.md)メモリ内の配列オブジェクトのレイアウトに関する情報を提供します。

 [COR_DEBUG_IL_TO_NATIVE_MAP](cor-debug-il-to-native-map-structure.md)Microsoft 中間言語 (MSIL) コードをネイティブコードにマップするために使用されるオフセットを格納します。

 [COR_DEBUG_STEP_RANGE](cor-debug-step-range-structure.md)コード範囲のオフセット情報を格納します。

 [COR_FIELD 構造体](cor-field-structure.md)オブジェクトのフィールドに関する情報を提供します。

 [COR_GC_REFERENCE 構造体](cor-gc-reference-structure.md)ガベージコレクトされるオブジェクトに関する情報を格納します。

 [COR_HEAPINFO 構造体](cor-heapinfo-structure.md)列挙可能かどうかなど、ガベージコレクションヒープに関する一般的な情報を提供します。

 [COR_HEAPOBJECT 構造体](cor-heapobject-structure.md)マネージヒープ上のオブジェクトに関する情報を提供します。

 [COR_IL_MAP](cor-il-map-structure.md)関数の相対オフセットの変更を指定します。

 [COR_SEGMENT 構造体](cor-segment-structure.md)マネージヒープのメモリ領域に関する情報を格納します。

 [COR_TYPEID 構造体](cor-typeid-structure.md)型識別子を格納します。

 [COR_TYPE_LAYOUT 構造体](cor-type-layout-structure.md)メモリ内のオブジェクトのレイアウトに関する情報を提供します。

 [COR_VERSION](cor-version-structure.md)共通言語ランタイムの標準の4部構成のバージョン番号を格納します。

 [CorDebugBlockingObject 構造体](cordebugblockingobject-structure.md)スレッドをブロックするオブジェクトと、スレッドがブロックされる理由を定義します。

 [CorDebugEHClause 構造体](cordebugehclause-structure.md)指定された中間言語 (IL) の例外処理 (EH) 句を表します。

 [CorDebugExceptionObjectStackFrame 構造体](cordebugexceptionobjectstackframe-structure.md)例外オブジェクトからのスタックフレーム情報を表します。

 [Cordebugguidtotypemapping 構造体](cordebugguidtotypemapping-structure.md)Windows ランタイム GUID を対応する[テキスト](icordebugtype-interface.md)オブジェクトにマップします。

 [DacpGetModuleAddress 構造体](dacpgetmoduleaddress-structure.md)モジュールアドレス要求のコンテナーを定義します。

 [DacpMethodDescData 構造体](dacpmethoddescdata-structure.md)メソッドのランタイム情報のトランスポートバッファーを定義します。

 [Dacpmoduledata 構造体](dacpmoduledata-structure.md)モジュールのランタイム情報のトランスポートバッファーを定義します。

 [DacpReJitData 構造体](dacprejitdata-structure.md)プロファイラーによってインストルメント化された特定のメソッドに関する基本情報を定義します。

 [StackTrace_SimpleContext 構造体](stacktrace-simplecontext-structure.md)完全な `CONTEXT` 構造の代わりに使用できる単純なコンテキストを提供します。

## <a name="related-sections"></a>関連セクション

 [デバッグ コクラス](debugging-coclasses.md)

 [デバッグ インターフェイス](debugging-interfaces.md)

 [デバッグ グローバル静的関数](debugging-global-static-functions.md)

 [列挙型のデバッグ](debugging-enumerations.md)

 [デバッグ](index.md)
