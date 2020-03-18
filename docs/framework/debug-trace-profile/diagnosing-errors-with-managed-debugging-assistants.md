---
title: マネージド デバッグ アシスタントによるエラーの診断
ms.date: 08/14/2018
f1_keywords:
- EHMDA
helpviewer_keywords:
- run-time error debugging
- managed code, run-time debugging
- resource debugging
- registry, MDAs
- common language runtime, debugging
- MDAs (managed debugging assistants)
- configuration files [.NET Framework], debugging runtime events
- messages, managed debugging assistants
- application configuration files, managed debugging assistants
- debugging [.NET Framework], managed debugging assistants
- environment variables, MDAs
- access violation debugging [.NET Framework]
- diagnostics, managed debugging assistants
- unmanaged code, run-time debugging
- default debug output stream
- memory, debugging
- app.config files, managed debugging assistants
- managed debugging assistants (MDAs)
- debugging [.NET Framework], run-time errors
- trapping events
- runtime, error debugging
- disabling MDAs
- output, managed debugging assistants
- errors [.NET Framework], managed debugging assistants
ms.assetid: 76994ee6-9fa9-4059-b813-26578d24427c
ms.openlocfilehash: 712fbbe9e0ad291385e8eef321c5e8a2fa092a5d
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216558"
---
# <a name="diagnose-errors-with-managed-debugging-assistants"></a>マネージデバッグアシスタントを使用してエラーを診断する

マネージド デバッグ アシスタント (MDA) は、共通言語ランタイム (CLR: Common Language Runtime) と連携してランタイム状態に関する情報を提供するデバッグ支援ツールです。 MDA は、これ以外の方法ではトラップできないランタイム イベントに関する情報メッセージを生成します。 MDA を使用すると、マネージド コードからアンマネージド コードへの遷移時に発生する、検出が難しいアプリケーション バグを分離できます。

すべての Mda を[有効または無効](#enable-and-disable-mdas)にするには、Windows レジストリにキーを追加するか、環境変数を設定します。 特定の MDA を有効にするには、アプリケーション構成設定を使用します。 一部の MDA については、アプリケーションの構成ファイルで追加の構成設定を個別に設定できます。 この構成ファイルはランタイムの読み込み時に解析されるため、MDA は、マネージド アプリケーションが起動する前に有効にする必要があります。 MDA は、既に起動しているアプリケーションに対して有効にできません。

次の表に、.NET Framework に付属している Mda を示します。

|||
|-|-|
|[asynchronousThreadAbort](asynchronousthreadabort-mda.md)|[bindingFailure](bindingfailure-mda.md)|
|[callbackOnCollectedDelegate](callbackoncollecteddelegate-mda.md)|[contextSwitchDeadlock](contextswitchdeadlock-mda.md)|
|[dangerousThreadingAPI](dangerousthreadingapi-mda.md)|[dateTimeInvalidLocalFormat](datetimeinvalidlocalformat-mda.md)|
|[dirtyCastAndCallOnInterface](dirtycastandcalloninterface-mda.md)|[disconnectedContext](disconnectedcontext-mda.md)|
|[dllMainReturnsFalse](dllmainreturnsfalse-mda.md)|[exceptionSwallowedOnCallFromCom](exceptionswallowedoncallfromcom-mda.md)|
|[failedQI](failedqi-mda.md)|[fatalExecutionEngineError](fatalexecutionengineerror-mda.md)|
|[gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)|[gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)|
|[illegalPrepareConstrainedRegion](illegalprepareconstrainedregion-mda.md)|[invalidApartmentStateChange](invalidapartmentstatechange-mda.md)|
|[invalidCERCall](invalidcercall-mda.md)|[invalidFunctionPointerInDelegate](invalidfunctionpointerindelegate-mda.md)|
|[invalidGCHandleCookie](invalidgchandlecookie-mda.md)|[invalidIUnknown](invalidiunknown-mda.md)|
|[invalidMemberDeclaration](invalidmemberdeclaration-mda.md)|[invalidOverlappedToPinvoke](invalidoverlappedtopinvoke-mda.md)|
|[invalidVariant](invalidvariant-mda.md)|[jitCompilationStart](jitcompilationstart-mda.md)|
|[loaderLock](loaderlock-mda.md)|[loadFromContext](loadfromcontext-mda.md)|
|[marshalCleanupError](marshalcleanuperror-mda.md)|[marshaling](marshaling-mda.md)|
|[memberInfoCacheCreation](memberinfocachecreation-mda.md)|[moduloObjectHashcode](moduloobjecthashcode-mda.md)|
|[nonComVisibleBaseClass](noncomvisiblebaseclass-mda.md)|[notMarshalable](notmarshalable-mda.md)|
|[openGenericCERCall](opengenericcercall-mda.md)|[overlappedFreeError](overlappedfreeerror-mda.md)|
|[pInvokeLog](pinvokelog-mda.md)|[pInvokeStackImbalance](pinvokestackimbalance-mda.md)|
|[raceOnRCWCleanup](raceonrcwcleanup-mda.md)|[reentrancy](reentrancy-mda.md)|
|[releaseHandleFailed](releasehandlefailed-mda.md)|[reportAvOnComRelease](reportavoncomrelease-mda.md)|
|[streamWriterBufferedDataLost](streamwriterbuffereddatalost-mda.md)|[virtualCERCall](virtualcercall-mda.md)|

既定では、.NET Framework はすべてのマネージド デバッガーに対して MDA のサブセットをアクティブにします。 Visual Studio で既定の設定を表示するには、 **[デバッグ]** メニューの [ **Windows** > **例外設定**] を選択し、 **[マネージデバッグアシスタント]** の一覧を展開します。

![Visual Studio の [例外設定] ウィンドウ](./media/diagnosing-errors-with-managed-debugging-assistants/exception-settings-mdas.png)

## <a name="enable-and-disable-mdas"></a>Mda を有効または無効にする

MDA は、レジストリ キー、環境変数、およびアプリケーション構成設定を使用して有効または無効にできます。 アプリケーション構成設定を使用するには、レジストリ キーまたは環境変数を有効にする必要があります。

> [!TIP]
> Mda を無効にする代わりに、MDA の通知を受信するたびに、Visual Studio が MDA ダイアログボックスを表示しないようにすることができます。 これを行うには、 **[デバッグ]** メニューの [ **Windows** > **例外設定**] をクリックし、 **[マネージデバッグアシスタント]** の一覧を展開して、個々の MDA に対して スローされ **[たときにブレーク]** チェックボックスをオンまたはオフにします。

### <a name="registry-key"></a>レジストリ キー

Mda を有効にするには、 **HKEY_LOCAL_MACHINE \software\microsoft\\を追加します。Windows レジストリの Netframework\ MDA**サブキー (型 REG_SZ、値 1)。 次の例を、 *Mdaenable*という名前のテキストファイルにコピーします。Windows レジストリエディター (Regedit.exe) を開き、 **[ファイル]** メニューの **[インポート]** をクリックします。 *Mdaenable .reg*ファイルを選択して、そのコンピューターで mda を有効にします。 サブキーを**1**に設定すると (DWORD 値が**1**ではない)、 *ApplicationName*... .config ファイルから mda 設定を読み取ることができます。 たとえば、メモ帳の MDA 構成ファイルには、notepad.exe という名前が付けられます。

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework]
"MDA"="1"
```

64 ビット オペレーティング システム上で 32 ビット アプリケーションを実行しているコンピューターでは、MDA キーは次のように設定します。

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework]
"MDA"="1"
```

詳細については[、「アプリケーション固有の構成設定](#application-specific-configuration-settings)」を参照してください。 レジストリ設定は、COMPLUS_MDA 環境変数でオーバーライドできます。 詳細については、「[環境変数](#environment-variable)」を参照してください。

Mda を無効にするには、Windows レジストリエディターを使用して、MDA サブキーを**0** (ゼロ) に設定します。

MDA には、レジストリ キーを追加しなくても、デバッガーにアタッチされているアプリケーションを実行すると既定で有効になるものがあります。 これらのアシスタントを無効にするには、このセクションで前述したように*Mdadisable .reg*ファイルを実行します。

### <a name="environment-variable"></a>環境変数

MDA のアクティブ化は、COMPLUS_MDA 環境変数によって制御することもできます。この環境変数はレジストリ キーをオーバーライドします。 COMPLUS_MDA の文字列は、MDA 名やその他の特殊制御文字列の、セミコロンで区切られたリストで、大文字小文字の区別はありません。 マネージド デバッガーやアンマネージド デバッガーの下で起動すると、MDA のセットが既定で有効になります。 そのためには、デバッガーの下で既定で有効にする MDA のリスト (セミコロン区切り) を、環境変数またはレジストリ キーの値の前に暗黙的に付加します。 特殊制御文字列は次のとおりです。

- `0` - すべての MDA を非アクティブにします。

- `1` - *ApplicationName*.mda.config から MDA の設定を読み取ります。

- `managedDebugger` - デバッガーの下でマネージド実行可能ファイルを起動すると、暗黙的にアクティブ化されているすべての MDA が明示的にアクティブ化されます。

- `unmanagedDebugger` - デバッガーの下でアンマネージ実行可能ファイルを起動すると、暗黙的にアクティブ化されているすべての MDA が明示的にアクティブ化されます。

競合する設定がある場合は、最新の設定が以前の設定を次のようにオーバーライドします。

- `COMPLUS_MDA=0` は、デバッガーの下で暗黙的に有効化されている MDA を含め、すべての MDA を無効にします。

- `COMPLUS_MDA=gcUnmanagedToManaged` は、デバッガーの下で暗黙的に有効化される MDA に加えて `gcUnmanagedToManaged` も有効にします。

- `COMPLUS_MDA=0;gcUnmanagedToManaged` は `gcUnmanagedToManaged` を有効にしますが、デバッガーの下で別途暗黙的に有効化されている MDA を無効にします。

### <a name="application-specific-configuration-settings"></a>アプリケーション固有の構成設定

アプリケーションの MDA 構成ファイルでは、一部のアシスタントを個別に有効化、無効化、および構成できます。 MDA を構成する目的でアプリケーション構成ファイルの使用を有効にするには、MDA レジストリ キーまたは COMPLUS_MDA 環境変数を設定する必要があります。 アプリケーション構成ファイルは、通常、アプリケーションの実行可能ファイル (.exe) と同じディレクトリに置かれます。 ファイル名の形式は*ApplicationName*. mda. .config; です。たとえば、notepad.exe のように設定します。アプリケーション構成ファイルで有効になっているアシスタントには、そのアシスタントの動作を制御するために特別に設計された属性または要素が含まれている場合があります。

次の例は、[マーシャリング](marshaling-mda.md)を有効化および構成する方法を示しています。

```xml
<mdaConfig>
  <assistants>
    <marshaling>
      <methodFilter>
        <match name="*"/>
      </methodFilter>
      <fieldFilter>
        <match name="*"/>
      </fieldFilter>
    </marshaling>
  </assistants>
</mdaConfig>
```

`Marshaling` MDA では、アプリケーションでのマネージド コードからアンマネージド コードへの遷移ごとに、アンマネージド型にマーシャリングされるマネージド型についての情報が出力されます。 `Marshaling` MDA では、 **methodfilter**子要素および**fieldfilter**子要素で提供されるメソッドと構造体のフィールドの名前をそれぞれフィルター処理することもできます。

次の例では、既定の設定を使用して複数の Mda を有効にする方法を示します。

```xml
<mdaConfig>
  <assistants>
    <illegalPrepareConstrainedRegion />
    <invalidCERCall />
    <openGenericCERCall />
    <virtualCERCall />
  </assistants>
</mdaConfig>
```

> [!IMPORTANT]
> 構成ファイルに複数のアシスタントを指定する場合は、アルファベット順に記述する必要があります。 たとえば、`virtualCERCall` MDA と `invalidCERCall` MDA の両方を有効にする場合は、`<invalidCERCall />` エントリ、`<virtualCERCall />` エントリの順に追加する必要があります。 エントリがアルファベット順になっていない場合、ハンドルされない無効な構成ファイルであることを示す例外メッセージが表示されます。

## <a name="mda-exceptions"></a>MDA の例外

MDA が有効になっている場合、デバッガーでコードが実行されていなくても、アクティブになります。 デバッガーが存在しない場合に MDA イベントが発生した場合、そのイベントはハンドルされない例外とは異なりますが、イベント メッセージはハンドルされない例外のダイアログ ボックスに表示されます。 このダイアログ ボックスが表示されないようにするには、デバッグ環境でコードを実行しているのではないときに、MDA を有効にする設定を削除します。

Visual Studio 統合開発環境 (IDE: integrated development environment) でコードを実行すると、特定の MDA イベントに対して表示される [例外] ダイアログボックスを使用しないようにすることができます。 これを行うには、 **[デバッグ]** メニューの [ **Windows** > の**例外設定**] をクリックします。 **[例外設定]** ウィンドウで、 **[マネージデバッグアシスタント]** の一覧を展開し、個々の MDA に対して スローされ **[たときにブレークする]** チェックボックスをオフにします。 また、このダイアログボックスを使用して、MDA の例外ダイアログボックスの表示を*有効*にすることもできます。

## <a name="mda-output"></a>MDA の出力

MDA の出力は次の例のようになり、`PInvokeStackImbalance` MDA からの出力が表示されます。

**PInvoke 関数 ' MDATest! ' を呼び出しています。MDATest. Program:: StdCall ' がスタックを不均衡にしました。マネージ PInvoke 署名がアンマネージターゲットシグネチャと一致しないことが原因である可能性があります。PInvoke 署名の呼び出し規約とパラメーターが、ターゲットのアンマネージシグネチャと一致することを確認します。**

## <a name="see-also"></a>参照

- [デバッグ、トレース、およびプロファイリング](index.md)
