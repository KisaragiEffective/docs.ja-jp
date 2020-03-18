---
title: '方法 : トレース スイッチを作成、初期化、および構成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- trace switches, configuring
- tracing [.NET Framework], trace switches
- switches, trace
- tracing [.NET Framework], enabling or disabling
- Web.config configuration file, trace switches
ms.assetid: 5a0e41bf-f99c-4692-8799-f89617f5bcf9
ms.openlocfilehash: 358e34b2ce5d896ba02b343ce060604f2d42eeeb
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216473"
---
# <a name="how-to-create-initialize-and-configure-trace-switches"></a>方法 : トレース スイッチを作成、初期化、および構成する
トレース スイッチを使用すると、トレース出力を有効/無効にしたり、トレースの出力をフィルター処理したりできます。  
  
<a name="create"></a>   
## <a name="creating-and-initializing-a-trace-switch"></a>トレース スイッチの作成と初期化  
 トレース スイッチを使用するには、まずトレース スイッチを作成し、コード内に配置する必要があります。 事前定義されている 2 つのクラスからスイッチ オブジェクトを作成することができます。それは、<xref:System.Diagnostics.BooleanSwitch?displayProperty=nameWithType> クラスと <xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType> クラスです。 トレース メッセージを表示させるかどうかのみ処理すればよい場合は <xref:System.Diagnostics.BooleanSwitch> を使用し、トレースのレベルを区別する必要がある場合は <xref:System.Diagnostics.TraceSwitch> を使用します。 <xref:System.Diagnostics.TraceSwitch> を使用する場合は、独自のデバッグ メッセージを定義して、それらのメッセージを異なるトレース レベルに関連付けることができます。 どちらの種類のスイッチも、トレースまたはデバッグの両方で使用できます。 既定では、<xref:System.Diagnostics.BooleanSwitch> は無効に設定され、<xref:System.Diagnostics.TraceSwitch> はレベル <xref:System.Diagnostics.TraceLevel.Off?displayProperty=nameWithType> に設定されています。 トレース スイッチは、それらを使用する可能性のあるコードの任意の部分に作成して配置することができます。  
  
 トレース レベルやその他の構成オプションをコード内に設定することもできますが、構成ファイルを使用してスイッチの状態を管理することをお勧めします。 これは、構成システムでスイッチの構成を管理したほうが柔軟性が高く、アプリケーションを再コンパイルしないでもさまざまなスイッチのオン/オフを切り替えたりレベルを変更したりできるからです。  
  
#### <a name="to-create-and-initialize-a-trace-switch"></a>トレース スイッチを作成し、初期化するには  
  
1. スイッチを型 <xref:System.Diagnostics.BooleanSwitch?displayProperty=nameWithType> または型 <xref:System.Diagnostics.TraceSwitch?displayProperty=nameWithType> のいずれかに定義し、スイッチの名前と説明を設定します。  
  
2. トレース スイッチを構成します。 詳細については、「[トレース スイッチの構成](#configure)」を参照してください。  
  
     次のコードでは、それぞれの種類のスイッチを 1 つずつ作成します。  
  
    ```vb  
    Dim dataSwitch As New BooleanSwitch("Data", "DataAccess module")  
    Dim generalSwitch As New TraceSwitch("General", "Entire application")  
    ```  
  
    ```csharp  
    System.Diagnostics.BooleanSwitch dataSwitch =   
       new System.Diagnostics.BooleanSwitch("Data", "DataAccess module");  
    System.Diagnostics.TraceSwitch generalSwitch =   
       new System.Diagnostics.TraceSwitch("General",   
       "Entire application");  
    ```  
  
<a name="configure"></a>   
## <a name="configuring-trace-switches"></a>トレース スイッチの構成  
 アプリケーションを配布した後も、引き続き、アプリケーション内のトレース スイッチを構成することによりトレース出力を有効/無効にできます。 この場合、"スイッチを構成する" とは、初期化された後に外部ソースからその値を変更することを意味します。 構成ファイルを使用してスイッチ オブジェクトの値を変更することができます。 トレース スイッチを構成すると、スイッチのオン/オフの切り替えやレベル設定を行えるほか、リスナーに引き渡すメッセージの量と種類を決定できます。  
  
 スイッチは、.config ファイルを使用して構成されています。 Web アプリケーションでは、これは、プロジェクトに関連付けられた Web.config ファイルです。 Windows アプリケーションでは、このファイルには (application name) .exe という名前が付けられます。配置されたアプリケーションでは、このファイルは実行可能ファイルと同じフォルダーに存在する必要があります。  
  
 アプリケーションが、スイッチのインスタンスを作成するコードを初めて実行したときに、構成ファイルを確認して名前付きのスイッチに関するトレース レベルの情報を取得します。 トレース システムが特定のスイッチに関して構成ファイルを調べるのは、アプリケーションが初めてスイッチを作成するとき 1 回のみです。  
  
 配置されたアプリケーションでは、アプリケーションが実行されていないときにスイッチ オブジェクトを再構成することによりトレース コードを有効にできます。 この手順は、通常、スイッチ オブジェクトのオン/オフの切り替えやトレース レベルの変更を行った後、アプリケーションを再起動します。  
  
 スイッチのインスタンスを作成するときに、*displayName* 引数と *description* 引数の 2 つを指定することで、インスタンスを初期化することもできます。 コンストラクターの *displayName* 引数は、<xref:System.Diagnostics.Switch.DisplayName%2A?displayProperty=nameWithType> クラス インスタンスの <xref:System.Diagnostics.Switch> プロパティを設定します。 *displayName* は .config ファイルでスイッチを構成するために使用されている名前です。*description* 引数は、スイッチとそれがコントロールするメッセージについて簡単な説明を返します。  
  
 構成するスイッチの名前を指定するだけでなく、スイッチの値も指定する必要があります。 この値は整数です。 <xref:System.Diagnostics.BooleanSwitch> では、値 0 が**オフ**に相当し、0 以外の値が**オン**に相当します。 <xref:System.Diagnostics.TraceSwitch> では、0、1、2、3、および 4 が、それぞれ**オフ**、**エラー**、**警告**、**情報**、および**詳細**に相当します。 4 より大きい数値は**詳細**として扱われ、0 より小さい数値は**オフ**として扱われます。  
  
> [!NOTE]
> .NET Framework バージョン 2.0 では、スイッチの値を指定するためにテキストを使用できます。 たとえば、`true` に対して <xref:System.Diagnostics.BooleanSwitch> を指定したり、`Error` に対して列挙値のテキスト形式である <xref:System.Diagnostics.TraceSwitch> を指定したりできます。 `<add name="myTraceSwitch" value="Error" />` という行は、`<add name="myTraceSwitch" value="1" />` と同じです。  
  
 エンドユーザーがアプリケーションのトレース スイッチを構成できるようにするため、アプリケーションのスイッチに関する詳細なドキュメントを提供する必要があります。 どのスイッチで何を制御するのか、およびスイッチのオン/オフを切り替える方法について詳しく説明してください。 また、.config ファイルのコメントに適切なヘルプを記載して、エンド ユーザーに提供する必要があります。  
  
#### <a name="to-configure-trace-switches"></a>トレース スイッチを構成する  
  
1. トレース スイッチを使用するには、「[Creating and initializing a trace switch](#create)」(トレース スイッチの作成と初期化) のセクションで説明されているように、まず、それらを作成してコードに配置する必要があります。  
  
2. プロジェクトに構成ファイル (app.config または Web.config) が含まれない場合は、 **[プロジェクト]** メニューから **[新しい項目の追加]** を選択します。  
  
    - **Visual Basic:** **[新しい項目の追加]** ダイアログ ボックスで **[アプリケーション構成ファイル]** を選択します。  
  
         アプリケーション構成ファイルが作成され、開かれます。 これは、XML ドキュメントであり、ルート要素は `<configuration>.` です。  
  
    - **Visual C# :** **[新しい項目の追加]** ダイアログ ボックスで **[XML ファイル]** を選択します。 このファイルに app.config という名前を指定**します。** Xml エディターで、XML 宣言の後に次の XML を追加します。  
  
        ```xml  
        <configuration>  
        </configuration>  
        ```  
  
         プロジェクトをコンパイルすると、app.config ファイルがプロジェクトの出力フォルダーにコピーされ、名前が *applicationname*.exe.config に変更されます。  
  
3. `<configuration>` タグの後、かつ `</configuration>` タグの前に、スイッチを構成する適切な XML を追加します。 次の例では、**DisplayName** プロパティに **を設定した**BooleanSwitch`DataMessageSwitch` と、**DisplayName** プロパティに **を設定した**TraceSwitch`TraceLevelSwitch` を示します。  
  
    ```xml  
    <system.diagnostics>  
       <switches>  
          <add name="DataMessagesSwitch" value="0" />  
          <add name="TraceLevelSwitch" value="0" />  
       </switches>  
    </system.diagnostics>  
    ```  
  
     この構成では、両方のスイッチがオフになっています。  
  
4. **BooleanSwitch** (前の例の `DataMessagesSwitch` など) をオンにする必要がある場合は、**Value** を 0 以外の任意の整数に変更します。  
  
5. **TraceSwitch** (前の例の `TraceLevelSwitch` など) をオンにする必要がある場合は、**Value** を適切なレベル設定 (1 から 4) に変更します。  
  
6. .config ファイルにコメントを追加して、スイッチを適切に構成するためにどの値を変更すればよいかをエンドユーザーが明確に理解できるようにします。  
  
     次の例は、コメントを含む、最終的なコードを示しています。  
  
    ```xml  
    <system.diagnostics>  
       <switches>  
          <!-- This switch controls data messages. In order to receive data   
             trace messages, change value="0" to value="1" -->  
          <add name="DataMessagesSwitch" value="0" />  
          <!-- This switch controls general messages. In order to   
             receive general trace messages change the value to the   
             appropriate level. "1" gives error messages, "2" gives errors   
             and warnings, "3" gives more detailed error information, and   
             "4" gives verbose trace information -->  
          <add name="TraceLevelSwitch" value="0" />  
       </switches>  
    </system.diagnostics>  
    ```  
  
## <a name="see-also"></a>参照

- [アプリケーションのトレースとインストルメント](tracing-and-instrumenting-applications.md)
- [方法 : アプリケーション コードにトレース ステートメントを追加する](how-to-add-trace-statements-to-application-code.md)
- [トレース スイッチ](trace-switches.md)
- [トレースおよびデバッグ設定のスキーマ](../configure-apps/file-schema/trace-debug/index.md)
