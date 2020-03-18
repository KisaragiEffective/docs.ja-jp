---
title: コールバック メソッドとしてのデリゲートのマーシャ リング
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data marshaling, Callback sample
- marshaling, Callback sample
ms.assetid: 6ddd7866-9804-4571-84de-83f5cc017a5a
ms.openlocfilehash: c71c89e5797745144a2baed2d4846e3d9f9f26be
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73114013"
---
# <a name="marshaling-a-delegate-as-a-callback-method"></a>コールバック メソッドとしてのデリゲートのマーシャ リング
このサンプルでは、関数ポインターを要求するアンマネージ関数にデリゲートを渡す方法を示します。 デリゲートは、メソッドへの参照を保持できるクラスであり、タイプ セーフな関数ポインターまたはコールバック関数と同等のものです。

> [!NOTE]
> 呼び出しの中でデリゲートを使うと、共通言語ランタイムがデリゲートをその呼び出しの期間中ガベージ コレクションから保護します。 ただし、アンマネージ関数が呼び出し完了後に使うためにデリゲートを保存する場合は、アンマネージ関数がデリゲートを終了するまで手動でガベージ コレクションを防ぐ必要があります。 詳細については、「[HandleRef Sample](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hc662t8k(v=vs.100))」(HandleRef のサンプル) および「[GCHandle Sample](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/44ey4b32(v=vs.100))」(GCHandle のサンプル) をご覧ください。

Callback のサンプルで使用するアンマネージ関数とその元の関数宣言を次に示します。

- PinvokeLib.dll からエクスポートされる `TestCallBack`。

    ```cpp
    void TestCallBack(FPTR pf, int value);
    ```

- PinvokeLib.dll からエクスポートされる `TestCallBack2`。

    ```cpp
    void TestCallBack2(FPTR2 pf2, char* value);
    ```

[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) はカスタム アンマネージ ライブラリであり、上で示した関数の実装を含んでいます。

このサンプルでは、`NativeMethods` クラスには、`TestCallBack` メソッドと `TestCallBack2` メソッドのマネージド プロトタイプが含まれます。 どちらのメソッドも、コールバック関数にパラメーターとしてデリゲートを渡します。 デリゲートのシグネチャは、それが参照しているメソッドのシグネチャと一致する必要があります。 たとえば、`FPtr` および `FPtr2` デリゲートのシグネチャは、`DoSomething` および `DoSomething2` メソッドと同じです。

## <a name="declaring-prototypes"></a>プロトタイプの宣言
[!code-cpp[Conceptual.Interop.Marshaling#37](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/callback.cpp#37)]
[!code-csharp[Conceptual.Interop.Marshaling#37](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/callback.cs#37)]
[!code-vb[Conceptual.Interop.Marshaling#37](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/callback.vb#37)]

## <a name="calling-functions"></a>関数の呼び出し
[!code-cpp[Conceptual.Interop.Marshaling#38](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/callback.cpp#38)]
[!code-csharp[Conceptual.Interop.Marshaling#38](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/callback.cs#38)]
[!code-vb[Conceptual.Interop.Marshaling#38](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/callback.vb#38)]

## <a name="see-also"></a>関連項目

- [各種のマーシャリングのサンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ss9sb93t(v=vs.100))
- [プラットフォーム呼び出しのデータ型](marshaling-data-with-platform-invoke.md#platform-invoke-data-types)
- [マネージド コードでのプロトタイプの作成](creating-prototypes-in-managed-code.md)
