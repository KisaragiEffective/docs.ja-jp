---
title: AssemblyAttributesGoHereSM クラス (System.runtime.compilerservices)
ms.date: 03/30/2017
api_name:
- System.Runtime.CompilerServices.AssemblyAttributesGoHereSM
api_location:
- mscorlib.dll
api_type:
- Assembly
f1_keywords:
- AssemblyAttributesGoHereSM
helpviewer_keywords:
- AssemblyAttributesGoHereSM type
- Alink API, AssemblyAttributesGoHereSM type
ms.assetid: 4cf9bf39-1527-49e0-a0e9-55e7a018bf66
topic_type:
- apiref
ms.openlocfilehash: 379ba20ebf675bec71e6e5f5bcfc0dc2fbd1f92c
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446599"
---
# <a name="assemblyattributesgoheresm-class"></a>AssemblyAttributesGoHereSM クラス

ALink でプレースホルダーとして使用し、カスタム属性に関する情報を格納します。

## <a name="syntax"></a>構文

```csharp
internal sealed class AssemblyAttributesGoHereSM
```

## <a name="remarks"></a>コメント

この型への参照は、ソースにアセンブリのカスタム属性が含まれている netmodule 内部に埋め込まれていることがあります。 これらの型への参照が含まれる 1 つまたは複数の  netmodule からアセンブリ マニフェストを作成すると、ALink はこれらの参照にアタッチされた情報を使用して、実際のカスタム属性を生成します。 このため、この型がインスタンス化されることはなく、その型への参照はビルド処理の一部としてのみ使用され、最終的なアセンブリでは使用されません。

この型への参照は、セキュリティに関連して複数の用途を持つカスタム属性を示します。

これらの型は、.NET Framework 内で "internal" とマークされ、<xref:System.Runtime.CompilerServices> 名前空間に配置されます。

## <a name="requirements"></a>要件

mscorlib.dll

## <a name="see-also"></a>参照

- [AssemblyAttributesGoHere](assemblyattributesgohere.md)
- [AssemblyAttributesGoHereM](assemblyattributesgoherem.md)
- [AssemblyAttributesGoHereS](assemblyattributesgoheres.md)
