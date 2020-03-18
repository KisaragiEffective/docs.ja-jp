---
title: ICLRDataTarget インターフェイス
ms.date: 03/30/2017
api_name:
- ICLRDataTarget
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget
helpviewer_keywords:
- ICLRDataTarget interface [.NET Framework debugging]
ms.assetid: e2f05155-9bef-4e11-b703-7f05890665ca
topic_type:
- apiref
ms.openlocfilehash: 2b5c99e40aabdbc654bdc612729b2756e3ef5bb4
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793714"
---
# <a name="iclrdatatarget-interface"></a>ICLRDataTarget インターフェイス
共通言語ランタイム (CLR) のターゲット項目と対話するためのメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetCurrentThreadID メソッド](iclrdatatarget-getcurrentthreadid-method.md)|現在のスレッドのオペレーティングシステム id を取得します。|  
|[GetImageBase メソッド](iclrdatatarget-getimagebase-method.md)|指定したイメージのベースメモリアドレスを取得します。|  
|[GetMachineType メソッド](iclrdatatarget-getmachinetype-method.md)|ターゲットプロセスが使用している命令セットの種類の識別子を取得します。|  
|[GetPointerSize メソッド](iclrdatatarget-getpointersize-method.md)|現在のターゲットへのポインターのサイズ (バイト単位) を取得します。|  
|[GetThreadContext メソッド](iclrdatatarget-getthreadcontext-method.md)|指定した識別子を持つスレッドのコンテキストへのポインターを取得します。|  
|[GetTLSValue メソッド](iclrdatatarget-gettlsvalue-method.md)|指定したスレッドの指定したインデックスにあるスレッドローカルストレージ (TLS) の値を取得します。|  
|[ReadVirtual メソッド](iclrdatatarget-readvirtual-method.md)|指定された仮想メモリアドレスから指定されたバッファーにデータを読み取ります。|  
|[Request メソッド](iclrdatatarget-request-method.md)|実装で定義されているように、操作を要求するために、共通言語ランタイム (CLR) データアクセスサービスによって呼び出されます。|  
|[SetThreadContext メソッド](iclrdatatarget-setthreadcontext-method.md)|ターゲットプロセス内の指定されたスレッドの現在のコンテキストを設定します。|  
|[SetTLSValue メソッド](iclrdatatarget-settlsvalue-method.md)|ターゲットプロセス内の指定したスレッドのスレッドローカルストレージ (TLS) の値を設定します。|  
|[WriteVirtual メソッド](iclrdatatarget-writevirtual-method.md)|指定されたバッファーから指定された仮想メモリアドレスにデータを書き込みます。|  
  
## <a name="remarks"></a>コメント  
 API クライアント (つまり、デバッガー) は、特定のターゲット項目に適した方法でこのインターフェイスを実装する必要があります。 たとえば、ライブ プロセスの実装は、メモリ ダンプの実装とは異なります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** ClrData .idl, ClrData .h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRDataTarget2 インターフェイス](iclrdatatarget2-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
