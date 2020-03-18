---
title: ローダー ETW イベント
ms.date: 03/30/2017
helpviewer_keywords:
- loader events [.NET Framework]
- ETW, loader events (CLR)
ms.assetid: cb403cc6-56f8-4609-b467-cdfa09f07909
ms.openlocfilehash: 73665915a70225c2b1da47c7b60347b089564884
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716027"
---
# <a name="loader-etw-events"></a>ローダー ETW イベント
これらのイベントは、アプリケーションのドメイン、アセンブリ、およびモジュールのロードとアンロードに関連する情報を収集します。  
  
 すべてのローダー イベントは、 `LoaderKeyword` (0x8) キーワードで発生します。 `DCStart` および `DCEnd` のイベントは、`StartRundown`/`EndRundown` が有効になっている `LoaderRundownKeyword` (0x8) で発生します。 (詳細については、「 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)」を参照してください)。  

## <a name="application-domain-events"></a>アプリケーション ドメイン イベント
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Event|レベル|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`AppDomainLoad_V1` および `AppDomainUnLoad_V1`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`AppDomainDCStart_V1`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`AppDomainDCEnd_V1`|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|説明|  
|-----------|--------------|-----------------|  
|`AppDomainLoad_V1` (すべてのアプリケーション ドメインについて記録)|156|プロセスの有効期間中に、アプリケーション ドメインが作成されるたびに発生します。|  
|`AppDomainUnLoad_V1`|157|プロセスの有効期間中に、アプリケーション ドメインが破壊されるたびに発生します。|  
|`AppDomainDCStart_V1`|157|開始ランダウン中にアプリケーション ドメインを列挙します。|  
|`AppDomainDCEnd_V1`|158|終了ランダウン中にアプリケーション ドメインを列挙します。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|AppDomainID|win:UInt64|アプリケーション ドメインの一意の識別子。|  
|AppDomainFlags|win:UInt32|0x1: 既定のドメイン。<br /><br /> 0x2: 実行可能ファイル。<br /><br /> 0x4: アプリケーション ドメイン、ビット 28 ～ 31: このドメインの共有ポリシー。<br /><br /> 0: 共有ドメイン。|  
|AppDomainName|win:UnicodeString|わかりやすいアプリケーション ドメイン名。 プロセスの有効期間中に変更することがあります。|  
|AppDomainIndex|win:UInt32|このアプリケーション ドメインのインデックス。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  

## <a name="clr-loader-assembly-events"></a>CLR ローダー アセンブリ イベント  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Event|レベル|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`AssemblyLoad` および `AssemblyUnload`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`AssemblyDCStart`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`AssemblyDCEnd`|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|説明|  
|-----------|--------------|-----------------|  
|`AssemblyLoad_V1`|154|アセンブリが読み込まれたときに発生します。|  
|`AssemblyUnload_V1`|155|アセンブリがアンロードされたときに発生します。|  
|`AssemblyDCStart_V1`|155|開始ランダウン中にアセンブリを列挙します。|  
|`AssemblyDCEnd_V1`|156|終了ランダウン中にアセンブリを列挙します。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|AssemblyID|win:UInt64|アセンブリの一意の ID。|  
|AppDomainID|win:UInt64|このアセンブリのドメインの ID。|  
|BindingID|win:UInt64|アセンブリ バインディングを一意に識別する ID。|  
|AssemblyFlags|win:UInt32|0x1: ドメインに中立的なアセンブリ。<br /><br /> 0x2: 動的アセンブリ。<br /><br /> 0x4: アセンブリにネイティブ イメージがある。<br /><br /> 0x8: 収集可能なアセンブリ。|  
|AssemblyName|win:UnicodeString|完全修飾アセンブリ名。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|   

## <a name="module-events"></a>モジュール イベント
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Event|レベル|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`ModuleLoad_V2` および `ModuleUnload_V2`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`ModuleDCStart_V2`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`ModuleDCEnd_V2`|情報提供 (4)|  
||||  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|説明|  
|-----------|--------------|-----------------|  
|`ModuleLoad_V2`|152|プロセスの有効期間中にモジュールが読み込まれるときに発生します。|  
|`ModuleUnload_V2`|153|プロセスの有効期間中にモジュールがアンロードされるときに発生します。|  
|`ModuleDCStart_V2`|153|開始ランダウン中にモジュールを列挙します。|  
|`ModuleDCEnd_V2`|154|終了ランダウン中にモジュールを列挙します。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt64|モジュールの一意な ID。|  
|AssemblyID|win:UInt64|このモジュールが存在するアセンブリの ID。|  
|ModuleFlags|win:UInt32|0x1: ドメインに中立的なモジュール。<br /><br /> 0x2: モジュールにネイティブ イメージがある。<br /><br /> 0x4: 動的モジュール。<br /><br /> 0x8: マニフェスト モジュール。|  
|Reserved1|win:UInt32|予約済みのフィールド。|  
|ModuleILPath|win:UnicodeString|モジュールの Microsoft intermediate language (MSIL) のイメージのパス、またはそれが動的アセンブリ (null で終わる) である場合は動的モジュール名。|  
|ModuleNativePath|win:UnicodeString|モジュール ネイティブ イメージがある場合、そのパス (null で終わる)。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
|ManagedPdbSignature|win:GUID|このモジュールに一致するマネージド プログラム データベース (PDB) の GUID の署名。 (「解説」を参照してください。)|  
|ManagedPdbAge|win:UInt32|このモジュールに一致する管理対象 PDB に書き込まれた期間を表す数値。 (「解説」を参照してください。)|  
|ManagedPdbBuildPath|win:UnicodeString|このモジュールに一致する管理対象の PDB が構成されている場所へのパス。 これは、ファイル名だけの場合もあります。 (「解説」を参照してください。)|  
|NativePdbSignature|win:GUID|このモジュールに一致するネイティブ イメージ ジェネレーター (NGen) PDB の GUID の署名 (該当する場合)。 (「解説」を参照してください。)|  
|NativePdbAge|win:UInt32|このモジュールに一致する NGen PDB に書き込まれた期間を表す数値 (該当する場合)。 (「解説」を参照してください。)|  
|NativePdbBuildPath|win:UnicodeString|このモジュールに一致する管理対象の NGen PDB が構成されている場所へのパス (該当する場合)。 これは、ファイル名だけの場合もあります。 (「解説」を参照してください。)|  
  
### <a name="remarks"></a>Remarks  
  
- 名前に"Pdb"が付いているフィールドは、プロファイル セッション中に読み込まれたモジュールに一致する PDB を検索するプロファイリング ツールによって使用できます。 これらのフィールドの値は、読み込まれたモジュールに一致する PDB の位置を特定するためにデバッガーが通常使用する、モジュールの IMAGE_DIRECTORY_ENTRY_DEBUG のセクションに書き込まれたデータに対応します。  
  
- "ManagedPdb"で始まるフィールド名は、マネージド コンパイラ (c# または Visual Basic コンパイラなど) によって生成された MSIL モジュールに対応する管理対象の PDB を参照します。 この PDB は、管理対象の PDB 形式を使用して、ファイル、行番号、およびシンボルの名前など、元のマネージド ソース コードからの要素が MSIL モジュールにコンパイルされている MSIL 要素にどのようにマップされるかについて説明します。  
  
- "NativePdb"で始まるフィールド名は、 `NGEN createPDB`を呼び出すことによって生成された NGen PDB を参照します。 この PDB は、ネイティブ PDB 形式を使用して、ファイル、行番号、およびシンボルの名前など、元のマネージド ソース コードからの要素が NGen モジュールにコンパイルされているネイティブ要素にどのようにマップされるかについて説明します。  

## <a name="clr-domain-module-events"></a>CLR ドメイン モジュール イベント
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Event|レベル|  
|-----------------------------------|-----------|-----------|  
|`LoaderKeyword` (0x8)|`DomainModuleLoad_V1`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `StartRundownKeyword`|`DomainModuleDCStart_V1`|情報提供 (4)|  
|`LoaderRundownKeyword` (0x8) +<br /><br /> `EndRundownKeyword`|`DomainModuleDCEnd_V1`|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|説明|  
|-----------|--------------|-----------------|  
|`DomainModuleLoad_V1`|151|モジュールがアプリケーション ドメインに読み込まれるときに発生します。|  
|`DomainModuleDCStart_V1`|151|開始ランダウン中にアプリケーション ドメインに読み込まれたモジュールを列挙し、すべてのアプリケーション ドメインについてログに記録されます。|  
|`DomainModuleDCEnd_V1`|152|終了ランダウン中にアプリケーション ドメインに読み込まれたモジュールを列挙し、すべてのアプリケーション ドメインについてログに記録されます。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|ModuleID|win:UInt64|このモジュールが所属するアセンブリを識別します。|  
|AssemblyID|win:UInt64|このモジュールが存在するアセンブリの ID。|  
|AppDomainID|win:UInt64|このモジュールを使用する、アプリケーション ドメインの ID。|  
|ModuleFlags|win:UInt32|0x1: ドメインに中立的なモジュール。<br /><br /> 0x2: モジュールにネイティブ イメージがある。<br /><br /> 0x4: 動的モジュール。<br /><br /> 0x8: マニフェスト モジュール。|  
|Reserved1|win:UInt32|予約済みのフィールド。|  
|ModuleILPath|win:UnicodeString|モジュールの MSIL のイメージのパス、またはそれが動的アセンブリ (null で終わる) である場合は動的モジュール名。|  
|ModuleNativePath|win:UnicodeString|モジュール ネイティブ イメージがある場合、そのパス (null で終わる)。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  

## <a name="module-range-events"></a>モジュールの範囲イベント
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Event|レベル|  
|-----------------------------------|-----------|-----------|  
|`PerfTrackKeyWord`)|`ModuleRange`|情報提供 (4)|  
|`PerfTrackKeyWord`|`ModuleRangeDCStart`|情報提供 (4)|  
|`PerfTrackKeyWord`|`ModuleRangeDCEnd`|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|説明|  
|-----------|--------------|-----------------|  
|`ModuleRange`|158|ロードされたネイティブ イメージ ジェネレーター (NGen) のイメージが IBC によって最適化されていて、NGen イメージのホット セクションに関する情報が含まれる場合は、このイベントが存在します。|  
|`ModuleRangeDCStart`|160|ランダウンの開始時に発生する `ModuleRange` イベント。|  
|`ModuleRangeDCEnd`|161|ランダウンの終了時に発生する `ModuleRange` イベント。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:UInt16|CLR の複数のインスタンスが読み込まれている場合、プロセス内の CLR の特定のインスタンスを一意に識別します。|  
|ModuleID|win:UInt64|このモジュールが所属するアセンブリを識別します。|  
|RangeBegin|win:UInt32|指定された範囲の種類の範囲の開始を表す、モジュール内のオフセット。|  
|RangeSize|win:UInt32|指定された範囲のサイズ (バイト単位)。|  
|RangeType|win:UInt32|コールド IBC 範囲を表す単一の値、0x4。 このフィールドは、後により多くの値を表すことができるようになります。|  
|RangeSize1|win:UInt32|0 は無効なデータを示します。|  
|RangeBegin2|win:UnicodeString||  
  
### <a name="remarks"></a>Remarks  
 .NET Framework のプロセスで読み込まれた NGen イメージが IBC に最適化されている場合、NGen イメージにホットの範囲を含む `ModuleRange` イベントが `moduleID` と `ClrInstanceID`と共にログに記録されます。  NGen イメージが IBC に最適化されていない場合は、このイベントは記録されません。 モジュール名を確認するには、このイベントをモジュールの読み込みの ETW イベントで照合する必要があります。  
  
 このイベントのペイロードのサイズは変数です。 `Count` フィールドは、イベントに含まれている範囲オフセットの数を示します。  このイベントは、実際の範囲を判断するために Windows `IStart` イベントと照合する必要があります。 Windows Image Load イベントは、イメージが読み込まれ、読み込まれたイメージの仮想アドレスが含まれている場合は必ず記録されます。  
  
 モジュールの範囲イベントは、4 以上のすべての ETW レベルで発行され、情報イベントに分類されます。  
  
## <a name="see-also"></a>関連項目

- [CLR ETW イベント](clr-etw-events.md)
