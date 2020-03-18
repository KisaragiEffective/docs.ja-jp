---
title: '軽減策: 新しい 64 ビット JIT コンパイラ'
ms.date: 03/30/2017
helpviewer_keywords:
- JIT compiler, 64-bit
- JIT compilation, 64-bit
- RyuJIT compiler
ms.assetid: 0332dabc-72c5-4bdc-8975-20d717802b17
ms.openlocfilehash: 883aaf032bde632b08f965d3450cfbea4feb8e65
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181252"
---
# <a name="mitigation-new-64-bit-jit-compiler"></a>軽減策: 新しい 64 ビット JIT コンパイラ
.NET Framework 4.6 以降では、ランタイムに Just-In-Time コンパイル用の新しい 64 ビット JIT コンパイラが含まれています。 この変更は、32 ビット JIT コンパイラでのコンパイルには影響しません。  
  
## <a name="unexpected-behavior-or-exceptions"></a>予期しない動作または例外  
 場合によっては、新しい 64 ビット JIT コンパイラでのコンパイルの結果、古い 64 ビット JIT コンパイラでコンパイルされたコードの実行時に監視されない動作が発生したり、ランタイム例外が発生したりすることがあります。 既知の相違には次のようなものがあります。  
  
> [!IMPORTANT]
> これらの既知の問題はすべて、.NET Framework 4.6.2 と共にリリースされた新しい 64 ビット コンパイラで対処済みです。 また、Windows 更新プログラムに含まれる .NET Framework 4.6 および 4.6.1 のサービス リリースでも大部分に対処済みです。 使用している Windows のバージョンが最新のものであることを確認するか、.NET Framework 4.6.2 にアップグレードすることで、これらの問題を解消できます。  
  
- 特定の条件下では、最適化が有効なリリース ビルドの <xref:System.NullReferenceException> がボックス化解除操作でスローされる場合があります。  
  
- 場合によっては、大きなメソッド本文の実働コードの実行時に <xref:System.StackOverflowException> がスローされることがあります。  
  
- 特定の条件下では、メソッドに渡された構造体が、リリース ビルドの値の型ではなく、参照型として扱われます。 この問題の兆候の 1 つは、コレクション内の個々の項目が予期しない順序で表示されることです。  
  
- 特定の条件下では、最適化が有効な場合、高ビットが設定された <xref:System.UInt16> 値が正しく比較されません。  
  
- 特定の条件下では、特に、配列値の初期化中に、<xref:System.Reflection.Emit.OpCodes.Initblk?displayProperty=nameWithType> IL 命令でのメモリ初期化により、正しくない値でメモリが初期化される場合があります。 その結果、ハンドルされない例外または正しくない出力が発生する場合があります。  
  
- 特定のまれな条件下では、コンパイラの最適化が有効な場合に、条件付きのビット テストで正しくない <xref:System.Boolean> 値が返されたり、例外がスローされたりすることがあります。  
  
- 特定の条件下では、`if` ステートメントを使用して、`try` ブロックの開始前と `try` ブロックの終了時の条件をテストし、同じ条件を `catch` または `finally` ブロックで評価する場合、新しい 64 ビット JIT コンパイラが、コードの最適化の際に `if` または `catch` ブロックから `finally` 条件を削除します。 その結果、`if` または `catch` ブロックの `finally` ステートメント内のコードは無条件で実行されます。  
  
<a name="General"></a>
## <a name="mitigation-of-known-issues"></a>既知の問題の軽減策  
 上記の問題が発生する場合は、次のいずれかの方法で解決できます。  
  
- .NET Framework 4.6.2 にアップグレードします。 .NET Framework 4.6.2 に含まれている新しい 64 ビット コンパイラは、これらの既知の問題のそれぞれに対処します。  
  
- Windows Update を実行して、Windows のバージョンが最新のものであることを確認します。 .NET Framework 4.6 および 4.6.1 へのサービス更新により、ボックス化解除操作での <xref:System.NullReferenceException> を除き、これらの問題のそれぞれに対処することができます。  
  
- 古い 64 ビット JIT コンパイラでコンパイルします。 この方法の詳細については、「[その他の問題の軽減策](#Other)」を参照してください。  
  
<a name="Other"></a>
## <a name="mitigation-of-other-issues"></a>その他の問題の軽減策  
 古い 64 ビット コンパイラと新しい 64 ビット JIT コンパイラでコンパイルされたコードの動作、またはアプリのデバッグ バージョンとリリース バージョン (両方とも新しい 64 ビット JIT コンパイラでコンパイル) の動作に違いが見られる場合は、次のようにして、古い 64 ビット JIT コンパイラでアプリをコンパイルすることができます。  
  
- アプリケーションごとに、アプリケーションの構成ファイルに [\<useLegacyJit>](../configure-apps/file-schema/runtime/uselegacyjit-element.md) 要素を追加できます。 次のように新しい 64 ビット JIT コンパイラでのコンパイルを無効にし、代わりに従来の 64 ビット JIT コンパイラを使用します。  
  
    ```xml  
    <?xml version ="1.0"?>  
    <configuration>  
        <runtime>  
           <useLegacyJit enabled="1" />  
        </runtime>  
    </configuration>  
    ```  
  
- ユーザーごとに、`REG_DWORD` という `useLegacyJit` 値をレジストリの `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` キーに追加できます。 値を 1 にすると、従来の 64 ビット JIT コンパイラが有効になり、値を 0 にすると、従来の 64 ビット JIT コンパイラが無効になり、新しい 64 ビット JIT コンパイラが有効になります。  
  
- コンピューターごとに、`REG_DWORD` という名前の `useLegacyJit` 値をレジストリの `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` キーに追加できます。 値を 1 にすると、従来の 64 ビット JIT コンパイラが有効になり、値を 0 にすると、従来の 64 ビット JIT コンパイラが無効になり、新しい 64 ビット JIT コンパイラが有効になります。  
  
 [Microsoft Connect](https://connect.microsoft.com/VisualStudio) でバグを報告し、問題を弊社に知らせることもできます。  
  
## <a name="see-also"></a>参照

- [アプリケーションの互換性](application-compatibility.md)
- [\<useLegacyJit> 要素](../configure-apps/file-schema/runtime/uselegacyjit-element.md)
