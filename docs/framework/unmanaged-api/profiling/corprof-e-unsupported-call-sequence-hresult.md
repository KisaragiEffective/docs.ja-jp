---
title: CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT
ms.date: 03/30/2017
f1_keywords:
- CORPROF_E_UNSUPPORTED_CALL_SEQUENCE
helpviewer_keywords:
- CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT [.NET Framework profiling]
ms.assetid: f2fc441f-d62e-4f72-a011-354ea13c8c59
ms.openlocfilehash: b4ab5c8f7cdca1303cb4fbbc4fa39db3c5977c15
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867010"
---
# <a name="corprof_e_unsupported_call_sequence-hresult"></a>CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT

CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT は .NET Framework バージョン2.0 で導入されました。 .NET Framework 4 は、次の2つのシナリオでこの HRESULT を返します。  
  
- ハイジャックプロファイラーが任意のタイミングでスレッドのレジスタコンテキストを強制的にリセットすると、スレッドは不整合な状態にある構造体にアクセスしようとします。  
  
- プロファイラーが、ガベージコレクションを禁止するコールバックメソッドからガベージコレクションをトリガーする情報メソッドを呼び出そうとしたとき。  
  
これらの2つのシナリオについては、次のセクションで説明します。  
  
## <a name="hijacking-profilers"></a>プロファイラーのハイジャック  

  (このシナリオは、主にハイジャックプロファイラーの問題ですが、ハイジャックされていないプロファイラーでこの HRESULT が表示される場合もあります)。  
  
 このシナリオでは、ハイジャックプロファイラーは、スレッドがプロファイラーコードを入力したり、 [ICorProfilerInfo](icorprofilerinfo-interface.md)メソッドを使用して共通言語ランタイム (CLR) を再入力したりできるように、任意のタイミングでスレッドのレジスタコンテキストを強制的にリセットします。  
  
 プロファイリング API が CLR のデータ構造をポイントするために使用する Id の多く。 多くの `ICorProfilerInfo` 呼び出しでは、これらのデータ構造から情報を読み取って戻すだけです。 ただし、CLR では、実行時にこれらの構造内の項目が変更される可能性があり、ロックを使用することもあります。 プロファイラーがスレッドをハイジャックしたときに、CLR がロックを既に保持している (または取得しようとしている) とします。 スレッドが CLR に再入力され、さらに多くのロックを取得しようとした場合、または変更処理中の構造を検査する場合、これらの構造は不整合な状態になる可能性があります。 このような状況では、デッドロックとアクセス違反が簡単に発生します。  
  
 一般に、ハイジャックされていないプロファイラーが[ICorProfilerCallback](icorprofilercallback-interface.md)メソッド内でコードを実行し、有効なパラメーターを使用して `ICorProfilerInfo` メソッドを呼び出すと、デッドロックが発生したりアクセス違反が発生したりすることはありません。 たとえば、 [ICorProfilerCallback:: ClassLoadFinished](icorprofilercallback-classloadfinished-method.md)メソッド内で実行されるプロファイラーコードは、 [ICorProfilerInfo2:: GetClassIDInfo2](icorprofilerinfo2-getclassidinfo2-method.md)メソッドを呼び出すことによって、クラスに関する情報を要求する場合があります。 このコードでは、情報が使用できないことを示す CORPROF_E_DATAINCOMPLETE HRESULT が返される場合があります。 ただし、デッドロックが発生したり、アクセス違反が発生したりすることはありません。 これらの呼び出しは、`ICorProfilerCallback` メソッドから作成されるため、`ICorProfilerInfo` の呼び出しは同期と見なされます。  
  
 ただし、`ICorProfilerCallback` メソッド内に含まれていないコードを実行するマネージスレッドは、非同期呼び出しを行うと見なされます。 .NET Framework バージョン1では、非同期呼び出しで発生する可能性があることを判断するのは困難でした。 この呼び出しは、デッドロック、クラッシュ、または無効な回答を与える可能性があります。 .NET Framework バージョン2.0 では、この問題を回避するための簡単なチェックがいくつか導入されました。 .NET Framework 2.0 では、unsafe `ICorProfilerInfo` 関数を非同期的に呼び出すと、CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT で失敗します。  
  
 一般に、非同期呼び出しは安全ではありません。 ただし、次のメソッドは安全であり、特に非同期呼び出しをサポートしています。  
  
- [GetEventMask](icorprofilerinfo-geteventmask-method.md)  
  
- [SetEventMask](icorprofilerinfo-seteventmask-method.md)  
  
- [GetCurrentThreadID](icorprofilerinfo-getcurrentthreadid-method.md)  
  
- [GetThreadContext](icorprofilerinfo-getthreadcontext-method.md)  
  
- [GetThreadAppDomain](icorprofilerinfo2-getthreadappdomain-method.md)  
  
- [GetFunctionFromIP](icorprofilerinfo-getfunctionfromip-method.md)  
  
- [GetFunctionInfo](icorprofilerinfo-getfunctioninfo-method.md)  
  
- [GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md)  
  
- [GetCodeInfo](icorprofilerinfo-getcodeinfo-method.md)  
  
- [GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md)  
  
- [GetModuleInfo](icorprofilerinfo-getmoduleinfo-method.md)  
  
- [GetClassIDInfo](icorprofilerinfo-getclassidinfo-method.md)  
  
- [GetClassIDInfo2](icorprofilerinfo2-getclassidinfo2-method.md)  
  
- [IsArrayClass](icorprofilerinfo-isarrayclass-method.md)  
  
- [SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)  
  
- [DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)  
  
 詳細については、CLR プロファイル API のブログで[CORPROF_E_UNSUPPORTED_CALL_SEQUENCE した理由](https://docs.microsoft.com/archive/blogs/davbr/why-we-have-corprof_e_unsupported_call_sequence)を参照してください。  
  
## <a name="triggering-garbage-collections"></a>ガベージコレクションのトリガー  
 このシナリオでは、ガベージコレクションを禁止するコールバックメソッド (たとえば、`ICorProfilerCallback` メソッドの1つ) 内で実行されているプロファイラーを使用します。 プロファイラーが、ガベージコレクションをトリガーする可能性のある情報メソッド (`ICorProfilerInfo` インターフェイスのメソッドなど) を呼び出そうとすると、情報メソッドが CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT で失敗します。  
  
 次の表は、ガベージコレクションを禁止するコールバックメソッドと、ガベージコレクションをトリガーする可能性のある情報メソッドを示しています。 プロファイラーが、一覧表示されているいずれかのコールバックメソッド内で実行され、一覧表示されている情報メソッドの1つを呼び出すと、その情報メソッドが CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT で失敗します。  
  
|ガベージコレクションを禁止するコールバックメソッド|ガベージコレクションをトリガーする情報メソッド|  
|------------------------------------------------------|------------------------------------------------------------|  
|[ThreadAssignedToOSThread](icorprofilercallback-threadassignedtoosthread-method.md)<br /><br /> [ExceptionUnwindFunctionEnter](icorprofilercallback-exceptionunwindfunctionenter-method.md)<br /><br /> [ExceptionUnwindFunctionLeave](icorprofilercallback-exceptionunwindfunctionleave-method.md)<br /><br /> [ExceptionUnwindFinallyEnter](icorprofilercallback-exceptionunwindfinallyenter-method.md)<br /><br /> [ExceptionUnwindFinallyLeave](icorprofilercallback-exceptionunwindfinallyleave-method.md)<br /><br /> [ExceptionCatcherEnter](icorprofilercallback-exceptioncatcherenter-method.md)<br /><br /> [RuntimeSuspendStarted](icorprofilercallback-runtimesuspendstarted-method.md)<br /><br /> [RuntimeSuspendFinished](icorprofilercallback-runtimesuspendfinished-method.md)<br /><br /> [RuntimeSuspendAborted](icorprofilercallback-runtimesuspendaborted-method.md)<br /><br /> [RuntimeThreadSuspended](icorprofilercallback-runtimethreadsuspended-method.md)<br /><br /> [RuntimeThreadResumed](icorprofilercallback-runtimethreadresumed-method.md)<br /><br /> [MovedReferences](icorprofilercallback-movedreferences-method.md)<br /><br /> [ObjectReferences](icorprofilercallback-objectreferences-method.md)<br /><br /> [ObjectsAllocatedByClass](icorprofilercallback-objectsallocatedbyclass-method.md)<br /><br /> [RootReferences2](icorprofilercallback-rootreferences-method.md)<br /><br /> [HandleCreated](icorprofilercallback2-handlecreated-method.md)<br /><br /> [HandleDestroyed](icorprofilercallback2-handledestroyed-method.md)<br /><br /> [GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)<br /><br /> [GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)|[GetILFunctionBodyAllocator](icorprofilerinfo-getilfunctionbodyallocator-method.md)<br /><br /> [SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)<br /><br /> [SetILInstrumentedCodeMap](icorprofilerinfo-setilinstrumentedcodemap-method.md)<br /><br /> [ForceGC](icorprofilerinfo-forcegc-method.md)<br /><br /> [GetClassFromToken](icorprofilerinfo-getclassfromtoken-method.md)<br /><br /> [GetClassFromTokenAndTypeArgs](icorprofilerinfo2-getclassfromtokenandtypeargs-method.md)<br /><br /> [GetFunctionFromTokenAndTypeArgs](icorprofilerinfo2-getfunctionfromtokenandtypeargs-method.md)<br /><br /> [GetAppDomainInfo](icorprofilerinfo-getappdomaininfo-method.md)<br /><br /> [EnumModules](icorprofilerinfo3-enummodules-method.md)<br /><br /> [RequestProfilerDetach](icorprofilerinfo3-requestprofilerdetach-method.md)<br /><br /> [GetAppDomainsContainingModule](icorprofilerinfo3-getappdomainscontainingmodule-method.md)|  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)
- [ICorProfilerCallback3 インターフェイス](icorprofilercallback3-interface.md)
- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
- [ICorProfilerInfo3 インターフェイス](icorprofilerinfo3-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
