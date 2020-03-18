---
title: ICorDebugSymbolProvider インターフェイス
ms.date: 03/30/2017
ms.assetid: 85b24196-b6c6-4bda-9de3-47180bd6ff96
ms.openlocfilehash: 6f7a8a2b12c047b956a3b6e85fe8365e0360b3f2
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791528"
---
# <a name="icordebugsymbolprovider-interface"></a>ICorDebugSymbolProvider インターフェイス
デバッグ シンボル情報を取得するために使用できるメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetAssemblyImageBytes メソッド](icordebugsymbolprovider-getassemblyimagebytes-method.md)|マージされたアセンブリ内の指定の相対仮想アドレス (RVA: relative virtual address) で、マージされたアセンブリのデータを読み取ります。|  
|[GetAssemblyImageMetadata メソッド](icordebugsymbolprovider-getassemblyimagemetadata-method.md)|マージされたアセンブリのメタデータを返します。|  
|[GetCodeRange メソッド](icordebugsymbolprovider-getcoderange-method.md)|メソッド内の指定の相対仮想アドレス (RVA: relative virtual address) で、メソッド開始アドレスとサイズを取得します。|  
|[GetInstanceFieldSymbols メソッド](icordebugsymbolprovider-getinstancefieldsymbols-method.md)|typespec シグネチャに対応するインスタンス フィールド シンボルを取得します。|  
|[GetMergedAssemblyRecords メソッド](icordebugsymbolprovider-getmergedassemblyrecords-method.md)|すべてのマージされたアセンブリのシンボル レコードを取得します。|  
|[GetMethodLocalSymbols メソッド](icordebugsymbolprovider-getmethodlocalsymbols-method.md)|メソッドの指定の相対仮想アドレス (RVA) で、そのメソッドのローカル シンボルを取得します。|  
|[GetMethodParameterSymbols メソッド](icordebugsymbolprovider-getmethodparametersymbols-method.md)|メソッドの指定の相対仮想アドレス (RVA: relative virtual address ) で、そのメソッドのパラメーター シンボルを取得します。|  
|[GetMethodProps メソッド](icordebugsymbolprovider-getmethodprops-method.md)|メソッドの指定の相対仮想アドレス (RVA) で、そのメソッドのプロパティに関する情報 (メソッドのメタデータ トークンなど) と、そのジェネリック パラメーターに関する情報を返します。|  
|[GetObjectSize メソッド](icordebugsymbolprovider-getobjectsize-method.md)|typespec シグネチャに基づいてオブジェクトのオブジェクト サイズを返します。|  
|[GetStaticFieldSymbols メソッド](icordebugsymbolprovider-getstaticfieldsymbols-method.md)|typespec シグネチャに対応する静的フィールド シンボルを取得します。|  
|[GetTypeProps メソッド](icordebugsymbolprovider-gettypeprops-method.md)|Vtable の指定の相対仮想アドレス (RVA) における、ジェネリック パラメーターのシグネチャの数などの型のプロパティに関する情報を返します。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは .NET ネイティブでのみ使用可能です。 .NET ネイティブの外部で ICorDebug シナリオについてこのインターフェイスを実装する場合は、共通言語ランタイムはこのインターフェイスを無視します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
