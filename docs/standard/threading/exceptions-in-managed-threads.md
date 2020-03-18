---
title: マネージド スレッドの例外
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- unhandled exceptions,in managed threads
- threading [.NET Framework],unhandled exceptions
- threading [.NET Framework],exceptions in managed threads
- managed threading
ms.assetid: 11294769-2e89-43cb-890e-ad4ad79cfbee
ms.openlocfilehash: 6c14c60b30f8f70aa5e888ed45d6f867154e18d8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159651"
---
# <a name="exceptions-in-managed-threads"></a>マネージド スレッドの例外
.NET Framework バージョン 2.0 以降では、共通言語ランタイムはスレッド内のほとんどのハンドルされない例外をそのまま続行させます。 ほとんどの場合、これはハンドルされない例外によってアプリケーションが終了することを意味します。  
  
> [!NOTE]
> これは、スレッド プールのスレッド内でのハンドルされない例外など、多数のハンドルされない例外に関する安全策を提供している、.NET Framework バージョン 1.0 および 1.1 からの重要な変更です。 このトピックの「[以前のバージョンからの変更](#ChangeFromPreviousVersions)」を参照してください。  
  
 共通言語ランタイムには、プログラム フローの制御に使用する特定のハンドルされない例外について、次のような安全策が用意されています。  
  
- <xref:System.Threading.ThreadAbortException> が呼び出されたため、スレッドで <xref:System.Threading.Thread.Abort%2A> がスローされる。  
  
- スレッドが実行中のアプリケーション ドメインがアンロードされると、スレッドで <xref:System.AppDomainUnloadedException> がスローされる。  
  
- 共通言語ランタイムまたはホスト プロセスは、内部例外をスローすることによってスレッドを終了します。  
  
 共通言語ランタイムによって作成されたスレッドでこれらの例外がハンドルされない場合、その例外によってスレッドは終了しますが、共通言語ランタイムは例外を続行させません。  
  
 メイン スレッドまたはアンマネージ コードからランタイムに入ったスレッドでこれらの例外がハンドルされない場合、例外は通常どおり続行するため、アプリケーションが終了します。  
  
> [!NOTE]
> マネージド コードが例外ハンドラーをインストールする機会を得る前に、ランタイムはハンドルされない例外をスローできます。 マネージド コードにこのような例外をハンドルする機会がない場合でも、例外を続行させることができます。  
  
## <a name="exposing-threading-problems-during-development"></a>開発時におけるスレッド処理の問題の露呈  
 アプリケーションを終了せずに、スレッドが暗黙に失敗したまま放置されていると、プログラミングの深刻な問題が検出されない状態になる可能性があります。 長期間実行されるサービスや他のアプリケーションでは、これは特に問題となります。 スレッドが失敗すると、プログラムの状態が徐々に破損します。 アプリケーションのパフォーマンスが低下、またはアプリケーションが応答しなくなる場合があります。  
  
 スレッド内でハンドルされない例外を続行させておき、結果としてオペレーティング システムにそのプログラムを終了させることで、開発およびテスト中にこのような問題が明らかになります。 プログラムの終了に関するエラー報告はデバッグをサポートします。  
  
<a name="ChangeFromPreviousVersions"></a>
## <a name="change-from-previous-versions"></a>以前のバージョンからの変更  
 最も重要な変更は、マネージド スレッドに関する変更です。 .NET Framework バージョン 1.0 および 1.1 では、共通言語ランタイムには、次の状況でのハンドルされない例外に関する安全策が用意されています。  
  
- スレッド プールのスレッドのハンドルされない例外は存在しません。 タスクが例外をスローし、その例外がハンドルされない場合、ランタイムは例外のスタック トレースをコンソールに出力し、スレッドをスレッド プールに戻します。  
  
- <xref:System.Threading.Thread.Start%2A> クラスの <xref:System.Threading.Thread> メソッドを使用して作成されたスレッドのハンドルされない例外は存在しません。 このようなスレッド上で実行中のコードが例外をスローし、その例外がハンドルされない場合、ランタイムは例外のスタック トレースをコンソールに出力し、スレッドを適切に終了します。  
  
- ファイナライザー スレッドのハンドルされない例外は存在しません。 ファイナライザーが例外をスローし、その例外がハンドルされない場合、ランタイムは例外のスタック トレースをコンソールに出力し、ファイナライザー スレッドがファイナライザーの実行を再開できるようにします。  
  
 マネージド スレッドのフォアグラウンドまたはバックグラウンドのステータスは、この動作に影響しません。  
  
 アンマネージ コードで作成されたスレッドのハンドルされない例外の場合、微妙な相違点があります。 ランタイムの JIT アタッチ ダイアログは、ネイティブ コードを通ってきたスレッド内でのマネージド例外またはネイティブ例外に関する、オペレーティング システム ダイアログより優先されます。 プロセスは常に終了します。  
  
### <a name="migrating-code"></a>コードの移行  
 通常は、この変更によってこれまでに認識されていないプログラミングの問題が明らかになるため、問題を修正できます。 ただし、プログラマがランタイムの安全策 (スレッドを終了するなど) を利用する場合もあります。 状況によっては、プログラマは次の移行方法のいずれかを検討する必要があります。  
  
- シグナルを受信したときに、スレッドが適切に終了するようにコードを再構築します。  
  
- スレッドを中止するには、<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドを使用します。  
  
- プロセスを終了できるように、スレッドを中止する必要がある場合は、スレッドをバックグラウンド スレッドにして、プロセス終了時にスレッドが自動的に終了するようにします。  
  
 どのような場合でも、方法は例外に関するデザイン ガイドラインに従う必要があります。 「[例外のデザイン ガイドライン](../../../docs/standard/design-guidelines/exceptions.md)」を参照してください。  
  
### <a name="application-compatibility-flag"></a>アプリケーション互換性フラグ  
 一時的な互換性対策として、管理者はアプリケーション構成ファイルの `<runtime>` セクションに互換性フラグを配置できます。 これにより、共通言語ランタイムをバージョン 1.0 および 1.1 の動作に戻すことができます。  
  
```xml  
<legacyUnhandledExceptionPolicy enabled="1"/>  
```  
  
## <a name="host-override"></a>ホストのオーバーライド  
 .NET Framework バージョン 2.0 では、アンマネージ ホストはホスト API の [ICLRPolicyManager](../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md) インターフェイスを使用して、共通言語ランタイムの既定のハンドルされない例外ポリシーをオーバーライドできます。 [ICLRPolicyManager::SetUnhandledExceptionPolicy](../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-setunhandledexceptionpolicy-method.md) 関数を使用して、ハンドルされない例外のポリシーを設定します。  
  
## <a name="see-also"></a>参照

- [マネージド スレッド処理の基本](../../../docs/standard/threading/managed-threading-basics.md)
