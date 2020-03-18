---
title: IMetaDataEmit::MergeEnd メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.MergeEnd
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::MergeEnd
helpviewer_keywords:
- MergeEnd method [.NET Framework metadata]
- IMetaDataEmit::MergeEnd method [.NET Framework metadata]
ms.assetid: 2d64315a-1af1-4c60-aedf-f8a781914aea
topic_type:
- apiref
ms.openlocfilehash: 34ecfc2f01f22971e135358806adeea632e02f8b
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448030"
---
# <a name="imetadataemitmergeend-method"></a>IMetaDataEmit::MergeEnd メソッド

[IMetaDataEmit:: Merge](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-merge-method.md)の1つ以上の前の呼び出しで指定されたすべてのメタデータスコープを現在のスコープにマージします。

## <a name="syntax"></a>構文

```cppcpp
HRESULT MergeEnd ();
```

## <a name="parameters"></a>パラメーター

このメソッドはパラメーターを受け取りません。

## <a name="remarks"></a>コメント

このルーチンは、前の `IMetaDataEmit::Merge`の呼び出しで指定されたすべてのインポートスコープのメタデータの実際のマージを現在の出力スコープにトリガーします。

マージには、次の特殊な条件が適用されます。

- モジュールバージョン識別子 (MVID) はインポートされません。これは、インポートスコープ内のメタデータに固有であるためです。

- 既存のモジュール全体のプロパティは上書きされません。

  モジュールのプロパティが現在のスコープに対して既に設定されている場合は、モジュールのプロパティはインポートされません。 ただし、モジュールのプロパティが現在のスコープで設定されていない場合は、最初に検出されたときに1回だけインポートされます。 これらのモジュールのプロパティが再度検出された場合、それらは重複しています。 すべてのモジュールプロパティ (MVID を除く) の値が比較され、重複が見つからない場合は、エラーが発生します。

- 型定義 (`TypeDef`) の場合、重複は現在のスコープにマージされません。 `TypeDef` オブジェクトの + *GUID* + *バージョン番号*の各*完全修飾オブジェクト名*に対して重複がチェックされます。 名前または GUID に一致するものがあり、他の2つの要素のいずれかが異なる場合は、エラーが発生します。 それ以外の場合、3つすべての項目が一致すると、`MergeEnd` は、エントリが実際に重複していることを確認するための大まかなチェックを行います。それ以外の場合は、エラーが発生します。 この大まかなチェックは次のようになります。

  - 同じ順序で発生する同じメンバー宣言。 `mdPrivateScope` としてフラグが設定されているメンバー (「 [CorMethodAttr](../../../../docs/framework/unmanaged-api/metadata/cormethodattr-enumeration.md)列挙体」を参照) は、このチェックには含まれていません。これらは特別にマージされます。

  - 同じクラスレイアウト。

  つまり、`TypeDef` オブジェクトは、宣言されているすべてのメタデータスコープで常に完全かつ一貫して定義される必要があります。メンバーの実装 (クラスの場合) が複数のコンパイル単位にわたって分散されている場合、完全な定義はすべてのスコープに存在すると見なされ、各スコープに増分されることはありません。 たとえば、パラメーター名がコントラクトに関連する場合は、すべてのスコープに同じ方法で出力する必要があります。これらが関連性のない場合は、メタデータに出力されません。

  例外として、`TypeDef` オブジェクトは `mdPrivateScope`としてフラグが設定された増分メンバーを持つことができます。 これらの問題が発生すると、`MergeEnd` によって、重複の有無に関係なく、現在のスコープに増分的に追加されます。 コンパイラはプライベートスコープを認識しているため、コンパイラは規則の適用を行う必要があります。

- 相対仮想アドレス (RVAs) はインポートまたはマージされません。コンパイラは、この情報を再生成する必要があります。

- カスタム属性は、関連付けられているアイテムがマージされた場合にのみマージされます。 たとえば、クラスに関連付けられているカスタム属性は、クラスが最初に検出されたときにマージされます。 カスタム属性がコンパイル単位に固有の `TypeDef` または `MemberDef` に関連付けられている場合 (メンバーコンパイルのタイムスタンプなど) は、マージされず、そのようなメタデータを削除または更新するためにコンパイラによって行われます。

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー:** Cor

**ライブラリ:** Mscoree.dll のリソースとして使用されます。

**.NET Framework のバージョン:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]

## <a name="see-also"></a>参照

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
