---
title: IHostAssemblyStore::ProvideAssembly メソッド
ms.date: 03/30/2017
api_name:
- IHostAssemblyStore.ProvideAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostAssemblyStore::ProvideAssembly
helpviewer_keywords:
- ProvideAssembly method [.NET Framework hosting]
- IHostAssemblyStore::ProvideAssembly method [.NET Framework hosting]
ms.assetid: 625c3dd5-a3f0-442c-adde-310dadbb5054
topic_type:
- apiref
ms.openlocfilehash: a93d700c9c398076d87156cd2eb9c6d0d08cccfd
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124484"
---
# <a name="ihostassemblystoreprovideassembly-method"></a>IHostAssemblyStore::ProvideAssembly メソッド
[IHostAssemblyManager:: GetNonHostStoreAssemblies](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-getnonhoststoreassemblies-method.md)から返された[ICLRAssemblyReferenceList](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)によって参照されていないアセンブリへの参照を取得します。 共通言語ランタイム (CLR) は、一覧に表示されないアセンブリごとに `ProvideAssembly` を呼び出します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ProvideAssembly (  
    [in]  AssemblyBindInfo *pBindInfo,  
    [out] UINT64           *pAssemblyId,  
    [out] UINT64           *pHostContext,  
    [out] IStream          **ppStmAssemblyImage,  
    [out] IStream          **ppStmPDB  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pBindInfo`  
 からホストが特定のバインド特性 (バージョン管理ポリシーの有無、バインド先のアセンブリなど) を決定するために使用する[Assemblybindinfo](../../../../docs/framework/unmanaged-api/hosting/assemblybindinfo-structure.md)インスタンスへのポインター。  
  
 `pAssemblyId`  
 入出力この `IStream`の要求されたアセンブリの一意の識別子へのポインター。  
  
 `pHostContext`  
 入出力プラットフォーム呼び出しを必要とせずに、要求されたアセンブリの証拠を決定するために使用されるホスト固有のデータへのポインター。 `pHostContext` は、マネージ <xref:System.Reflection.Assembly> クラスの <xref:System.Reflection.Assembly.HostContext%2A> プロパティに対応します。  
  
 `ppStmAssemblyImage`  
 入出力読み込むポータブル実行可能 (PE) イメージを含む `IStream` のアドレスへのポインター。アセンブリが見つからなかった場合は null。  
  
 `ppStmPDB`  
 入出力プログラムデバッグ (PDB) 情報を格納している `IStream` のアドレスへのポインター。 .pdb ファイルが見つからなかった場合は null。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`ProvideAssembly` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージドコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
|COR_E_FILENOTFOUND (0x80070002)|要求されたアセンブリが見つかりませんでした。|  
|E_NOT_SUFFICIENT_BUFFER|`pAssemblyId` によって指定されたバッファーサイズが、ホストが返す識別子を保持するのに十分な大きさではありません。|  
  
## <a name="remarks"></a>Remarks  
 `pAssemblyId` に対して返される id 値は、ホストによって指定されます。 識別子は、プロセスの有効期間内で一意である必要があります。 CLR は、この値をストリームの一意の識別子として使用します。 また、`ProvideAssembly`の他の呼び出しによって返される `pAssemblyId` の値に対して各値をチェックします。 ホストが別の `IStream`に対して同じ `pAssemblyId` 値を返した場合、CLR は、そのストリームの内容が既にマップされているかどうかを確認します。 その場合、ランタイムは新しいイメージをマップするのではなく、イメージの既存のコピーを読み込みます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRAssemblyReferenceList インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-interface.md)
- [IHostAssemblyStore インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-interface.md)
