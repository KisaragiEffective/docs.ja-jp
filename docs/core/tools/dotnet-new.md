---
title: dotnet new コマンド
description: dotnet new コマンドは、指定されたテンプレートに基づいて新しい .NET Core プロジェクトを作成します。
ms.date: 02/13/2020
ms.openlocfilehash: d3c609419596b123f5bfb3ca85cf292a61154a70
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398029"
---
# <a name="dotnet-new"></a>dotnet new

**この記事の対象:** ✔️ .NET Core 2.0 SDK 以降のバージョン

## <a name="name"></a>name

`dotnet new` - 指定したテンプレートに基づいて、新しいプロジェクト、構成ファイル、またはソリューションを作成します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet new <TEMPLATE> [--dry-run] [--force] [-i|--install] [-lang|--language] [-n|--name]
    [--nuget-source] [-o|--output] [-u|--uninstall] [--update-apply] [--update-check] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

## <a name="description"></a>[説明]

`dotnet new` コマンドは、テンプレートに基づいて、.NET Core プロジェクトまたはその他の成果物を作成します。

このコマンドは、[テンプレート エンジン](https://github.com/dotnet/templating)を呼び出し、指定されたテンプレートとオプションに基づいて、ディスク上に成果物を作成します。

## <a name="arguments"></a>引数

- **`TEMPLATE`**

  コマンドが呼び出されたときにインスタンス化するテンプレート。 各テンプレートには、渡すことができるオプションが存在する場合があります。 詳細については、[テンプレートのオプション](#template-options)を参照してください。

  `dotnet new --list` を実行すると、インストールされているすべてのテンプレートの一覧を表示できます。 `TEMPLATE` の値が返されたテーブルの「**テンプレート**」列または「**短い形式の名前**」列のテキストと完全に一致しない場合、それら 2 つの列で部分文字列一致が実行されます。

  .NET Core 3.0 SDK 以降では、次の条件で `dotnet new` コマンドを呼び出すと、CLI によって NuGet.org 内のテンプレートが検索されます。

  - `dotnet new` の呼び出し時に、CLI によってテンプレートの一致が (部分的にも) 検出されない場合。
  - テンプレートの新しいバージョンが利用可能な場合。 この場合、プロジェクトまたは成果物が作成されますが、更新されたバージョンのテンプレートについて、CLI によって警告が表示されます。

  このコマンドには、テンプレートの既定の一覧が含まれています。 使用可能なテンプレートの一覧を取得するには、`dotnet new -l` を使います。 次の表は、.NET Core SDK にプレインストールされているテンプレートの一覧です。 テンプレートの既定の言語は、角かっこで示されます。 短い名前のリンクをクリックすると、特定のテンプレート オプションが表示されます。

| テンプレート                                    | 短い名前                      | 言語     | Tags                                  | 導入時期 |
|----------------------------------------------|---------------------------------|--------------|---------------------------------------|------------|
| コンソール アプリケーション                          | [console](#console)             | [C#], F#, VB | Common/Console                        | 1.0        |
| クラス ライブラリ                                | [classlib](#classlib)           | [C#], F#, VB | Common/Library                        | 1.0        |
| WPF アプリケーション                              | [wpf](#wpf)                     | [C#]         | Common/WPF                            | 3.0        |
| WPF クラス ライブラリ                            | [wpflib](#wpf)                  | [C#]         | Common/WPF                            | 3.0        |
| WPF カスタム コントロール ライブラリ                   | [wpfcustomcontrollib](#wpf)     | [C#]         | Common/WPF                            | 3.0        |
| WPF ユーザー コントロール ライブラリ                     | [wpfusercontrollib](#wpf)       | [C#]         | Common/WPF                            | 3.0        |
| Windows フォーム (WinForms) アプリケーション         | [winforms](#winforms)           | [C#]         | Common/WinForms                       | 3.0        |
| Windows フォーム (WinForms) クラス ライブラリ       | [winformslib](#winforms)        | [C#]         | Common/WinForms                       | 3.0        |
| Worker Service                               | [worker](#web-others)           | [C#]         | Common/Worker/Web                     | 3.0        |
| 単体テスト プロジェクト                            | [mstest](#test)                 | [C#], F#, VB | Test/MSTest                           | 1.0        |
| NUnit 3 テスト プロジェクト                         | [nunit](#nunit)                  | [C#], F#, VB | Test/NUnit                            | 2.1.400    |
| NUnit 3 テスト項目                            | `nunit-test`                    | [C#], F#, VB | Test/NUnit                            | 2.2        |
| xUnit テスト プロジェクト                           | [xunit](#test)                  | [C#], F#, VB | Test/xUnit                            | 1.0        |
| Razor コンポーネント                              | `razorcomponent`                | [C#]         | Web/ASP.NET                           | 3.0        |
| Razor ページ                                   | [page](#page)                   | [C#]         | Web/ASP.NET                           | 2.0        |
| MVC ViewImports                              | [viewimports](#namespace)       | [C#]         | Web/ASP.NET                           | 2.0        |
| MVC ViewStart                                | `viewstart`                     | [C#]         | Web/ASP.NET                           | 2.0        |
| Blazor サーバー アプリ                            | [blazorserver](#blazorserver)   | [C#]         | Web/Blazor                            | 3.0        |
| ASP.NET Core 空                           | [web](#web)                     | [C#], F#     | Web/Empty                             | 1.0        |
| ASP.NET Core Web アプリ (モデル ビュー コントローラー) | [mvc](#web-options)             | [C#], F#     | Web/MVC                               | 1.0        |
| ASP.NET Core Web アプリ                         | [webapp、razor](#web-options)   | [C#]         | Web/MVC/Razor Pages                   | 2.2、2.0   |
| Angular 付きの ASP.NET Core                    | [angular](#spa)                 | [C#]         | Web/MVC/SPA                           | 2.0        |
| React.js 付きの ASP.NET Core                   | [react](#spa)                   | [C#]         | Web/MVC/SPA                           | 2.0        |
| React.js および Redux 付きの ASP.NET Core         | [reactredux](#reactredux)       | [C#]         | Web/MVC/SPA                           | 2.0        |
| Razor クラス ライブラリ                          | [razorclasslib](#razorclasslib) | [C#]         | Web/Razor/Library/Razor Class Library | 2.1        |
| ASP.NET Core Web API                         | [webapi](#webapi)               | [C#], F#     | Web/WebAPI                            | 1.0        |
| ASP.NET Core gRPC サービス                    | [grpc](#web-others)             | [C#]         | Web/gRPC                              | 3.0        |
| プロトコル バッファー ファイル                         | [proto](#namespace)             |              | Web/gRPC                              | 3.0        |
| dotnet gitignore ファイル                        | `gitignore`                     |              | Config                                | 3.0        |
| global.json file                             | [globaljson](#globaljson)       |              | Config                                | 2.0        |
| NuGet 構成                                 | `nugetconfig`                   |              | Config                                | 1.0        |
| dotnet ローカル ツール マニフェスト ファイル              | `tool-manifest`                 |              | Config                                | 3.0        |
| Web 構成                                   | `webconfig`                     |              | Config                                | 1.0        |
| ソリューション ファイル                                | `sln`                           |              | 解決策:                              | 1.0        |

## <a name="options"></a>オプション

- **`--dry-run`**

  指定されたコマンドが実行された場合に起きることの概要が表示されます。 .NET Core 2.2 SDK 以降で利用できます。

- **`--force`**

  既存のファイルを変更する場合でも、コンテンツが強制的に生成されます。 これは、選択されたテンプレートによって出力ディレクトリ内の既存のファイルがオーバーライドされる場合に必要です。

- **`-h|--help`**

  コマンドのヘルプを印刷します。 `dotnet new` コマンド自体、または任意のテンプレートに対して呼び出すことができます。 たとえば、「 `dotnet new mvc --help` 」のように入力します。

- **`-i|--install <PATH|NUGET_ID>`**

  指定された `PATH` または `NUGET_ID` からテンプレート パックがインストールされます。 テンプレート パッケージのプレリリース版をインストールする場合は、`<package-name>::<package-version>` の形式でバージョンを指定する必要があります。 既定では、`dotnet new` は、バージョンに対して \* を渡します。これは最後の安定したパッケージのバージョンを表します。 「[使用例](#examples)」のセクションで、例をご覧ください。
  
  このコマンドの実行時にテンプレートのバージョンが既にインストールされていたら、テンプレートは指定されたバージョンか、最新の安定したバージョン (バージョンが指定されていない場合) に更新されます。

  カスタム テンプレートの作成方法については、[「dotnet new のカスタム テンプレート」](custom-templates.md) を参照してください。

- **`-l|--list`**

  指定した名前を含むテンプレートを列挙します。 名前が指定されていない場合、すべてのテンプレートが一覧表示されます。

- **`-lang|--language {C#|F#|VB}`**

  作成するテンプレートの言語。 使用できる言語は、テンプレートによって異なります ([引数](#arguments)の既定値を参照してください)。 一部のテンプレートでは無効です。

  > [!NOTE]
  > 一部のシェルは `#` を特殊文字として解釈します。 そのような場合は、言語パラメーター値を引用符で囲みます。 たとえば、「 `dotnet new console -lang "F#"` 」のように入力します。

- **`-n|--name <OUTPUT_NAME>`**

  作成される出力の名前です。 名前が指定されていない場合、現在のディレクトリの名前が使用されます。

- **`--nuget-source`**

  インストール中に使用する NuGet ソースを 1 つ指定します。 .NET Core 2.1 SDK 以降で利用できます。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  生成された出力を配置する場所。 既定値は、現在のディレクトリです。

- **`--type`**

  使用可能な種類に基づいて、テンプレートをフィルター処理します。 定義済みの値は、"project"、"item"、または "other" です。

- **`-u|--uninstall [PATH|NUGET_ID]`**

  指定された `PATH` または `NUGET_ID` でテンプレート パックがアンインストールされます。 `<PATH|NUGET_ID>` 値が指定されないと、現在インストールされているすべてのテンプレート パックとそれらに関連付けられているテンプレートが表示されます。 `NUGET_ID` を指定した場合、バージョン番号は含めないでください。

  このオプションにパラメーターを指定しないと、コマンドによって、インストールされたテンプレートとそれらに関する詳細が表示されます。

  > [!NOTE]
  > `PATH` を使用してテンプレートをアンインストールするには、完全修飾パスを使用する必要があります。 たとえば、*C:/Users/\<ユーザー>/Documents/Templates/GarciaSoftware.ConsoleTemplate.CSharp* は有効ですが、 *./GarciaSoftware.ConsoleTemplate.CSharp* が含まれるフォルダーから、そのパスを指定することはできません。
  > テンプレートのパスの最後にある終端ディレクトリのスラッシュは含めないでください。

- **`--update-apply`**

  現在インストールされているテンプレート パックに利用できる更新プログラムがあるかどうか確認され、それらがインストールされます。 .NET Core 3.0 SDK 以降で使用できます。

- **`--update-check`**

  現在インストールされているテンプレート パックに利用できる更新プログラムがあるかどうか確認されます。 .NET Core 3.0 SDK 以降で使用できます。

## <a name="template-options"></a>テンプレート オプション

プロジェクト テンプレートはそれぞれ、追加のオプションが与えられている場合があります。 コア テンプレートの場合、次のオプションが追加されています。

### <a name="console"></a>console

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 3.0 SDK 以降で使用できます。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--langVersion <VERSION_NUMBER>`**

  作成されたプロジェクト ファイルの `LangVersion` プロパティが設定されます。 たとえば、C# 7.3 を使うには `--langVersion 7.3` を使います。 F# に対してはサポートされません。 .NET Core 2.2 SDK 以降で利用できます。

  既定の C# バージョンの一覧については、「[既定値](../../csharp/language-reference/configure-language-version.md#defaults)」をご覧ください。

- **`--no-restore`**

  指定した場合、プロジェクトの作成中には暗黙的な復元が実行されません。 .NET Core 2.2 SDK 以降で利用できます。

***

### <a name="classlib"></a>classlib

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 値: .NET Core クラス ライブラリを作成するには `netcoreapp<version>`、.NET 標準クラス ライブラリを作成するには `netstandard<version>` です。 既定値は `netstandard2.0` です。

- **`--langVersion <VERSION_NUMBER>`**

  作成されたプロジェクト ファイルの `LangVersion` プロパティが設定されます。 たとえば、C# 7.3 を使うには `--langVersion 7.3` を使います。 F# に対してはサポートされません。 .NET Core 2.2 SDK 以降で利用できます。

  既定の C# バージョンの一覧については、「[既定値](../../csharp/language-reference/configure-language-version.md#defaults)」をご覧ください。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="wpf"></a> wpf、wpflib、wpfcustomcontrollib、wpfusercontrollib

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 既定値は `netcoreapp3.1` です。 .NET Core 3.1 SDK 以降で利用できます。

- **`--langVersion <VERSION_NUMBER>`**

  作成されたプロジェクト ファイルの `LangVersion` プロパティが設定されます。 たとえば、C# 7.3 を使うには `--langVersion 7.3` を使います。

  既定の C# バージョンの一覧については、「[既定値](../../csharp/language-reference/configure-language-version.md#defaults)」をご覧ください。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="winforms"></a> winforms、winformslib

- **`--langVersion <VERSION_NUMBER>`**

  作成されたプロジェクト ファイルの `LangVersion` プロパティが設定されます。 たとえば、C# 7.3 を使うには `--langVersion 7.3` を使います。

  既定の C# バージョンの一覧については、「[既定値](../../csharp/language-reference/configure-language-version.md#defaults)」をご覧ください。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="web-others"></a> worker、grpc

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 既定値は `netcoreapp3.1` です。 .NET Core 3.1 SDK 以降で利用できます。

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="test"></a> mstest、xunit

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 3.0 SDK 以降で利用できるオプションです。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`-p|--enable-pack`**

  [dotnet pack](dotnet-pack.md) を使用してプロジェクトのパッケージ化が有効化されます。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="nunit"></a>nunit

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.2         | `netcoreapp2.2` |
  | 2.1         | `netcoreapp2.1` |

- **`-p|--enable-pack`**

  [dotnet pack](dotnet-pack.md) を使用してプロジェクトのパッケージ化が有効化されます。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="page"></a>ページ (page)

- **`-na|--namespace <NAMESPACE_NAME>`**

  生成されるコードの名前空間です。 既定値は `MyApp.Namespace` です。

- **`-np|--no-pagemodel`**

  PageModel なしでページが作成されます。

***

### <a name="namespace"></a> viewimports、proto

- **`-na|--namespace <NAMESPACE_NAME>`**

  生成されるコードの名前空間です。 既定値は `MyApp.Namespace` です。

***

### <a name="blazorserver"></a>blazorserver

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  使う認証の種類。 指定できる値は、

  - `None` - 認証は行われません (既定)。
  - `Individual` - 個別認証です。
  - `IndividualB2C` - Azure AD B2C での個別認証。
  - `SingleOrg` - 単一のテナントに対する組織認証。
  - `MultiOrg` - 複数のテナントに対する組織認証です。
  - `Windows` - Windows 認証。

- **`--aad-b2c-instance <INSTANCE>`**

  接続先の Azure Active Directory B2C インスタンス。 `IndividualB2C` 認証で使用します。 既定値は `https://login.microsoftonline.com/tfp/` です。

- **`-ssp|--susi-policy-id <ID>`**

  このプロジェクト用のサインインおよびサインアップ ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`-rp|--reset-password-policy-id <ID>`**

  このプロジェクトのリセット パスワード ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`-ep|--edit-profile-policy-id <ID>`**

  このプロジェクトの編集プロファイル ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`--aad-instance <INSTANCE>`**

  接続先の Azure Active Directory インスタンスです。 `SingleOrg` 認証または `MultiOrg` 認証で使用します。 既定値は `https://login.microsoftonline.com/` です。

- **`--client-id <ID>`**

  このプロジェクトのクライアント ID です。 `IndividualB2C` 認証、`SingleOrg` 認証、または `MultiOrg` 認証で使用します。 既定値は `11111111-1111-1111-11111111111111111` です。

- **`--domain <DOMAIN>`**

  ディレクトリ テナントのドメインです。 `SingleOrg` 認証または `IndividualB2C` 認証で使用します。 既定値は `qualified.domain.name` です。

- **`--tenant-id <ID>`**

  接続先のディレクトリの TenantId ID です。 `SingleOrg` 認証で使用します。 既定値は `22222222-2222-2222-2222-222222222222` です。

- **`--callback-path <PATH>`**

  リダイレクト URI のアプリケーションの基本パス内の要求パスです。 `SingleOrg` 認証または `IndividualB2C` 認証で使用します。 既定値は `/signin-oidc` です。

- **`-r|--org-read-access`**

  このアプリケーションにディレクトリへの読み取りアクセスを許可します。 `SingleOrg` 認証または `MultiOrg` 認証にのみ適用されます。

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`--no-https`**

  HTTPS を無効にします。 このオプションは、`Individual`、`IndividualB2C`、`SingleOrg`、または `MultiOrg` が `--auth` 用に使用されない場合にのみ適用されます。

- **`-uld|--use-local-db`**

  SQLite ではなく LocalDB が使用されるように指定されます。 `Individual` 認証または `IndividualB2C` 認証にのみ適用されます。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="web"></a>Web

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 2.2 SDK では使用できません。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

- **`--no-https`**

  HTTPS を無効にします。

***

### <a name="web-options"></a> mvc、webapp

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  使う認証の種類。 指定できる値は、

  - `None` - 認証は行われません (既定)。
  - `Individual` - 個別認証です。
  - `IndividualB2C` - Azure AD B2C での個別認証。
  - `SingleOrg` - 単一のテナントに対する組織認証。
  - `MultiOrg` - 複数のテナントに対する組織認証です。
  - `Windows` - Windows 認証。

- **`--aad-b2c-instance <INSTANCE>`**

  接続先の Azure Active Directory B2C インスタンス。 `IndividualB2C` 認証で使用します。 既定値は `https://login.microsoftonline.com/tfp/` です。

- **`-ssp|--susi-policy-id <ID>`**

  このプロジェクト用のサインインおよびサインアップ ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`-rp|--reset-password-policy-id <ID>`**

  このプロジェクトのリセット パスワード ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`-ep|--edit-profile-policy-id <ID>`**

  このプロジェクトの編集プロファイル ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`--aad-instance <INSTANCE>`**

  接続先の Azure Active Directory インスタンスです。 `SingleOrg` 認証または `MultiOrg` 認証で使用します。 既定値は `https://login.microsoftonline.com/` です。

- **`--client-id <ID>`**

  このプロジェクトのクライアント ID です。 `IndividualB2C` 認証、`SingleOrg` 認証、または `MultiOrg` 認証で使用します。 既定値は `11111111-1111-1111-11111111111111111` です。

- **`--domain <DOMAIN>`**

  ディレクトリ テナントのドメインです。 `SingleOrg` 認証または `IndividualB2C` 認証で使用します。 既定値は `qualified.domain.name` です。

- **`--tenant-id <ID>`**

  接続先のディレクトリの TenantId ID です。 `SingleOrg` 認証で使用します。 既定値は `22222222-2222-2222-2222-222222222222` です。

- **`--callback-path <PATH>`**

  リダイレクト URI のアプリケーションの基本パス内の要求パスです。 `SingleOrg` 認証または `IndividualB2C` 認証で使用します。 既定値は `/signin-oidc` です。

- **`-r|--org-read-access`**

  このアプリケーションにディレクトリへの読み取りアクセスを許可します。 `SingleOrg` 認証または `MultiOrg` 認証にのみ適用されます。

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`--no-https`**

  HTTPS を無効にします。 このオプションは、`Individual`、`IndividualB2C`、`SingleOrg`、または `MultiOrg` が使用されない場合にのみ適用されます。

- **`-uld|--use-local-db`**

  SQLite ではなく LocalDB が使用されるように指定されます。 `Individual` 認証または `IndividualB2C` 認証にのみ適用されます。

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 3.0 SDK 以降で利用できるオプションです。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

- **`--use-browserlink`**

  プロジェクトに BrowserLink を含めます。 .NET Core 2.2 および 3.1 SDK では使用できません。

***

### <a name="spa"></a> angular、react

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  使う認証の種類。 .NET Core 3.0 SDK 以降で使用できます。
  
  指定できる値は、

  - `None` - 認証は行われません (既定)。
  - `Individual` - 個別認証です。

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

- **`--no-https`**

  HTTPS を無効にします。 このオプションは、認証が `None` の場合にのみ適用されます。

- **`-uld|--use-local-db`**

  SQLite ではなく LocalDB が使用されるように指定されます。 `Individual` 認証または `IndividualB2C` 認証にのみ適用されます。 .NET Core 3.0 SDK 以降で使用できます。

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 2.2 SDK では使用できません。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

***

### <a name="reactredux"></a>reactredux

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 2.2 SDK では使用できません。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.0` |

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

- **`--no-https`**

  HTTPS を無効にします。

***

### <a name="razorclasslib"></a>razorclasslib

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

- **`-s|--support-pages-and-views`**

  このライブラリへのコンポーネントに加え、従来の Razor ページとビューの追加がサポートされます。 .NET Core 3.0 SDK 以降で使用できます。

***
  
### <a name="webapi"></a>webapi

- **`-au|--auth <AUTHENTICATION_TYPE>`**

  使う認証の種類。 指定できる値は、

  - `None` - 認証は行われません (既定)。
  - `IndividualB2C` - Azure AD B2C での個別認証。
  - `SingleOrg` - 単一のテナントに対する組織認証。
  - `Windows` - Windows 認証。

- **`--aad-b2c-instance <INSTANCE>`**

  接続先の Azure Active Directory B2C インスタンス。 `IndividualB2C` 認証で使用します。 既定値は `https://login.microsoftonline.com/tfp/` です。

- **`-ssp|--susi-policy-id <ID>`**

  このプロジェクト用のサインインおよびサインアップ ポリシー ID です。 `IndividualB2C` 認証で使用します。

- **`--aad-instance <INSTANCE>`**

  接続先の Azure Active Directory インスタンスです。 `SingleOrg` 認証で使用します。 既定値は `https://login.microsoftonline.com/` です。

- **`--client-id <ID>`**

  このプロジェクトのクライアント ID です。 `IndividualB2C` 認証または `SingleOrg` 認証で使用します。 既定値は `11111111-1111-1111-11111111111111111` です。

- **`--domain <DOMAIN>`**

  ディレクトリ テナントのドメインです。 `IndividualB2C` 認証または `SingleOrg` 認証で使用します。 既定値は `qualified.domain.name` です。

- **`--tenant-id <ID>`**

  接続先のディレクトリの TenantId ID です。 `SingleOrg` 認証で使用します。 既定値は `22222222-2222-2222-2222-222222222222` です。

- **`-r|--org-read-access`**

  このアプリケーションにディレクトリへの読み取りアクセスを許可します。 `SingleOrg` 認証にのみ適用されます。

- **`--exclude-launch-settings`**

  生成されたテンプレートから *launchSettings.json* が除外されます。

- **`--no-https`**

  HTTPS を無効にします。 `app.UseHsts` と `app.UseHttpsRedirection` は `Startup.Configure` に追加されません。 このオプションは、`IndividualB2C` または `SingleOrg` が認証用に使用されない場合にのみ適用されます。

- **`-uld|--use-local-db`**

  SQLite ではなく LocalDB が使用されるように指定されます。 `IndividualB2C` 認証にのみ適用されます。

- **`-f|--framework <FRAMEWORK>`**

  ターゲットにする[フレームワーク](../../standard/frameworks.md)が指定されます。 .NET Core 2.2 SDK では使用できません。

  次の表は、使用する SDK バージョン番号に対応した既定値を示しています。

  | SDK バージョン | 既定値   |
  |-------------|-----------------|
  | 3.1         | `netcoreapp3.1` |
  | 3.0         | `netcoreapp3.0` |
  | 2.1         | `netcoreapp2.1` |

- **`--no-restore`**

  プロジェクトの作成中に暗黙的な復元は実行されません。

***

### <a name="globaljson"></a>globaljson

- **`--sdk-version <VERSION_NUMBER>`**

  *global.json* ファイル内で使用する .NET Core SDK のバージョンが指定されます。

***

## <a name="examples"></a>例

- テンプレート名を指定することによって、C# コンソール アプリケーション プロジェクトを作成します。

  ```dotnetcli
  dotnet new "Console Application"
  ```

- 現在のディレクトリに、F# コンソール アプリケーション プロジェクトを作成します。

  ```dotnetcli
  dotnet new console -lang F#
  ```

- 指定されたディレクトリ内に .NET Standard クラス ライブラリ プロジェクトが作成されます。

  ```dotnetcli
  dotnet new classlib -lang VB -o MyLibrary
  ```

- 認証なしで、現在のディレクトリに新しい ASP.NET Core C# MVC プロジェクトを作成します。

  ```dotnetcli
  dotnet new mvc -au None
  ```

- 新しい xUnit プロジェクトを作成します。

  ```dotnetcli
  dotnet new xunit
  ```

- シングル ページ アプリケーション (SPA) テンプレートに使用できるすべてのテンプレートを一覧表示します。

  ```dotnetcli
  dotnet new spa -l
  ```

- 部分文字列 *we* に一致するすべてのテンプレートの一覧を取得します。 完全一致が見つからないので、短い名前と名前の両方の列に対して部分文字列一致が実行されます。

  ```dotnetcli
  dotnet new we -l
  ```

- *ng* に一致するテンプレートの呼び出しを試みます。 一致が 1 つだけでない場合、部分的に一致するテンプレートの一覧を取得します。

  ```dotnetcli
  dotnet new ng
  ```

- ASP.NET Core 用の SPA テンプレートのバージョン 2.0 がインストールされます。

  ```dotnetcli
  dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
  ```

- インストールされているテンプレートとそれらに関する詳細や、それらのアンインストール方法を一覧表示します。

  ```dotnetcli
  dotnet new -u
  ```

- 現在のディレクトリに *global.json* が作成され、SDK バージョンが 3.1.101 に設定されます。

  ```dotnetcli
  dotnet new globaljson --sdk-version 3.1.101
  ```

## <a name="see-also"></a>参照

- [dotnet new のカスタム テンプレート](custom-templates.md)
- [dotnet new のカスタム テンプレートを作成する](../tutorials/cli-templates-create-item-template.md)
- [dotnet/dotnet-template-samples GitHub リポジトリ](https://github.com/dotnet/dotnet-template-samples)
- [dotnet new で使用できるテンプレート](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)
