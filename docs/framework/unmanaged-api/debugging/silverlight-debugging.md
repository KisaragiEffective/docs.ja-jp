---
title: Silverlight デバッグ
ms.date: 03/30/2017
helpviewer_keywords:
- debugging API [Silverlight]
- Silverlight, debugging
ms.assetid: 5e903e04-17d0-4014-ac9a-a43330ec8b1c
ms.openlocfilehash: 91f311818b615ea8f166bb3362ec52d39fcd0297
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790327"
---
# <a name="silverlight-debugging"></a>Silverlight デバッグ
このセクションのトピックでは、Windows オペレーティング システム上または Macintosh プラットフォーム上で動作している Silverlight ベースのアプリケーションのデバッグをサポートするために共通言語ランタイム (CLR: Common Language Runtime) で提供される環境とインターフェイスについて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [EnumerateCLRs 関数](enumerateclrs-function.md)  
 プロセスで CLR を列挙するメカニズムを提供します。  
  
 [CloseCLREnumeration 関数](closeclrenumeration-function.md)  
 [列挙型 Ateclrs 関数](enumerateclrs-function.md)によって返されたハンドルの配列にある有効な CLR 継続スタートアップイベントを閉じ、ハンドルおよび文字列パス配列のメモリを解放します。  
  
 [CreateCoreClrDebugTarget 関数](createcoreclrdebugtarget-function.md)  
 プロセスおよびランタイムの列挙のためのリモート ターゲットへの接続を作成します。  
  
 [CreateCordbObject 関数](createcordbobject-function.md)  
 リモート プロセスでマネージド デバッグ セッションをインスタンス化するための機能を提供するデバッガー インターフェイスを作成します。  
  
 [CreateVersionStringFromModule 関数](createversionstringfrommodule-function.md)  
 対象プロセス内の CLR パスからバージョン文字列を作成します。  
  
 [CreateDebuggingInterfaceFromVersion 関数](createdebugginginterfacefromversion-function-for-silverlight.md)  
 [Createversionstringfrommodule 関数](createversionstringfrommodule-function.md)関数から返された CLR バージョン文字列を受け取り、対応するデバッガーインターフェイスを返します。  
  
 [CoreClrDebugProcInfo 構造体](coreclrdebugprocinfo-structure.md)  
 リモート コンピューターで実行されているプロセスを表します。  
  
 [CoreClrDebugRuntimeInfo 構造体](coreclrdebugruntimeinfo-structure.md)  
 リモート コンピューター上のプロセスに読み込まれる CLR インスタンスを表します。  
  
 [GetStartupNotificationEvent 関数](getstartupnotificationevent-function.md)  
 指定された対象プロセスに読み込まれている任意の共通言語ランタイム (CLR: Common Language Runtime) によって通知されるイベント ハンドルを作成または開きます。  
  
 [ICoreClrDebugTarget インターフェイス](icoreclrdebugtarget-interface.md)  
 プロセスおよびランタイムの列挙のためのリモート ターゲットへの接続を作成します。  
  
 [InitDbgTransportManager 関数](initdbgtransportmanager-function.md)  
 プロセスおよびランタイムの列挙のためにリモート ターゲットに接続するよう、トランスポート マネージャーを初期化します。  
  
 [ShutdownDbgTransportManager 関数](shutdowndbgtransportmanager-function.md)  
 リモート ターゲット コンピューターへの接続のためにトランスポート マネージャーをシャットダウンします。  
  
## <a name="see-also"></a>関連項目

- [デバッグ コクラス](debugging-coclasses.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ グローバル静的関数](debugging-global-static-functions.md)
- [列挙型のデバッグ](debugging-enumerations.md)
- [デバッグ構造体](debugging-structures.md)
