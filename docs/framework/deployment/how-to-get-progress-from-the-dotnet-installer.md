---
title: '方法: .NET Framework 4.5 インストーラーの進行状況を表示する'
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- progress information, .NET Framework installer
- .NET Framework, installing
ms.assetid: 0a1a3ba3-7e46-4df2-afd3-f3a8237e1c4f
ms.openlocfilehash: cd81ad83aee80341d0334cfa8caa165b25ee0564
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75716495"
---
# <a name="how-to-get-progress-from-the-net-framework-45-installer"></a>方法: .NET Framework 4.5 インストーラーの進行状況を表示する

.NET Framework 4.5 は再頒布可能なランタイムです。 このバージョンの .NET Framework 用アプリを開発する場合は、アプリのセットアップに必要なパーツとして、.NET Framework 4.5 セットアップを含める (チェーンする) ことができます。 セットアップ手順をカスタマイズまたは統一するために、アプリのセットアップの進行状況を表示しながら、.NET Framework 4.5 セットアップをサイレントで起動し、その進行状況を追跡できます。 サイレントな追跡を可能にするために、.NET Framework 4.5 セットアップ (監視対象) ではメモリ マップ I/O (MMIO) セグメントを使用することでプロトコルを定義し、セットアップ (ウォッチャーまたはチェーン元) と通信します。 このプロトコルは、チェーン元による進行状況情報の取得、詳細な結果の取得、メッセージへの応答、.NET Framework 4.5 セットアップの取り消しを行う方法を定義します。

- **呼び出し**。 .NET Framework 4.5 セットアップを呼び出して、MMIO セクションから進行状況情報を受け取るには、セットアップ プログラムで以下の処理を行う必要があります。

    1. .NET Framework 4.5 再頒布可能プログラムを呼び出します。

        `dotNetFx45_Full_x86_x64.exe /q /norestart /pipe section-name`

        この *section name* は、アプリの識別に使用する名前です。 .NET Framework のセットアップでは MMIO セクションに対する読み書きを非同期に行うので、その間、イベントおよびメッセージの使用が役立つ場合があります。 この例では、.NETFramework セットアップ プロセスは、MMIO セクションの割り当て (`TheSectionName`) とイベントの定義 (`TheEventName`) の両方を行うコンストラクターによって作成されます。

        ```cpp
        Server():ChainerSample::MmioChainer(L"TheSectionName", L"TheEventName")
        ```

        これらの名前は、実際のセットアップ プログラムの固有な名前に置き換えてください。

    2. MMIO セクションから読み取ります。 .NET Framework 4.5 では、ダウンロード操作とインストール操作は同時に行われます。 .NET Framework の 1 つのパーツがインストールしている間に、別のパーツがダウンロードします。 その結果、進行状況は、0 から 255 まで増加する 2 つの値 (`m_downloadSoFar` および `m_installSoFar`) として MMIO セクションに送り返されます (書き込まれます)。 255 が書き込まれて、.NET Framework が終了すると、インストールは完了します。

- **終了コード**。 .NET Framework 4.5 再頒布可能プログラムを呼び出すためのコマンドからの以下の終了コードは、セットアップが成功または失敗したことを示します。

  - 0: セットアップは、正常に完了しました。

  - 3010: セットアップは正常に完了しました。システムの再起動が必要です。

  - 1602: セットアップは取り消されました。

  - 他のすべてのコード: セットアップでエラーが発生しました。詳細については、%temp% に作成されるログ ファイルを調べてください。

- **セットアップの取り消し**。 `Abort` メソッドを使用して MMIO セクションの `m_downloadAbort` フラグと `m_ installAbort` フラグを設定することで、いつでもセットアップを取り消すことができます。

## <a name="chainer-sample"></a>チェーン元のサンプル

チェーン元のサンプルが、進行状況を表示しながらサイレントで起動し、.NET Framework 4.5 セットアップを追跡します。 このサンプルは、.NET Framework 4 用のチェーン元サンプルに似ています。 ただし、.NET Framework 4 アプリケーションを閉じるためのメッセージ ボックスを処理することで、システムの再起動を避けることができます。 このメッセージ ボックスの詳細については、「[.NET Framework 4.5 のインストール中のシステム再起動の削減](reducing-system-restarts.md)」を参照してください。 このサンプルは .NET Framework 4 インストーラーで使用できます。そのシナリオではメッセージは送信されません。

> [!WARNING]
> 例の実行は、管理者として行う必要があります。

MSDN サンプル ギャラリーから [.NET Framework 4.5 チェーン元のサンプル](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba)の完全な Visual Studio ソリューションをダウンロードできます。

以下のセクションでは、この例の重要なファイルである MMIOChainer.h、ChainingdotNet4.cpp、および IProgressObserver.h について説明します。

#### <a name="mmiochainerh"></a>MMIOChainer.h

- MMIOChainer.h ファイル ([完全なコード](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=663039622)を参照) には、データ構造体の定義と、チェーン元クラスが派生する基底クラスが含まれます。 .NET Framework 4.5 によって、MMIO データ構造体が拡張され、.NET Framework 4.5 インストーラーが必要とするデータが処理されます。 MMIO 構造体への変更には下位互換性があるため、.NET Framework 4 チェーン元は、再コンパイルを必要とせずに .NET Framework 4.5 のセットアップで機能します。 ただし、このシナリオはシステムの再起動を削減するための機能をサポートしていません。

    バージョン フィールドは、構造体およびメッセージ形式へのリビジョンを識別するための手段を提供します。 .NET Framework セットアップでは、`VirtualQuery` 関数を呼び出してファイル マップのサイズを判断することで、チェーン元インターフェイスのバージョンを判別します。 サイズがバージョン フィールドに対応できる十分な大きさである場合、.NET Framework セットアップでは指定された値を使用します。 .NET Framework 4 の場合のように、バージョン フィールドを含めるにはファイル マップが小さすぎる場合、セットアップ プロセスではバージョン 0 (4) を使用します。 .NET Framework セットアップが送信しようとするメッセージのバージョンをチェーン元がサポートしていない場合、.NET Framework セットアップでは応答を無視します。

    MMIO データ構造体は以下のように定義されます。

    ```cpp
    // MMIO data structure for interprocess communication
        struct MmioDataStructure
        {
            bool m_downloadFinished;               // Is download complete?
            bool m_installFinished;                // Is installation complete?
            bool m_downloadAbort;                  // Set to cause downloader to abort.
            bool m_installAbort;                   // Set to cause installer to abort.
            HRESULT m_hrDownloadFinished;          // Resulting HRESULT for download.
            HRESULT m_hrInstallFinished;           // Resulting HRESULT for installation.
            HRESULT m_hrInternalError;
            WCHAR m_szCurrentItemStep[MAX_PATH];
            unsigned char m_downloadSoFar;         // Download progress 0-255 (0-100% done).
            unsigned char m_installSoFar;          // Installation progress 0-255 (0-100% done).
            WCHAR m_szEventName[MAX_PATH];         // Event that chainer creates and chainee opens to sync communications.

            BYTE m_version;                        // Version of the data structure, set by chainer:
                                                   // 0x0: .NET Framework 4
                                                   // 0x1: .NET Framework 4.5

            DWORD m_messageCode;                   // Current message sent by the chainee; 0 if no message is active.
            DWORD m_messageResponse;               // Chainer's response to current message; 0 if not yet handled.
            DWORD m_messageDataLength;             // Length of the m_messageData field, in bytes.
            BYTE m_messageData[1];                 // Variable-length buffer; content depends on m_messageCode.
        };
    ```

- `MmioDataStructure` データ構造体を直接使用するのではなく、チェーン元を実装するための `MmioChainer` クラスを使用します。 `MmioChainer` クラスから派生させて、.NET Framework 4.5 再頒布可能プログラムをチェーンします。

#### <a name="iprogressobserverh"></a>IProgressObserver.h

- IProgressObserver.h ファイルは進行状況のオブザーバー ([完全なコードを参照](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=1263700592)) を実装します。 このオブザーバーは、ダウンロードとインストールの進行状況 (1% ～ 100% 完了を示す、符号なしの `char` 0 ～ 255 の値で指定) の通知を受け取ります。 オブザーバーは、チェーン対象がメッセージを送信したときにも通知を受け取ります。通知を受け取ったオブザーバーは、応答を送信する必要があります。

    ```cpp
        class IProgressObserver
        {
        public:
            virtual void OnProgress(unsigned char) = 0; // 0 - 255:  255 == 100%
            virtual void Finished(HRESULT) = 0;         // Called when operation is complete
            virtual DWORD Send(DWORD dwMessage, LPVOID pData, DWORD dwDataLength) = 0; // Called when a message is sent
        };
    ```

#### <a name="chainingdotnet45cpp"></a>ChainingdotNet4.5.cpp

- [ChainingdotNet4.5.cpp](https://code.msdn.microsoft.com/NET-Framework-45-Developer-e416a0ba/sourcecode?fileId=47345&pathId=1757268882) ファイルは、`Server` クラスを実装します。このクラスは `MmioChainer` クラスから派生し、適切なメソッドをオーバーライドして進行状況情報を表示します。 MmioChainer は、指定されたセクション名でセクションを作成し、指定されたイベント名でチェーン元を初期化します。 イベント名は、マップされたデータ構造体に保存されます。 セクションとイベント名は一意にする必要があります。 次のコードの `Server` クラスは、指定されたセットアップ プログラムを起動して進行状況を監視し、終了コードを返します。

    ```cpp
    class Server : public ChainerSample::MmioChainer, public ChainerSample::IProgressObserver
    {
    public:
        …………….
        Server():ChainerSample::MmioChainer(L"TheSectionName", L"TheEventName") //customize for your event names
        {}
    ```

    インストールが Main メソッドで開始されます。

    ```cpp
    // Main entry point for program
    int __cdecl main(int argc, _In_count_(argc) char **_argv)
    {
        int result = 0;
        CString args;
        if (argc > 1)
        {
            args = CString(_argv[1]);
        }

        if (IsNetFx4Present(NETFX45_RC_REVISION))
        {
            printf(".NET Framework 4.5 is already installed");
        }
        else
        {
            result = Server().Launch(args);
        }

        return result;
    }
    ```

- インストールを始める前に、チェーン元では `IsNetFx4Present` を呼び出して、.NET Framework 4.5 が既にインストールされているかどうかの確認が行われます。

    ```cpp
    ///  Checks for presence of the .NET Framework 4.
    ///    A value of 0 for dwMinimumRelease indicates a check for the .NET Framework 4 full
    ///    Any other value indicates a check for a specific compatible release of the .NET Framework 4.
    #define NETFX40_FULL_REVISION 0
    // TODO: Replace with released revision number
    #define NETFX45_RC_REVISION MAKELONG(50309, 5)   // .NET Framework 4.5
    bool IsNetFx4Present(DWORD dwMinimumRelease)
    {
        DWORD dwError = ERROR_SUCCESS;
        HKEY hKey = NULL;
        DWORD dwData = 0;
        DWORD dwType = 0;
        DWORD dwSize = sizeof(dwData);

        dwError = ::RegOpenKeyExW(HKEY_LOCAL_MACHINE, L"SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full", 0, KEY_READ, &hKey);
        if (ERROR_SUCCESS == dwError)
        {
            dwError = ::RegQueryValueExW(hKey, L"Release", 0, &dwType, (LPBYTE)&dwData, &dwSize);

            if ((ERROR_SUCCESS == dwError) && (REG_DWORD != dwType))
            {
                dwError = ERROR_INVALID_DATA;
            }
            else if (ERROR_FILE_NOT_FOUND == dwError)
            {
                // Release value was not found, let's check for 4.0.
                dwError = ::RegQueryValueExW(hKey, L"Install", 0, &dwType, (LPBYTE)&dwData, &dwSize);

                // Install = (REG_DWORD)1;
                if ((ERROR_SUCCESS == dwError) && (REG_DWORD == dwType) && (dwData == 1))
                {
                    // treat 4.0 as Release = 0
                    dwData = 0;
                }
                else
                {
                    dwError = ERROR_INVALID_DATA;
                }
            }
        }

        if (hKey != NULL)
        {
            ::RegCloseKey(hKey);
        }

        return ((ERROR_SUCCESS == dwError) && (dwData >= dwMinimumRelease));
    }
    ```

- `Launch` メソッド内の実行可能ファイル (例では Setup.exe) のパスを正しい場所に変更するか、コードをカスタマイズして場所を判断することができます。 `MmioChainer` 基底クラスは、派生クラスが呼び出すブロッキング `Run()` メソッドを提供します。

    ```cpp
    bool Launch(const CString& args)
    {
    CString cmdline = L"dotNetFx45_Full_x86_x64.exe -pipe TheSectionName " + args; // Customize with name and location of setup .exe that you want to run
    STARTUPINFO si = {0};
    si.cb = sizeof(si);
    PROCESS_INFORMATION pi = {0};

    // Launch the Setup.exe that installs the .NET Framework 4.5
    BOOL bLaunchedSetup = ::CreateProcess(NULL,
     cmdline.GetBuffer(),
     NULL, NULL, FALSE, 0, NULL, NULL,
     &si,
     &pi);

    // If successful
    if (bLaunchedSetup != 0)
    {
    IProgressObserver& observer = dynamic_cast<IProgressObserver&>(*this);
    Run(pi.hProcess, observer);

    ……………………..
    return (bLaunchedSetup != 0);
    }
    ```

- `Send` メソッドはメッセージを途中受信して処理します。 このバージョンの .NET Framework では、サポートされているメッセージはアプリケーションの終了メッセージのみです。

    ```cpp
            // SendMessage
            //
            // Send a message and wait for the response.
            // dwMessage: Message to send
            // pData: The buffer to copy the data to
            // dwDataLength: Initially a pointer to the size of pBuffer. Upon successful call, the number of bytes copied to pBuffer.
            //--------------------------------------------------------------
        virtual DWORD Send(DWORD dwMessage, LPVOID pData, DWORD dwDataLength)
        {
            DWORD dwResult = 0;
            printf("received message: %d\n", dwMessage);
            // Handle message
            switch (dwMessage)
            {
            case MMIO_CLOSE_APPS:
                {
                    printf("    applications are holding files in use:\n");
                    IronMan::MmioCloseApplications* applications = reinterpret_cast<IronMan::MmioCloseApplications*>(pData);
                    for(DWORD i = 0; i < applications->m_dwApplicationsSize; i++)
                    {
                        printf("      %ls (%d)\n", applications->m_applications[i].m_szName, applications->m_applications[i].m_dwPid);
                    }

                    printf("    should appliations be closed? (Y)es, (N)o, (R)efresh : ");
                    while (dwResult == 0)
                    {
                        switch (toupper(getwchar()))
                        {
                        case 'Y':
                            dwResult = IDYES;  // Close apps
                            break;
                        case 'N':
                            dwResult = IDNO;
                            break;
                        case 'R':
                            dwResult = IDRETRY;
                            break;
                        }
                    }
                    printf("\n");
                    break;
                }
            default:
                break;
            }
            printf("  response: %d\n  ", dwResult);
            return dwResult;
        }
    };
    ```

- 進行状況のデータは、0 (0%) ～ 255 (100%) の範囲の符号なし `char` です。

    ```cpp
    private: // IProgressObserver
        virtual void OnProgress(unsigned char ubProgressSoFar)
        {…………
       }
    ```

- HRESULT は、`Finished` メソッドに渡されます。

    ```cpp
    virtual void Finished(HRESULT hr)
    {
    // This HRESULT is communicated over MMIO and may be different than process
    // Exit code of the Chainee Setup.exe itself
    printf("\r\nFinished HRESULT: 0x%08X\r\n", hr);
    }
    ```

    > [!IMPORTANT]
    > .NET Framework 4.5 の再頒布可能プログラムによって、通常、多数の進行状況メッセージと、(チェーン元側で) 完了を示す 1 つのメッセージが書き込まれます。 また、非同期に読み取りを行って、`Abort` レコードを探します。 `Abort` レコードを受け取った場合は、インストールを取り消し、インストールが中止され、セットアップ操作がロールバックされた後に、データとして E_ABORT を含む終了レコードを書き込みます。

標準的なサーバーは、ランダムな MMIO ファイル名を作成し、ファイル (前のコード例の `Server::CreateSection` で示されているファイル) を作成した後、`CreateProcess` メソッドを使用して `-pipe someFileSectionName` オプションでパイプ名を渡すことによって、再頒布可能プログラムを起動します。 サーバーは、アプリケーションの UI 固有のコードを使用して `OnProgress`、`Send`、および `Finished` の各メソッドを実装する必要があります。

## <a name="see-also"></a>関連項目

- [配置ガイド (開発者向け)](deployment-guide-for-developers.md)
- [配置](index.md)
