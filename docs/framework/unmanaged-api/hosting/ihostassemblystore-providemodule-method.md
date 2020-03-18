---
title: IHostAssemblyStore::ProvideModule メソッド
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore.ProvideModule
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore::ProvideModule
helpviewer_keywords:
- IHostAssemblyStore::ProvideModule method [.NET Framework hosting]
- ProvideModule method [.NET Framework hosting]
ms.assetid: f42e3dd0-c88e-4748-b6c0-4c515a633180
topic_type:
- apiref
ms.openlocfilehash: eed09a8149a21140ad61133f29391f86cb0fb929
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124473"
---
# <a name="ihostassemblystoreprovidemodule-method"></a>IHostAssemblyStore::ProvideModule メソッド
アセンブリ内のモジュール、またはリンクされている (埋め込みではない) リソースファイル内のモジュールを解決します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ProvideModule (  
    [in]  ModuleBindInfo *pBindInfo,  
    [out] DWORD          *pdwModuleId,  
    [out] IStream        **ppStmModuleImage,  
    [out] IStream        **ppStmPDB  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pBindInfo`  
 から要求されたモジュールの <xref:System.AppDomain>、アセンブリ、およびモジュール名を記述する[Modulebindinfo](../../../../docs/framework/unmanaged-api/hosting/modulebindinfo-structure.md)インスタンスへのポインター。  
  
 `pdwModuleId`  
 入出力読み込まれたモジュールを含む `IStream` の一意の識別子へのポインター。  
  
 `ppStmModuleImage`  
 入出力読み込むポータブル実行可能 (PE) イメージを格納する `IStream` オブジェクトのアドレスへのポインター。モジュールが見つからなかった場合は null。  
  
 `ppStmPDB`  
 入出力要求されたモジュールのプログラムデバッグ (PDB) 情報を格納している `IStream` オブジェクトのアドレスへのポインター。 .pdb ファイルが見つからなかった場合は null。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`ProvideModule` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
|COR_E_FILENOTFOUND (0x80070002)|要求されたアセンブリまたはリンクされたリソースが見つかりませんでした。|  
|E_NOT_SUFFICIENT_BUFFER|`pdwModuleId` は、ホストが返す識別子を格納するのに十分な大きさではありません。|  
  
## <a name="remarks"></a>Remarks  
 `pdwModuleId` に対して返される id 値は、ホストによって指定されます。 識別子は、プロセスの有効期間内で一意である必要があります。 CLR は、この値を、関連付けられているストリームの一意の識別子として使用します。 各値は、[アセンブリ](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-provideassembly-method.md)の呼び出しによって返される `pAssemblyId` の値と、`ProvideModule`の他の呼び出しによって返される `pdwModuleId` の値とを比較して確認されます。 ホストが別の `IStream`に対して同じ識別子の値を返した場合、CLR は、そのストリームの内容が既にマップされているかどうかを確認します。 その場合、CLR は新しいイメージをマップするのではなく、イメージの既存のコピーを読み込みます。 したがって、識別子は `ProvideAssembly`から返されたアセンブリ識別子と重複することはできません。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRAssemblyReferenceList インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-interface.md)
- [IHostAssemblyStore インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-interface.md)
