---
title: 列挙体のプロファイリング
ms.date: 03/30/2017
helpviewer_keywords:
- profiling enumerations [.NET Framework]
- enumerations [.NET Framework profiling]
- unmanaged enumerations [.NET Framework], profiling
ms.assetid: 8d5f9570-9853-4ce8-8101-df235d5b258e
ms.openlocfilehash: 1a9781fa1b4b608152faa7d5edc80bd4866f0c81
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868136"
---
# <a name="profiling-enumerations"></a>列挙体のプロファイリング
このセクションでは、プロファイル API が使用するアンマネージ列挙について説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [COR_PRF_CLAUSE_TYPE 列挙型](cor-prf-clause-type-enumeration.md)  
 コードが入った、または出た例外句のタイプを示します。  
  
 [COR_PRF_CODEGEN_FLAGS 列挙型](cor-prf-codegen-flags-enumeration.md)  
 [ICorProfilerFunctionControl:: SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md)メソッドで設定できるコード生成フラグを定義します。  
  
 [COR_PRF_FINALIZER_FLAGS 列挙型](cor-prf-finalizer-flags-enumeration.md)  
 オブジェクトのファイナライザーを記述します。  
  
 [COR_PRF_GC_GENERATION 列挙型](cor-prf-gc-generation-enumeration.md)  
 ガベージ コレクションのジェネレーションを識別します。  
  
 [COR_PRF_GC_REASON 列挙型](cor-prf-gc-reason-enumeration.md)  
 ガベージ コレクションが発生している理由を示します。  
  
 [COR_PRF_GC_ROOT_FLAGS 列挙型](cor-prf-gc-root-flags-enumeration.md)  
 ガベージ コレクターのルートのプロパティを示します。  
  
 [COR_PRF_GC_ROOT_KIND 列挙型](cor-prf-gc-root-kind-enumeration.md)  
 [ICorProfilerCallback2:: RootReferences2](icorprofilercallback2-rootreferences2-method.md)コールバックによって公開されるガベージコレクターのルートの種類を示します。  
  
 [COR_PRF_HIGH_MONITOR 列挙型](cor-prf-high-monitor-enumeration.md)  
 プロファイラーが読み込み時に[ICorProfilerInfo5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)メソッドに対して指定できる、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)列挙に含まれるフラグだけでなく、フラグも提供します。  
  
 [COR_PRF_JIT_CACHE 列挙型](cor-prf-jit-cache-enumeration.md)  
 キャッシュされている関数検索の結果を示します。  
  
 [COR_PRF_MISC 列挙型](cor-prf-misc-enumeration.md)  
 特殊な識別子を指定する定数値を含めます。  
  
 [COR_PRF_MODULE_FLAGS 列挙型](cor-prf-module-flags-enumeration.md)  
 モジュールのプロパティを指定します。  
  
 [COR_PRF_MONITOR 列挙型](cor-prf-monitor-enumeration.md)  
 プロファイラーがサブスクライブしようとする動作、機能、またはイベントの指定で使用する値を含めます。  
  
 [COR_PRF_RUNTIME_TYPE 列挙型](cor-prf-runtime-type-enumeration.md)  
 共通言語ランタイムのバージョンを表す値を含めます。  
  
 [COR_PRF_SNAPSHOT_INFO 列挙型](cor-prf-snapshot-info-enumeration.md)  
 プロファイラーの `StackSnapshotCallback` 関数への各呼び出しにおいて、スタック スナップショットでどのくらいのデータが返されるかを指定します。  
  
 [COR_PRF_STATIC_TYPE 列挙型](cor-prf-static-type-enumeration.md)  
 フィールドが静的であるかどうかを示し、静的な場合は、フィールドに適用される静的なクオリティを示します。  
  
 [COR_PRF_SUSPEND_REASON 列挙型](cor-prf-suspend-reason-enumeration.md)  
 ランタイムが中断された理由を示します。  
  
 [COR_PRF_TRANSITION_REASON 列挙型](cor-prf-transition-reason-enumeration.md)  
 マネージド コードからアンマネージド コードへ、またはその逆の遷移の理由を示します。  
  
## <a name="related-sections"></a>関連セクション  
 [プロファイルの概要](profiling-overview.md)  
  
 [プロファイリングのインターフェイス](profiling-interfaces.md)  
  
 [グローバル静的関数のプロファイル](profiling-global-static-functions.md)  
  
 [構造体のプロファイリング](profiling-structures.md)
