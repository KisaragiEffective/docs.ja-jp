---
title: ICorProfilerInfo インターフェイス
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo
helpviewer_keywords:
- ICorProfilerInfo interface [.NET Framework profiling]
ms.assetid: eb4e4ce0-06e7-4469-bbc4-edc2eb5da4b1
topic_type:
- apiref
ms.openlocfilehash: b8cba2b1a1f206392a59f8bc9b968e725e0ce6ee
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869311"
---
# <a name="icorprofilerinfo-interface"></a>ICorProfilerInfo インターフェイス
コードプロファイラーが共通言語ランタイム (CLR) と通信してイベントの監視および要求情報を制御するために使用するメソッドを提供します。  
  
> [!NOTE]
> `ICorProfilerInfo` インターフェイスの各メソッドは、成功または失敗を示す HRESULT を返します。 使用できるリターンコードの一覧については、「CorError. h」を参照してください。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[BeginInprocDebugging メソッド](icorprofilerinfo-begininprocdebugging-method.md)|インプロセスデバッグサポートを初期化します。 このメソッドは .NET Framework バージョン2.0 では廃止されています。|  
|[EndInprocDebugging メソッド](icorprofilerinfo-endinprocdebugging-method.md)|インプロセスデバッグセッションをシャットダウンします。 このメソッドは .NET Framework バージョン2.0 では廃止されています。|  
|[ForceGC メソッド](icorprofilerinfo-forcegc-method.md)|ランタイム内で強制的にガベージコレクションを実行します。|  
|[GetAppDomainInfo メソッド](icorprofilerinfo-getappdomaininfo-method.md)|指定されたアプリケーションドメインに関する情報を取得します。|  
|[GetAssemblyInfo メソッド](icorprofilerinfo-getassemblyinfo-method.md)|指定したアセンブリに関する情報を取得します。|  
|[GetClassFromObject メソッド](icorprofilerinfo-getclassfromobject-method.md)|の `ClassID` を取得します。<br /><br /> オブジェクト (`ObjectID`を指定)。|  
|[GetClassFromToken メソッド](icorprofilerinfo-getclassfromtoken-method.md)|メタデータトークンを指定して、クラスの ID を取得します。 このメソッドは .NET Framework バージョン2.0 では廃止されています。 代わりに[ICorProfilerInfo2:: GetClassFromTokenAndTypeArgs](icorprofilerinfo2-getclassfromtokenandtypeargs-method.md)メソッドを使用してください。|  
|[GetClassIDInfo メソッド](icorprofilerinfo-getclassidinfo-method.md)|指定したクラスの親モジュールとメタデータトークンを取得します。|  
|[GetCodeInfo メソッド](icorprofilerinfo-getcodeinfo-method.md)|指定した関数 ID に関連付けられているネイティブ コードの範囲を取得します。 このメソッドは、互換性のために残されています。 代わりに[ICorProfilerInfo2:: GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md)メソッドを使用してください。|  
|[GetCurrentThreadID メソッド](icorprofilerinfo-getcurrentthreadid-method.md)|マネージスレッドの場合、現在のスレッドの ID を取得します。|  
|[GetEventMask メソッド](icorprofilerinfo-geteventmask-method.md)|プロファイラーが CLR からイベント通知を受信する現在のイベントカテゴリを取得します。|  
|[GetFunctionFromIP メソッド](icorprofilerinfo-getfunctionfromip-method.md)|マネージコード命令ポインターを `FunctionID`にマップします。|  
|[GetFunctionFromToken メソッド](icorprofilerinfo-getfunctionfromtoken-method.md)|関数の ID を取得します。 このメソッドは .NET Framework バージョン2.0 では廃止されています。 代わりに[ICorProfilerInfo2:: GetFunctionFromTokenAndTypeArgs](icorprofilerinfo2-getfunctionfromtokenandtypeargs-method.md)メソッドを使用してください。|  
|[GetFunctionInfo メソッド](icorprofilerinfo-getfunctioninfo-method.md)|指定された関数の親クラスとメタデータトークンを取得します。|  
|[GetHandleFromThread メソッド](icorprofilerinfo-gethandlefromthread-method.md)|スレッドの ID を Win32 スレッドハンドルにマップします。|  
|[GetILFunctionBody メソッド](icorprofilerinfo-getilfunctionbody-method.md)|Microsoft 中間言語 (MSIL) コード内のメソッドの本文へのポインターを、ヘッダーを開始位置として取得します。|  
|[GetILFunctionBodyAllocator メソッド](icorprofilerinfo-getilfunctionbodyallocator-method.md)|MSIL コードでメソッドの本体を交換するために使用されるメモリを割り当てるメソッドを提供するインターフェイスを取得します。|  
|[GetILToNativeMapping メソッド](icorprofilerinfo-getiltonativemapping-method.md)|指定された関数に含まれるコードの MSIL オフセットからネイティブオフセットへのマップを取得します。|  
|[GetInprocInspectionInterface メソッド](icorprofilerinfo-getinprocinspectioninterface-method.md)|は、のプロセスインターフェイスに対してクエリを実行できるオブジェクトを取得します。 このメソッドは .NET Framework バージョン2.0 では廃止されています。|  
|[GetInprocInspectionIThisThread メソッド](icorprofilerinfo-getinprocinspectionithisthread-method.md)|のオブジェクトインターフェイスに対してクエリを実行できるオブジェクトを取得します。 このメソッドは .NET Framework バージョン2.0 では廃止されています。|  
|[GetModuleInfo メソッド](icorprofilerinfo-getmoduleinfo-method.md)|モジュール ID を指定して、モジュールのファイル名とモジュールの親アセンブリの ID を取得します。|  
|[GetModuleMetaData メソッド](icorprofilerinfo-getmodulemetadata-method.md)|指定したモジュールにマップされるメタデータインターフェイスインスタンスを取得します。|  
|[GetObjectSize メソッド](icorprofilerinfo-getobjectsize-method.md)|指定したオブジェクトのサイズを取得します。|  
|[GetThreadContext メソッド](icorprofilerinfo-getthreadcontext-method.md)|指定したスレッドに現在関連付けられているコンテキスト id を取得します。|  
|[GetThreadInfo メソッド](icorprofilerinfo-getthreadinfo-method.md)|指定したスレッドの現在の Win32 スレッド id を取得します。|  
|[GetTokenAndMetadataFromFunction メソッド](icorprofilerinfo-gettokenandmetadatafromfunction-method.md)|メタデータトークンと、指定された関数のトークンに対して使用できるメタデータインターフェイスのインスタンスを取得します。|  
|[IsArrayClass メソッド](icorprofilerinfo-isarrayclass-method.md)|指定したクラスが配列クラスであるかどうかを判断します。|  
|[SetEnterLeaveFunctionHooks メソッド](icorprofilerinfo-setenterleavefunctionhooks-method.md)|マネージ関数の "enter"、"leave"、および "tailcall" フックで呼び出されるプロファイラー実装関数を指定します。|  
|[SetEventMask メソッド](icorprofilerinfo-seteventmask-method.md)|プロファイラーが CLR からの通知の受信を希望するイベントの種類を指定する値を設定します。|  
|[SetFunctionIDMapper メソッド](icorprofilerinfo-setfunctionidmapper-method.md)|`FunctionID` 値を代替値に対応付けるために呼び出すプロファイラー実装関数を指定します。代替値は、プロファイラーの関数の開始フックと終了フックに渡されます。|  
|[SetFunctionReJIT メソッド](icorprofilerinfo-setfunctionrejit-method.md)|実装されていません。 [使用しない]。|  
|[SetILFunctionBody メソッド](icorprofilerinfo-setilfunctionbody-method.md)|指定したモジュール内の指定した関数の本体を置き換えます。|  
|[SetILInstrumentedCodeMap メソッド](icorprofilerinfo-setilinstrumentedcodemap-method.md)|指定された関数の元の MSIL のオフセットが、関数のプロファイラーによって変更された MSIL の新しいオフセットにどのようにマップされるかを指定します。|  
  
## <a name="remarks"></a>コメント  
 プロファイラーは、CLR と通信してイベントの監視と要求情報を制御するために、`ICorProfilerInfo` インターフェイス内のメソッドを呼び出します。  
  
 `ICorProfilerInfo` インターフェイスのメソッドは、フリースレッドモデルを使用して CLR によって実装されます。 各メソッドが、成功または失敗を示す HRESULT を返します。 使用できるリターンコードの一覧については、「CorError. h」を参照してください。  
  
 CLR は、 [ICorProfilerCallback:: Initialize](icorprofilercallback-initialize-method.md)のプロファイラーの実装を介して、初期化中に各コードプロファイラーへの `ICorProfilerInfo` インターフェイスを渡します。 コードプロファイラーは、`ICorProfilerInfo` インターフェイスのメソッドを呼び出して、CLR の制御下で実行されているマネージコードに関する情報を取得できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
