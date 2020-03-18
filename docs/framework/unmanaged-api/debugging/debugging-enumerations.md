---
title: 列挙体のデバッグ
ms.date: 03/30/2017
helpviewer_keywords:
- debugging enumerations [.NET Framework]
- unmanaged enumerations [.NET Framework], debugging
- enumerations [.NET Framework debugging]
ms.assetid: 3af9f584-f1b4-4154-aeaa-8fce7c9f8b50
ms.openlocfilehash: a83b1aa0b2cc068ed2f73dca04083b1085d45201
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789167"
---
# <a name="debugging-enumerations"></a>列挙体のデバッグ
このセクションでは、デバッグ API が使用するアンマネージ列挙体について説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [CLR_DEBUGGING_PROCESS_FLAGS 列挙型](clr-debugging-process-flags-enumeration.md)  
 [ICLRDebugging:: OpenVirtualProcess](iclrdebugging-openvirtualprocess-method.md)メソッドによって使用される値を提供します。  
  
 [CLRDataEnumMemoryFlags 列挙型](clrdataenummemoryflags-enumeration.md)  
 [ICLRDataEnumMemoryRegions:: EnumMemoryRegions](iclrdataenummemoryregions-enummemoryregions-method.md)メソッドへの呼び出しに含まれるメモリ領域を示します。  
  
 [COR_PUB_ENUMPROCESS 列挙型](cor-pub-enumprocess-enumeration.md)  
 列挙するプロセスの型を識別します。  
  
 [CorDebugBlockingReason 列挙型](cordebugblockingreason-enumeration.md)  
 指定されたオブジェクト上でスレッドがブロックされる理由を指定します。  
  
 CorDebugChainReason  
 呼び出しチェーンが開始する理由を示します。  
  
 [CorDebugCodeInvokeKind 列挙型](cordebugcodeinvokekind-enumeration.md)  
 エクスポートされた関数がマネージド コードを呼び出す方法を示します。  
  
 [CorDebugCodeInvokePurpose 列挙型](cordebugcodeinvokepurpose-enumeration.md)  
 エクスポートされた関数がマネージド コードを呼び出す理由を示します。  
  
 CorDebugCreateProcessFlags  
 [ICorDebug:: CreateProcess](icordebug-createprocess-method.md)メソッドの呼び出しで使用できる追加のデバッグオプションを提供します。  
  
 [CorDebugDebugEventKind 列挙型](cordebugdebugeventkind-enumeration.md)  
 [DecodeEvent](icordebugprocess6-decodeevent-method.md)メソッドによって情報がデコードされるイベントの種類を示します。  
  
 [CorDebugDecodeEventFlagsWindows 列挙型](cordebugdecodeeventflagswindows-enumeration.md)  
 Windows プラットフォームのデバッグ イベントに関する追加情報を提供します。  
  
 CorDebugExceptionCallbackType  
 [ICorDebugManagedCallback2:: Exception](icordebugmanagedcallback2-exception-method.md)イベントから作成されたコールバックの種類を示します。  
  
 [CorDebugExceptionFlags 列挙型](cordebugexceptionflags-enumeration.md)  
 例外に関する追加情報を提供します。  
  
 CorDebugExceptionUnwindCallbackType  
 アンワインド フェーズ中にコールバックによって通知されるイベントを示します。  
  
 [CorDebugGCType 列挙型](cordebuggctype-enumeration.md)  
 ガベージ コレクターがワークステーションまたはサーバーのどちらで実行されているかを示します。  
  
 [CorDebugGenerationTypes 列挙型](cordebuggenerationtypes-enumeration.md)  
 マネージド ヒープ上のメモリ領域の生成を指定します。  
  
 CorDebugHandleType  
 ハンドル型を示します。  
  
 [CorDebugIlToNativeMappingTypes 列挙型](cordebugiltonativemappingtypes-enumeration.md)  
 ネイティブ命令の特定の範囲が特別なコード領域に対応するかどうかを示します。  
  
 CorDebugIntercept  
 ステップ インできるコードの型を示します。  
  
 [CorDebugInterfaceVersion 列挙型](cordebuginterfaceversion-enumeration.md)  
 .NET Framework のバージョン、またはインターフェイスが導入された .NET Framework のバージョンのいずれかを指定します。  
  
 CorDebugInternalFrameType  
 スタック フレームの型を示します。  
  
 [CorDebugJITCompilerFlags 列挙型](cordebugjitcompilerflags-enumeration.md)  
 マネージド Just-In-Time (JIT) コンパイラの動作に影響を与える値が含まれます。  
  
 [CorDebugJITCompilerFlagsDeprecated 列挙型](cordebugjitcompilerflagsdeprecated-enumeration.md)  
 互換性のために残されています。 代わりに、 [CorDebugJITCompilerFlags](cordebugjitcompilerflags-enumeration.md)列挙体の `CORDEBUG_JIT_DEFAULT` メンバーを使用してください。  
  
 CorDebugMappingResult  
 命令ポインター (IP) の値が得られた方法の詳細を提供します。  
  
 [CorDebugMDAFlags 列挙型](cordebugmdaflags-enumeration.md)  
 マネージド デバッグ アシスタント (MDA) が生成されるスレッドのステータスを指定します。  
  
 [CorDebugNGenPolicy 列挙型](cordebugngenpolicy-enumeration.md)  
 デバッガーがネイティブ イメージ キャッシュからネイティブ (NGen) イメージを読み込むかどうかを指定する値を提供します。  
  
 [CorDebugPlatform 列挙型](cordebugplatform-enumeration.md)  
 は[、のターゲットプラットフォームの値](icordebugdatatarget-getplatform-method.md)を提供しています。  
  
 [CorDebugRecordFormat 列挙型](cordebugrecordformat-enumeration.md)  
 ネイティブ例外デバッグ イベントに関する情報を格納するバイト配列内のデータの形式を示します。  
  
 CorDebugRegister  
 指定されたプロセッサ アーキテクチャに関連付けられたレジスタを指定します。  
  
 [CorDebugSetContextFlag 列挙型](cordebugsetcontextflag-enumeration.md)  
 スタック上のアクティブ (またはリーフ) フレーム上からのコンテキストなのか、別のフレームからのアンワインドにより計算されたコンテキストなのかを示します。  
  
 [CorDebugStateChange 列挙型](cordebugstatechange-enumeration.md)  
 プロセスへの変更に基づいて破棄が必要となった、キャッシュされたデータの量を示します。  
  
 CorDebugStepReason  
 個々のステップの結果を示します。  
  
 CorDebugThreadState  
 デバッグのスレッドの状態を指定します。  
  
 \>CorDebugUnmappedStop  
 ステッパによるコード実行の停止をトリガーする可能性のあるマップ解除したコードの型を指定します。  
  
 CorDebugUserState  
 スレッドのユーザーの状態を示します。  
  
 [CorGCReferenceType 列挙型](corgcreferencetype-enumeration.md)  
 ガベージ コレクトされる必要のあるオブジェクトのソースを識別します。  
  
 [ILCodeKind 列挙型](ilcodekind-enumeration.md)  
 デバッガーがローカル変数またはプロファイラー ReJIT インストルメンテーションに追加されたコードにアクセスできるかどうかを指定する値を提供します。  
  
 [LoggingLevelEnum 列挙型](logginglevelenum-enumeration.md)  
 マネージド スレッドがイベントを記録する際にイベント ログに書き込まれる説明メッセージの重大度レベルを示します。  
  
 [LogSwitchCallReason 列挙型](logswitchcallreason-enumeration.md)  
 デバッグとトレースの切り替えで実行された操作を示します。  
  
 [VariableLocationType 列挙型](variablelocationtype-enumeration.md)  
 変数のネイティブな場所の種類を示します。  
  
 [WriteableMetadataUpdateMode 列挙型](writeablemetadataupdatemode-enumeration.md)  
 メモリ内のメタデータ更新をデバッガーに対して可視にするかどうかを指定する値を提供します。 

 [ClrDataSourceType 列挙型](clrdatasourcetype-enumeration.md)CLRDATA_IL_ADDRESS_MAP 構造体によって使用される値を提供します。

## <a name="related-sections"></a>関連セクション  
 [デバッグ コクラス](debugging-coclasses.md)  
  
 [デバッグ インターフェイス](debugging-interfaces.md)  
  
 [デバッグ グローバル静的関数](debugging-global-static-functions.md)  
  
 [デバッグ構造体](debugging-structures.md)
