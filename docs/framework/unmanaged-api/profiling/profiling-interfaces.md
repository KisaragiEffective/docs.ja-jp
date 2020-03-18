---
title: プロファイリングのインターフェイス
ms.date: 04/10/2018
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], profiling
- profiling interfaces [.NET Framework]
- interfaces [.NET Framework profiling]
ms.assetid: d9303db8-e881-4217-91b7-8c7573c8ef9e
ms.openlocfilehash: 8b6b9acff2945e2d8fd684bfa31e4af086ea5ab9
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868149"
---
# <a name="profiling-interfaces"></a>プロファイリングのインターフェイス
ここでは、共通言語ランタイム (CLR) で実行中のプログラムに対してプロファイルを可能にするアンマネージ インターフェイスについて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [ICLRProfiling インターフェイス](iclrprofiling-interface.md)  
 [Attachprofiler](iclrprofiling-attachprofiler-method.md)メソッドを提供します。これにより、実行中のプロセスにプロファイラーをアタッチできます。  
  
 [ICorProfilerAssemblyReferenceProvider インターフェイス](icorprofilerassemblyreferenceprovider-interface.md)  
 プロファイラーが[ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)コールバックに追加するアセンブリ参照を CLR に通知できるようにします。  
  
 [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)  
 プロファイラーがサブスクライブしたイベントが発生したときにコード プロファイラーに通知するために、CLR が使用するメソッドを提供します。  
  
 [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)  
 .NET Framework 2.0 以降でサポートされるコールバックによって、`ICorProfilerCallback` インターフェイスを拡張します。  
  
 [ICorProfilerCallback3 インターフェイス](icorprofilercallback3-interface.md)  
 CLR がプロファイラーにアタッチとデタッチの状態情報を伝えるために使用するコールバック メソッドを提供します。  
  
 [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)  
 プロファイラーと情報をやりとりするために CLR が使用するコールバック メソッドを提供します。  
  
 [ICorProfilerCallback5 インターフェイス](icorprofilercallback5-interface.md)  
 ガベージ コレクションのルートによって参照されるオブジェクトの遷移的なクロージャを識別するメソッドを提供します。  
  
 [ICorProfilerCallback6 インターフェイス](icorprofilercallback6-interface.md)  
 CLR が プロファイラーに対して、アセンブリがロード中であることを通知するために使用するコールバック メソッドを提供します。  
  
 [ICorProfilerCallback7 インターフェイス](icorprofilercallback7-interface.md)  
 メモリ内モジュールに関連付けられているシンボルストリームが更新されたことをプロファイラーに通知するために共通言語ランタイムが使用するコールバックメソッドを提供します。  

[ICorProfilerCallback8 インターフェイス](icorprofilercallback8-interface.md)  
動的メソッドの JIT コンパイルが開始および終了したことをプロファイラーに通知するために共通言語ランタイムが使用するコールバックメソッドを提供します。

[ICorProfilerCallback9 インターフェイス](icorprofilercallback9-interface.md)  
動的メソッドがガベージコレクションされ、その後アンロードされることをプロファイラーに通知するために共通言語ランタイムが使用するコールバックメソッドを提供します。

 [ICorProfilerFunctionControl インターフェイス](icorprofilerfunctioncontrol-interface.md)  
 コード プロファイラーが CLR と通信できるようにするためのメソッドを提供します。これは特定のメソッドを再コンパイルするときに、JIT コンパイラーがどのようにしてコードを生成するかを制御するためのものです。  
  
 [ICorProfilerFunctionEnum インターフェイス](icorprofilerfunctionenum-interface.md)  
 CLR で関数のコレクションを順番に反復処理するためのメソッドを提供します。  
  
 [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)  
 コード プロファイラーが、イベントの監視および情報の要求を制御するために CLR との通信で使用するメソッドを提供します。  
  
 [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)  
 .NET Framework 2.0 以降でサポートされるメソッドによって、`ICorProfilerInfo` インターフェイスを拡張します。  
  
 [ICorProfilerInfo3 インターフェイス](icorprofilerinfo3-interface.md)  
 .NET Framework 4 以降のバージョンでサポートされているメソッドを使用して、`ICorProfilerInfo2` インターフェイスを拡張します。  
  
 [ICorProfilerInfo4 インターフェイス](icorprofilerinfo4-interface.md)  
 コード プロファイラーが、イベントの監視および情報の要求を制御するために CLR との通信で使用するメソッドを提供します。  
  
 [ICorProfilerInfo5 インターフェイス](icorprofilerinfo5-interface.md)  
 コード プロファイラーが、イベントの監視を制御するために CLR との通信で使用するメソッドを提供します。  
  
 [ICorProfilerInfo6 インターフェイス](icorprofilerinfo6-interface.md)  
 特定の NGen モジュールに属し、特定のメソッドの本体にインライン化されているすべてのメソッドに列挙子を提供します。  
  
 [ICorProfilerInfo7 インターフェイス](icorprofilerinfo7-interface.md)  
 新しく定義されたメタデータをモジュールに適用し、メモリ内シンボルストリームへのアクセスを提供するメソッドを提供します。  
  
 [ICorProfilerModuleEnum インターフェイス](icorprofilermoduleenum-interface.md)  
 アプリケーションまたはプロファイラーによってロードされたモジュールのコレクションを順番に反復処理するためのメソッドを提供します。  
  
 [ICorProfilerObjectEnum インターフェイス](icorprofilerobjectenum-interface.md)  
 [Ngen.exe (ネイティブイメージジェネレーター)](../../../../docs/framework/tools/ngen-exe-native-image-generator.md)によって生成される固定されたオブジェクトのコレクションを順番に反復処理するメソッドを提供します。  
  
 [ICorProfilerThreadEnum インターフェイス](icorprofilerthreadenum-interface.md)  
 CLR でスレッドのコレクションを順番に反復処理するためのメソッドを提供します。  
  
 [IMethodMalloc インターフェイス](imethodmalloc-interface.md)  
 新しい Microsoft 中間言語 (MSIL) 関数の本体にメモリを割り当てるための[Alloc](imethodmalloc-alloc-method.md)メソッドを提供します。  
  
## <a name="related-sections"></a>関連セクション  
 [プロファイルの概要](profiling-overview.md)  
  
 [グローバル静的関数のプロファイル](profiling-global-static-functions.md)  
  
 [列挙型のプロファイリング](profiling-enumerations.md)  
  
 [構造体のプロファイリング](profiling-structures.md)
