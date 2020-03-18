---
title: AssemblyAttributesGoHereM クラス (System.runtime.compilerservices)
ms.date: 03/30/2017
api_name:
- System.Runtime.CompilerServices.AssemblyAttributesGoHereM
api_location:
- mscorlib.dll
api_type:
- Assembly
f1_keywords:
- AssemblyAttributesGoHereM
helpviewer_keywords:
- AssemblyAttributesGoHereM type
- Alink API, AssemblyAttributesGoHereM type
ms.assetid: caaa8ba9-b4bb-4dd6-934d-57e436b2f3e5
topic_type:
- apiref
ms.openlocfilehash: 15b9445aa3eabbd14541cfe5481bfb553c8c0347
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446633"
---
# <a name="assemblyattributesgoherem-class"></a>AssemblyAttributesGoHereM クラス

ALink でプレースホルダーとして使用し、カスタム属性に関する情報を格納します。

## <a name="syntax"></a>構文

```csharp
internal sealed class AssemblyAttributesGoHereM
```

## <a name="remarks"></a>コメント

この型への参照は、ソースにアセンブリのカスタム属性が含まれている netmodule 内部に埋め込まれていることがあります。 これらの型への参照が含まれる 1 つまたは複数の  netmodule からアセンブリ マニフェストを作成すると、ALink はこれらの参照にアタッチされた情報を使用して、実際のカスタム属性を生成します。 このため、この型がインスタンス化されることはなく、その型への参照はビルド処理の一部としてのみ使用され、最終的なアセンブリでは使用されません。

この型への参照は、セキュリティに関連せず複数の用途を持つカスタム属性を示します。

これらの型は、.NET Framework 内で "internal" とマークされ、<xref:System.Runtime.CompilerServices> 名前空間に配置されます。

## <a name="requirements"></a>要件

mscorlib.dll

## <a name="see-also"></a>参照

- [AssemblyAttributesGoHere](assemblyattributesgohere.md)
- [AssemblyAttributesGoHereS](assemblyattributesgoheres.md)
- [AssemblyAttributesGoHereSM](assemblyattributesgoheresm.md)
