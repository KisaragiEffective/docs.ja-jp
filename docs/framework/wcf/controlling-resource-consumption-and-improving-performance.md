---
title: リソース消費の制御とパフォーマンスの向上
ms.date: 03/30/2017
ms.assetid: 9a829669-5f76-4c88-80ec-92d0c62c0660
ms.openlocfilehash: 16d6f29235455ff30e115b7aff3425412bc7ba6a
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802256"
---
# <a name="controlling-resource-consumption-and-improving-performance"></a>リソース消費の制御とパフォーマンスの向上
このトピックでは、Windows Communication Foundation (WCF) アーキテクチャのさまざまな領域におけるさまざまなプロパティについて説明します。このアーキテクチャでは、リソースの消費量を制御し、パフォーマンスメトリックに影響を与えます。

## <a name="properties-that-constrain-resource-consumption-in-wcf"></a>WCF でのリソース消費を制約するプロパティ
 Windows Communication Foundation (WCF) では、セキュリティとパフォーマンスの両方の目的で、特定の種類のプロセスに制約が適用されます。 これらの制約には、クォータとスロットルという 2 つの主要な形式があります。 *クォータ*とは、システム内のある時点で、トリガーが即座に例外になることを示す制限です。 *スロットル*とは、すぐに例外がスローされないようにする制限です。 スロットル制限に達すると、例外がスローされる代わりに、そのスロットル値によって設定された制限の範囲内で処理が続行されます。 この制限された処理によって他の場所で例外が発生する可能性がありますが、これはアプリケーションに依存します。

 クォータとスロットルの違いに加え、シリアル化レベル、トランスポート レベル、およびアプリケーション レベルにも制約を加えるプロパティがあります。 たとえば、システム提供のすべてのトランスポート バインディング要素によって実装されるクォータである <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A?displayProperty=nameWithType> は、既定で 65,536 バイトに設定されます。これは、悪意のあるクライアントがサービスに対して、大量のメモリを消費させるサービス拒否攻撃を実行することを防ぐための措置です (通常は、この値を下げることによってパフォーマンスを向上できます)。

 シリアル化のクォータの例としては、<xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A?displayProperty=nameWithType> プロパティがあります。このプロパティは、シリアライザーが <xref:System.Runtime.Serialization.DataContractSerializer.ReadObject%2A> メソッドの 1 回の呼び出しでシリアル化または逆シリアル化するオブジェクトの最大数を指定します。 アプリケーション レベルのスロットルの例としては、<xref:System.ServiceModel.Dispatcher.ServiceThrottle.MaxConcurrentSessions%2A?displayProperty=nameWithType> プロパティがあります。このプロパティは、既定で、セッションフル チャネルの同時接続の最大数を 10 に制限します (クォータとは異なり、このスロットル値に達しても、アプリケーションは処理を続行しますが、新しいセッションフル チャネルは受け入れません。つまり、他のセッションフル チャネルのいずれかが終了するまで、新しいクライアントは接続できません)。

 これらの制御は、特定の種類の攻撃に対してすぐに使用できる軽減策を提供し、メモリの占有領域や起動時間などのパフォーマンス メトリックを向上するように設計されています。 ただし、アプリケーションによっては、これらの制御によってサービス アプリケーションのパフォーマンスが低下したり、アプリケーションがまったく動作しなくなったりする可能性があります。 たとえば、ビデオ ストリーミング用に設計されたアプリケーションは、既定の <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A?displayProperty=nameWithType> プロパティをすぐに超える可能性があります。 このトピックでは、WCF のすべてのレベルでアプリケーションに適用されるさまざまなコントロールの概要を示し、設定がアプリケーションに妨げられるかどうかに関する詳細情報を取得するさまざまな方法、およびさまざまな問題を修正する方法について説明します。 ほとんどのスロットルおよび一部のクォータは、基本プロパティがシリアル化またはトランスポートの制約である場合でも、アプリケーション レベルで利用できます。 たとえば、サービス クラスの <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A?displayProperty=nameWithType> を使用して、<xref:System.ServiceModel.ServiceBehaviorAttribute.MaxItemsInObjectGraph%2A?displayProperty=nameWithType> プロパティを設定できます。

> [!NOTE]
> 特定の問題が発生した場合は、まず、 [WCF トラブルシューティングクイックスタート](wcf-troubleshooting-quickstart.md)を読んで、問題 (および解決策) がそこに表示されているかどうかを確認する必要があります。

 シリアル化プロセスを制限するプロパティは、「[データのセキュリティに関する考慮事項](./feature-details/security-considerations-for-data.md)」に記載されています。 トランスポートに関連するリソースの消費を制限するプロパティについては、「[トランスポートクォータ](./feature-details/transport-quotas.md)」を参照してください。 アプリケーション層でリソースの消費を制限するプロパティは、<xref:System.ServiceModel.Dispatcher.ServiceThrottle> クラスのメンバーです。

## <a name="detecting-application-and-performance-issues-related-to-quota-settings"></a>クォータ設定に関連するアプリケーションとパフォーマンスの問題の検出
 上記の値の既定値は、一般的なセキュリティの問題に対する基本的な保護を提供しながら、さまざまなアプリケーションで基本的なアプリケーション機能を使用できるように選択されたものです。 ただし、アプリケーション デザインによっては、1 つ以上のスロットル設定を超えてしまったためにアプリケーションがセキュリティで保護されなかったり、設計どおりに動作しなかったりする場合があります。 その場合は、超過したスロットル値とそのレベルを特定し、アプリケーションのスループットを向上するための適切な手順を決定する必要があります。

 通常、アプリケーションを作成してデバッグする際には、構成ファイルまたはプログラムで <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> プロパティを `true` に設定します。 これにより、WCF は、サービス例外スタックトレースを表示するためにクライアントアプリケーションに返すように指示します。 この機能により、ほとんどのアプリケーション レベルの例外が報告され、問題が発生している場合に、どのクォータ設定が関係しているかを表示できます。

 実行時に発生する一部の例外はアプリケーション層から見えないため、このメカニズムでは返されません。このため、カスタムの <xref:System.ServiceModel.Dispatcher.IErrorHandler?displayProperty=nameWithType> 実装で処理できないことがあります。 Microsoft Visual Studio などの開発環境で作業している場合は、これらの例外のほとんどは自動的に表示されます。 ただし、一部の例外は、[マイコードのみ](/visualstudio/debugger/just-my-code)Visual Studio などの開発環境の設定によってマスクされる場合があります。

 開発環境の機能に関係なく、WCF トレースとメッセージログの機能を使用して、すべての例外をデバッグし、アプリケーションのパフォーマンスを調整することができます。 詳細については、「[トレースを使用したアプリケーションのトラブルシューティング](./diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)」を参照してください。

## <a name="performance-issues-and-xmlserializer"></a>パフォーマンスの問題と XmlSerializer
 <xref:System.Xml.Serialization.XmlSerializer> を使用してシリアル化できるデータ型を使用するサービスおよびクライアント アプリケーションは、実行時にこのようなデータ型のシリアル化コードを生成およびコンパイルします。このため、起動時のパフォーマンスが低下することがあります。

> [!NOTE]
> 生成済みシリアル化コードはクライアント アプリケーションでのみ使用できます。サービスでは使用できません。

 [ServiceModel メタデータユーティリティツール (svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)を使用すると、アプリケーションのコンパイル済みアセンブリから必要なシリアル化コードを生成することで、これらのアプリケーションの起動時のパフォーマンスを向上させることができます。 詳細については、「[方法: XmlSerializer を使用して WCF クライアントアプリケーションの起動時間を向上させる](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)」を参照してください。

## <a name="performance-issues-when-hosting-wcf-services-under-aspnet"></a>ASP.NET で WCF サービスをホストする場合のパフォーマンスの問題

WCF サービスを IIS および ASP.NET でホストする場合、IIS と ASP.NET の構成設定が WCF サービスのスループットやメモリの占有領域に影響する場合があります。  ASP.NET のパフォーマンスの詳細については、「 [ASP.NET パフォーマンスの向上](https://docs.microsoft.com/previous-versions/msp-n-p/ff647787(v=pandp.10))」を参照してください。 予想外の結果を引き起こす可能性のある設定の 1 つに、<xref:System.Web.Configuration.ProcessModelSection.MinWorkerThreads%2A> があります。これは、<xref:System.Web.Configuration.ProcessModelSection> のプロパティです。 アプリケーションのクライアントが固定数または少数である場合、<xref:System.Web.Configuration.ProcessModelSection.MinWorkerThreads%2A> を 2 に設定すると、CPU の使用率が 100% に近いマルチプロセッサ コンピューターのスループットが向上する場合があります。 このパフォーマンスの向上にはコストが伴います。つまり、メモリの使用率も増加するため、スケーラビリティが低下する場合があります。

## <a name="see-also"></a>参照

- [管理と診断](./diagnostics/index.md)
- [大規模データとストリーミング](./feature-details/large-data-and-streaming.md)
