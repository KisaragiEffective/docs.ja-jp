---
title: グローバル静的関数のプロファイル
ms.date: 03/30/2017
helpviewer_keywords:
- global static functions [.NET Framework profiling]
- profiling global static functions [.NET Framework]
- unmanaged global static functions [.NET Framework], profiling
ms.assetid: 08a13a57-dc49-488d-b937-31e3051fda97
ms.openlocfilehash: 20ee2a9e045d839aa8ac043e035c438986b987ef
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76860867"
---
# <a name="profiling-global-static-functions"></a>グローバル静的関数のプロファイル
このセクションでは、プロファイリング API が使用するアンマネージ API 関数について説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
## <a name="net-framework-version-1-profiling-functions"></a>.NET Framework version 1 プロファイリング関数  
 [FunctionEnter 関数](functionenter-function.md)  
 コントロールが関数に渡されていることをプロファイラーに通知します。 .NET Framework 2.0 では非推奨となりました。  
  
 [FunctionLeave 関数](functionleave-function.md)  
 関数が呼び出し元に戻りようとしていることをプロファイラーに通知します。 .NET Framework 2.0 では非推奨となりました。  
  
 [FunctionTailcall 関数](functiontailcall-function.md)  
 現在実行中の関数が別の関数の末尾呼び出しを実行しようとしていることをプロファイラーに通知します。 .NET Framework 2.0 では非推奨となりました。  
  
## <a name="net-framework-version-2-profiling-functions"></a>.NET Framework version 2 プロファイリング関数  
 [FunctionIDMapper 関数](functionidmapper-function.md)  
 関数の特定の識別子が、その関数の[FunctionEnter2](functionenter2-function.md)、 [FunctionLeave2](functionleave2-function.md)、および[FunctionTailcall2](functiontailcall2-function.md)コールバックで使用される代替 ID に再マップされる可能性があることをプロファイラーに通知します。 また、プロファイラーが、その関数のコールバックを受信するかどうかを示すこともできます。  
  
 [FunctionEnter2 関数](functionenter2-function.md)  
 コントロールが関数に渡されていることをプロファイラーに通知し、スタックフレームと関数の引数に関する情報を提供します。 .NET Framework 4 では非推奨となりました。  
  
 [FunctionLeave2 関数](functionleave2-function.md)  
 関数が呼び出し元に戻り、スタックフレームおよび関数の戻り値に関する情報を提供することをプロファイラーに通知します。 .NET Framework 4 では非推奨となりました。  
  
 [FunctionTailcall2 関数](functiontailcall2-function.md)  
 現在実行中の関数が別の関数の末尾呼び出しを実行しようとしていることをプロファイラーに通知し、スタックフレームに関する情報を提供します。 .NET Framework 4 では非推奨となりました。  
  
 [StackSnapshotCallback 関数](stacksnapshotcallback-function.md)  
 [ICorProfilerInfo2::D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md)メソッドによって開始されるスタックウォーク中に、各マネージフレームおよびスタック上のアンマネージフレームの各実行に関する情報をプロファイラーに提供します。  
  
## <a name="net-framework-version-4-profiling-functions"></a>.NET Framework version 4 プロファイリング関数  
 [FunctionIDMapper2 関数](functionidmapper2-function.md)  
 指定された関数の識別子が、その関数の[FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)、 [FunctionTailcall3](functiontailcall3-function.md)、または[FunctionEnter3WithInfo](functionenter3withinfo-function.md)、 [FunctionLeave3WithInfo](functionleave3withinfo-function.md)、および[FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)コールバックで使用される代替 ID に再マップされる可能性があることをプロファイラーに通知します。 また、プロファイラーは、その関数のコールバックを受信するかどうかを示すこともできます。  
  
 `FunctionIDMapper2` は、`clientData` パラメーターを使用して[Functionidmapper](functionidmapper-function.md)関数を拡張します。これは、プロファイラーがランタイムを明確に区別するために使用する可能性があります。  
  
 [FunctionEnter3 関数](functionenter3-function.md)  
 コントロールが関数に渡されていることをプロファイラーに通知します。  
  
 [FunctionEnter3WithInfo 関数](functionenter3withinfo-function.md)  
 コントロールが関数に渡されていることをプロファイラーに通知し、スタックフレームと関数の引数を取得するために[ICorProfilerInfo3:: GetFunctionEnter3Info](icorprofilerinfo3-getfunctionenter3info-method.md)に渡すことができるハンドルを提供します。  
  
 [FunctionLeave3 関数](functionleave3-function.md)  
 関数から制御が返されていることをプロファイラーに通知します。  
  
 [FunctionLeave3WithInfo 関数](functionleave3withinfo-function.md)  
 関数から制御が返されていることをプロファイラーに通知し、スタックフレームと戻り値を取得するために[ICorProfilerInfo3:: GetFunctionLeave3Info](icorprofilerinfo3-getfunctionleave3info-method.md)に渡すことができるハンドルを提供します。  
  
 [FunctionTailcall3 関数](functiontailcall3-function.md)  
 現在実行中の関数が別の関数の末尾呼び出しを実行しようとしていることをプロファイラーに通知します。  
  
 [FunctionTailcall3WithInfo 関数](functiontailcall3withinfo-function.md)  
 現在実行中の関数が別の関数の末尾呼び出しを実行しようとしていることをプロファイラーに通知し、スタックフレームを取得するために[ICorProfilerInfo3:: GetFunctionTailcall3Info](icorprofilerinfo3-getfunctiontailcall3info-method.md)に渡すことができるハンドルを提供します。  
  
## <a name="related-sections"></a>関連セクション  
 [プロファイルの概要](profiling-overview.md)  
  
 [プロファイリングのインターフェイス](profiling-interfaces.md)  
  
 [列挙型のプロファイリング](profiling-enumerations.md)  
  
 [構造体のプロファイリング](profiling-structures.md)
