---
title: ICorProfilerInfo6::EnumNgenModuleMethodsInliningThisMethod メソッド
ms.date: 03/30/2017
ms.assetid: b933dfe6-7833-40cb-aad8-40842dc3034f
ms.openlocfilehash: 103fe1b6845edfe0a364db979557db63511f6ee3
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130383"
---
# <a name="icorprofilerinfo6enumngenmodulemethodsinliningthismethod-method"></a>ICorProfilerInfo6::EnumNgenModuleMethodsInliningThisMethod メソッド

特定の NGen モジュールで定義され、指定されたメソッドにインライン化されているすべてのメソッドに対する列挙子を返します。

## <a name="syntax"></a>構文

```cpp
HRESULT EnumNgenModuleMethodsInliningThisMethod(
        [in] ModuleID inlinersModuleId,
        [in] ModuleID inlineeModuleId,
        [in] mdMethodDef inlineeMethodId,
        [out] BOOL *incompleteData,
        [out] ICorProfilerMethodEnum** ppEnum
);
```

## <a name="parameters"></a>パラメーター

`inlinersModuleId`\
からNGen モジュールの識別子。

`inlineeModuleId`\
から`inlineeMethodId`を定義するモジュールの識別子。 詳細については、次の「解説」を参照してください。

`inlineeMethodId`\
からインラインメソッドの識別子。 詳細については、次の「解説」を参照してください。

`incompleteData`\
入出力指定したメソッドをインライン展開するすべてのメソッドが `ppEnum` に含まれているかどうかを示すフラグ。  詳細については、次の「解説」を参照してください。

`ppEnum`\
入出力列挙子のアドレスへのポインター

## <a name="remarks"></a>Remarks

`inlineeModuleId` と `inlineeMethodId` は、インライン化される可能性のあるメソッドの完全な識別子を形成します。 たとえば、モジュール `A` がメソッド `Simple.Add`を定義するとします。

```csharp
Simple.Add(int a, int b)
{ return a + b; }
```

モジュール B は `Fancy.AddTwice`を定義します。

```csharp
Fancy.AddTwice(int a, int b)
{ return Simple.Add(a,b) + Simple.Add(a,b); }
```

また、`SimpleAdd`への呼び出しをインライン `Fancy.AddTwice` ことを前提としています。 プロファイラーは、この列挙子を使用して、モジュール B で定義されているすべてのメソッド (インライン `Simple.Add`) を検索し、結果は `AddTwice`を列挙できます。  `inlineeModuleId` は、モジュール `A`の識別子であり、`inlineeMethodId` は `Simple.Add(int a, int b)`の識別子です。

関数がを返した後に `incompleteData` が true の場合、列挙子には、特定のメソッドのインライン展開を行うメソッドがすべて含まれているわけではありません。 これは、inliners モジュールの1つ以上の直接または間接的な依存関係がまだ読み込まれていない場合に発生する可能性があります。 プロファイラーが正確なデータを必要とする場合、より多くのモジュールが読み込まれると、後でモジュールの負荷に応じて、後で再試行する必要があります。

`EnumNgenModuleMethodsInliningThisMethod` メソッドを使用すると、ReJIT のインライン展開に関する制限を回避できます。 ReJIT を使用すると、プロファイラーはメソッドの実装を変更し、その場で新しいコードを作成できます。 たとえば、次のように `Simple.Add` を変更できます。

```csharp
Simple.Add(int a, int b)
{ return 42; }
```

ただし、`Fancy.AddTwice` は既に `Simple.Add`インライン化されているので、以前と同じ動作を続けます。 この制限を回避するには、呼び出し元は、インライン `Simple.Add` されているすべてのモジュール内のすべてのメソッドを検索し、これらの各メソッドで `ICorProfilerInfo5::RequestRejit` を使用する必要があります。 メソッドを再コンパイルすると、以前の動作ではなく、`Simple.Add` の新しい動作が行われます。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー** : CorProf.idl、CorProf.h

**ライブラリ:** CorGuids.lib

**.NET Framework のバージョン:** [!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]

## <a name="see-also"></a>関連項目

- [ICorProfilerInfo6 インターフェイス](icorprofilerinfo6-interface.md)
