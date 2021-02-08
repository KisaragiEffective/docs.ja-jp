---
description: 詳細については、「Common Data Types (アンマネージ API リファレンス)」を参照してください。
title: 共有のデータ型 (アンマネージ API リファレンス)
ms.date: 03/30/2017
ms.assetid: e4ab2c4c-9433-4eba-9e9a-096de406cafb
ms.openlocfilehash: 037a212d9b58adbfdc24635bc651946af9f9bb7c
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99772823"
---
# <a name="common-data-types-unmanaged-api-reference"></a>共有のデータ型 (アンマネージ API リファレンス)

このトピックでは、C/C++ `typedef` ステートメントで定義される .NET Framework のアンマネージ API で使用する、簡単なデータ型について示します。 これらのデータ型は通常、C/C++ のプリミティブ データ型のエイリアスです。 一般的にこれらのデータ型の値は不透明です。これらのデータ型の値は他の関数またはメソッドに対して変更なしで渡せるように、特定の関数またはメソッドによって返されるためです。  
  
|データ型|定義|定義されている場所|説明|  
|---------------|----------------|----------------|-----------------|  
|AppDomainID|`typedef UINT_PTR AppDomainID;`|corprof.h|アプリケーション ドメインの識別子。|  
|AssemblyID|`typedef UINT_PTR AssemblyID;`|corprof.h|アセンブリの識別子。|  
|ClassID|`typedef UINT_PTR ClassID;`|corprof.h|マネージド クラスの識別子。|  
|CLRDATA_ADDRESS|`typedef ULONG64 CLRDATA_ADDRESS;`|clrdata. h|64ビットのメモリアドレス。|
|CLRDATA_ENUM|`typedef ULONG64 CLRDATA_ADDRESS;`|利用不可|64ビットのメモリアドレス。|
|CONNID|`typedef DWORD CONNID;`|cordebug.h、mscoree.h|Microsoft SQL Server のインスタンスへ接続されるスレッドの接続識別子。|  
|ContextID|`typedef UINT_PTR ContextID;`|corprof.h|特定のマネージド スレッドに関連付けられているコンテキストの識別子。|  
|COR_PRF_ELT_INFO|`typedef UINT_PTR COR_PRF_ELT_INFO;`|corprof.h|特定のスタック フレームに関する情報を表す不透明なハンドル。|  
|COR_PRF_FRAME_INFO|`typedef UINT_PTR COR_PRF_FRAME_INFO;`|corprof.h|スタック フレームを指す不透明なハンドル。 これは、自身が渡されるコールバックの間のみ有効です。|  
|CORDB_ADDRESS|`typedef ULONG64 CORDB_ADDRESS;`|cordebug.h|メモリ内アドレス。|  
|CORDB_CONTINUE_STATUS|`typedef DWORD CORDB_CONTINUE_STATUS;`|cordebug.h|継続状態。|  
|CORDB_REGISTER|`typedef ULONG64 CORDB_REGISTER;`|cordebug.h|CPU レジスタの値。|
|FunctionID|`typedef UINT_PTR FunctionID;`|corprof.h|関数またはメソッドの識別子。|  
|GCHandleID|`typedef UINT_PTR GCHandleID;`|corprof.h|ガベージ コレクション ハンドル。|  
|mdMethodDef|`typedef mdToken mdMethodDef;`|cordebug.h|メソッド定義トークン。|
|mdToken|`typedef UINT32 mdToken;`|corprof.h|メタデータトークン (メタデータテーブル内の行)。|  
|ModuleID|`typedef UINT_PTR ModuleID;`|corprof.h|アセンブリ モジュールの識別子。|  
|ObjectID|`typedef UINT_PTR ObjectID;`|corprof.h|オブジェクトの識別子。|  
|PCCOR_SIGNATURE|`typedef SIZE_T PCCOR_SIGNATURE;`|cordebug.h|メンバーまたはメタデータシグネチャへのポインター。|
|ProcessID|`typedef UINT_PTR ProcessID;`|corprof.h|マネージド プロセスの識別子。|  
|ReJITID|`typedef UINT_PTR ReJITID;`|corprof.h|JIT コンパイルされた関数の識別子。|  
|SIZE_T|`typedef ULONG_PTR SIZE_T;`|corsym .h|64ビットメモリアドレスへのポインター。|
|TASKID|`typedef UINT64 TASKID;`|cordebug.h、mscoree.h|[ICLRTask](./hosting/iclrtask-interface.md)インスタンスの識別子。|  
|ThreadID|`typedef UINT_PTR ThreadID;`|corprof.h|マネージド スレッドの識別子。|  
  
## <a name="see-also"></a>関連項目

- [アンマネージ API リファレンス](index.md)
