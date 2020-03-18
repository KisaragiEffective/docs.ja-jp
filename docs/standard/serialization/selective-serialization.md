---
title: 選択的シリアル化
ms.date: 08/07/2017
dev_langs:
- CSharp
helpviewer_keywords:
- serialization, selective serialization
- binary serialization, selective serialization
ms.assetid: 39c56635-95d2-4afd-aff1-b022e7649bb3
ms.openlocfilehash: cc5d7964d5f3268f08721593fefc07e3eff853ca
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159599"
---
# <a name="selective-serialization"></a>選択的シリアル化
クラスには、シリアル化できないフィールドが含まれていることがよくあります。 たとえば、クラスのメンバー変数の 1 つにスレッド ID が格納されているとします。 クラスを逆シリアル化すると、クラスのシリアル化時に格納された ID を持つスレッドが実行されなくなることがあります。したがって、この値をシリアル化しても意味はありません。 以下のように、メンバー変数に [NonSerialized](xref:System.NonSerializedAttribute) 属性を使用してマークすることで、メンバー変数がシリアル化されないようにすることができます。  
  
```csharp  
[Serializable]  
public class MyObject
{  
  public int n1;  
  [NonSerialized] public int n2;  
  public String str;  
}  
```

機密データを含むオブジェクトは、可能であれば、シリアル化できないようにしてください。 オブジェクトをシリアル化する必要がある場合は、機密データを格納する特定のフィールドに `NonSerialized` 属性を適用します。 これらのフィールドをシリアル化の対象から除外しない場合は、フィールドに格納されているデータがシリアル化のアクセス許可を持つ任意のコードに公開されることに注意してください。 安全なシリアル化コードの記述の詳細については、「[セキュリティとシリアル化](../../../docs/framework/misc/security-and-serialization.md)」を参照してください。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
## <a name="see-also"></a>参照

- [バイナリ シリアル化](binary-serialization.md)
- [XML シリアル化および SOAP シリアル化](xml-and-soap-serialization.md)
- [セキュリティとシリアル化](../../../docs/framework/misc/security-and-serialization.md)
