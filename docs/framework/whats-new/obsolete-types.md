---
title: 旧版の .NET Framework の型
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 4.5, obsolete types
- types, obsolete in .NET Framework 4.5
- obsolete types [.NET Framework]
ms.assetid: e636d024-0fac-45eb-b721-25a8c0ceca8f
ms.openlocfilehash: b7932a553f39e1f1da2a3946878d6224099da8da
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74802688"
---
# <a name="obsolete-types-in-the-net-framework"></a>旧版の .NET Framework の型

<a name="introduction"></a>この記事の表には、.NET Framework 4.5 および .NET Framework 4.6 で互換性のために残されている型がアセンブリ別にまとめられています。 各アセンブリで互換性のために残されている型と推奨される代替型の一覧を表示するには、以下のリンクを使用してください。 これらの型は廃止されているため、そのメンバーもすべて廃止されます。 .NET Framework クラス ライブラリで互換性のために残されているその他のメンバーの一覧については、[互換性のために残されているメンバー](obsolete-members.md)に関する記事をご覧ください。

- [互換性のために残されているシステム アセンブリの型](#obsolete_types_in_system_assemblies)

  - [mscorlib.dll](#mscorlib)

  - [System.Core.dll](#Core)

  - [System.Data.dll](#data)

  - [System.Data.OracleClient.dll](#oracleclient)

  - [System.Design.dll](#design)

  - [System.dll](#system)

  - [System.EnterpriseServices.dll](#enterpriseservices)

  - [System.Net.dll](#net)

  - [System.ServiceModel.dll](#servicemodel)

  - [System.Web.dll](#web)

  - [System.Web.Mobile.dll](#mobile)

  - [System.Workflow.Activities.dll](#workflow_activities)

  - [System.Workflow.ComponentModel.dll](#workflow_componentmodel)

  - [System.Workflow.Runtime.dll](#workflow_runtime)

  - [System.WorkflowServices.dll](#workflowservices)

  - [System.Xaml.dll](#xaml)

  - [System.Xml.dll](#xml)

  - [WindowsBase.dll](#WindowsBase)

- [互換性のために残されている Microsoft アセンブリの型](#obsolete_types_in_microsoft_assemblies)

  - [IEHost.dll and IEExec.exe](#IEHost)

  - [Microsoft.Build.Engine.dll](#Engine)

  - [Microsoft.JScript.dll](#jscript)

  - [Microsoft.VisualBasic.Compatibility.dll](#VBCompat)

  - [Microsoft.VisualBasic.Compatibility.Data.dll](#VBCompatData)

  - [Microsoft.VisualC.dll](#visualc)

<a name="obsolete_types_in_system_assemblies"></a>

## <a name="obsolete-types-in-system-assemblies"></a>互換性のために残されているシステム アセンブリの型

次の表に、システム アセンブリで、互換性のために残されていることが公表されている型を示します。 これらのアセンブリは、.NET Framework を対象とする汎用アプリケーションの開発で使用されます。

<a name="mscorlib"></a>

### <a name="assembly-mscorlibdll"></a>アセンブリ: mscorlib.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.ExecutionEngineException?displayProperty=nameWithType>|以前には、この型は、ランタイムで未定義の致命的なエラーを示していました。 ランタイムがこの例外を発生させることはなくなりましたが、この型は互換性のために残されています。|
|<xref:System.Collections.CaseInsensitiveHashCodeProvider?displayProperty=nameWithType>|代わりに、<xref:System.StringComparer?displayProperty=nameWithType> を使用してください。|
|<xref:System.Collections.IHashCodeProvider?displayProperty=nameWithType>|代わりに、<xref:System.Collections.IEqualityComparer?displayProperty=nameWithType> を使用してください。|
|<xref:System.Configuration.Assemblies.AssemblyHash?displayProperty=nameWithType>|<xref:System.Configuration.Assemblies.AssemblyHash> クラスの使用は非推奨とされました。|
|<xref:System.Diagnostics.Contracts.Internal.ContractHelper?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。 代わりに、System.Runtime.CompilerServices 名前空間の <xref:System.Runtime.CompilerServices.ContractHelper?displayProperty=nameWithType> クラスを使用してください。|
|<xref:System.Reflection.Emit.UnmanagedMarshal?displayProperty=nameWithType>|代替 API を使用できます。代わりに、<xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> カスタム属性を生成してください。|
|<xref:System.Runtime.InteropServices.BIND_OPTS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.BIND_OPTS?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.BINDPTR?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.BINDPTR?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.CALLCONV?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.CALLCONV?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.CONNECTDATA?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.CONNECTDATA?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.DESCKIND?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.DESCKIND?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.DISPPARAMS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.DISPPARAMS?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.ELEMDESC?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.ELEMDESC?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.EXCEPINFO?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.EXCEPINFO?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.FILETIME?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.FILETIME?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.FUNCDESC?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.FUNCDESC?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.FUNCFLAGS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.FUNCFLAGS?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.FUNCKIND?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.FUNCKIND?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute?displayProperty=nameWithType>|この属性の使用は非推奨とされました。この属性は、将来のバージョンでは削除されます。|
|<xref:System.Runtime.InteropServices.IDispatchImplType?displayProperty=nameWithType>|<xref:System.Runtime.InteropServices.IDispatchImplAttribute?displayProperty=nameWithType> の使用は非推奨とされます。|
|<xref:System.Runtime.InteropServices.IDLDESC?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IDLDESC?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.IDLFLAG?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IDLFLAG?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.IMPLTYPEFLAGS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IMPLTYPEFLAGS?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.INVOKEKIND?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.INVOKEKIND?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.LIBFLAGS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.LIBFLAGS?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.PARAMDESC?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.PARAMDESC?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.PARAMFLAG?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.PARAMFLAG?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.SetWin32ContextInIDispatchAttribute?displayProperty=nameWithType>|この属性は非推奨とされました。 アプリケーション ドメインでは、IDispatch の呼び出しでアクティベーション コンテキスト境界は考慮されなくなりました。|
|<xref:System.Runtime.InteropServices.STATSTG?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.STATSTG?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.SYSKIND?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.SYSKIND?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.TYPEATTR?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.TYPEATTR?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.TYPEDESC?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.TYPEDESC?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.TYPEFLAGS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.TYPEFLAGS?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.TYPEKIND?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.TYPEKIND?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.TYPELIBATTR?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.TYPELIBATTR?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIBindCtx?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IBindCtx?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIConnectionPoint?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IConnectionPoint?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIConnectionPointContainer?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IConnectionPointContainer?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIEnumConnectionPoints?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IEnumConnectionPoints?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIEnumConnections?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IEnumConnections?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIEnumMoniker?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IEnumMoniker?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIEnumString?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IEnumString?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIEnumVARIANT?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IEnumVARIANT?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIMoniker?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IMoniker?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIPersistFile?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IPersistFile?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIRunningObjectTable?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IRunningObjectTable?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMIStream?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMITypeComp?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.ITypeComp?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMITypeInfo?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.ITypeInfo?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.UCOMITypeLib?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.ITypeLib?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.VARDESC?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.VARDESC?displayProperty=nameWithType> を使用してください|
|<xref:System.Runtime.InteropServices.VARFLAGS?displayProperty=nameWithType>|代わりに <xref:System.Runtime.InteropServices.ComTypes.VARFLAGS?displayProperty=nameWithType> を使用してください|
|<xref:System.Security.SecurityCriticalScope?displayProperty=nameWithType>|<xref:System.Security.SecurityCriticalScope> は、.NET 2.0 の透過的な互換性のみを目的として使用されます。|
|<xref:System.Security.SecurityTreatAsSafeAttribute?displayProperty=nameWithType>|<xref:System.Security.SecurityTreatAsSafeAttribute> は、.NET 2.0 の透過的な互換性のみを目的として使用されます。 代わりに、<xref:System.Security.SecuritySafeCriticalAttribute?displayProperty=nameWithType> を使用してください。|
|<xref:System.Security.Policy.FirstMatchCodeGroup?displayProperty=nameWithType>|この型は互換性のために残されていますが、.NET Framework の将来のリリースでは削除されます。|
|<xref:System.Security.Policy.PermissionRequestEvidence?displayProperty=nameWithType>|アセンブリ レベルの宣言セキュリティは互換性のために残されていますが、既定では、CLR によって強制されることはなくなりました。|
|<xref:System.Security.Policy.UnionCodeGroup?displayProperty=nameWithType>|この型は互換性のために残されていますが、.NET Framework の将来のリリースでは削除されます。|

[先頭に戻る](#introduction)

<a name="Core"></a>

### <a name="assembly-systemcoredll"></a>アセンブリ: System.Core.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Runtime.CompilerServices.ExecutionScope?displayProperty=nameWithType>|この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この型は使用しないでください。|

[先頭に戻る](#introduction)

<a name="data"></a>

### <a name="assembly-systemdatadll"></a>アセンブリ: System.Data.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Data.DataSysDescriptionAttribute?displayProperty=nameWithType>|<xref:System.Data.DataSysDescriptionAttribute> は非推奨となりました。|
|<xref:System.Data.PropertyAttributes?displayProperty=nameWithType>|<xref:System.Data.PropertyAttributes> は非推奨となりました。|
|<xref:System.Data.TypedDataSetGenerator?displayProperty=nameWithType>|<xref:System.Data.TypedDataSetGenerator> クラスは、将来のリリースでは削除されます。 System.Design.dll の <xref:System.Data.Design.TypedDataSetGenerator?displayProperty=nameWithType> を使用してください。|
|<xref:System.Xml.XmlDataDocument?displayProperty=nameWithType>|<xref:System.Xml.XmlDataDocument> クラスは、将来のリリースでは削除されます。|

[先頭に戻る](#introduction)

<a name="oracleclient"></a>

### <a name="assembly-systemdataoracleclientdll"></a>アセンブリ: System.Data.OracleClient.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Data.OracleClient.OracleClientFactory?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleClientFactory> は非推奨となりました。|
|<xref:System.Data.OracleClient.OracleCommand?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleCommand> は非推奨となりました。|
|<xref:System.Data.OracleClient.OracleCommandBuilder?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleCommandBuilder> は非推奨となりました。|
|<xref:System.Data.OracleClient.OracleConnection?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleConnection> は非推奨となりました。|
|<xref:System.Data.OracleClient.OracleConnectionStringBuilder?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleConnectionStringBuilder> は非推奨となりました。|
|<xref:System.Data.OracleClient.OracleDataAdapter?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OracleDataAdapter> は非推奨となりました。|
|<xref:System.Data.OracleClient.OraclePermission?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OraclePermission> は非推奨となりました。|
|<xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=nameWithType>|<xref:System.Data.OracleClient.OraclePermissionAttribute?displayProperty=nameWithType> は非推奨となりました。|

[先頭に戻る](#introduction)

<a name="design"></a>

### <a name="assembly-systemdesigndll"></a>アセンブリ: System.Design.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.ComponentModel.Design.LocalizationExtenderProvider?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。 代わりに <xref:System.ComponentModel.Design.Serialization.CodeDomLocalizationProvider?displayProperty=nameWithType> を使用してください|
|<xref:System.Web.UI.Design.DataBindingCollectionConverter?displayProperty=nameWithType>|データ バインディングの編集は、プロパティ グリッドではなく、<xref:System.ComponentModel.Design.DesignerActionList?displayProperty=nameWithType> から起動されるので、この型の使用は推奨されていません。|
|<xref:System.Web.UI.Design.DataBindingCollectionEditor?displayProperty=nameWithType>|データ バインディングの編集は、プロパティ グリッドではなく、<xref:System.ComponentModel.Design.DesignerActionList?displayProperty=nameWithType> から起動されるので、この型の使用は推奨されていません。|
|<xref:System.Web.UI.Design.IControlDesignerBehavior?displayProperty=nameWithType>|別の方法として、<xref:System.Web.UI.Design.IControlDesignerTag?displayProperty=nameWithType> および <xref:System.Web.UI.Design.IControlDesignerView?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.UI.Design.IHtmlControlDesignerBehavior?displayProperty=nameWithType>|別の方法として、<xref:System.Web.UI.Design.IControlDesignerTag?displayProperty=nameWithType> および <xref:System.Web.UI.Design.IControlDesignerView?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.UI.Design.ITemplateEditingFrame?displayProperty=nameWithType>|テンプレート編集は <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> で処理されるので、この型の使用は推奨されていません。 テンプレート編集をサポートするには、<xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> プロパティのテンプレート データを公開して、<xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType> を呼び出します。|
|<xref:System.Web.UI.Design.IWebFormReferenceManager?displayProperty=nameWithType>|別の方法として、<xref:System.Web.UI.Design.WebFormsReferenceManager?displayProperty=nameWithType> を使用することをお勧めします。 <xref:System.Web.UI.Design.WebFormsReferenceManager> は機能が追加されており、さらに機能を拡張できます。 <xref:System.Web.UI.Design.WebFormsReferenceManager> を取得するには、`RootDesigner.ReferenceManager` から <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> プロパティを使用します。|
|<xref:System.Web.UI.Design.IWebFormsDocumentService?displayProperty=nameWithType>|別の方法として、<xref:System.Web.UI.Design.WebFormsRootDesigner?displayProperty=nameWithType> を使用することをお勧めします。 <xref:System.Web.UI.Design.WebFormsRootDesigner> は機能が追加されており、さらに機能を拡張できます。 <xref:System.Web.UI.Design.WebFormsRootDesigner> を取得するには、<xref:System.Web.UI.Design.ControlDesigner.RootDesigner%2A> から <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> プロパティを使用します。|
|<xref:System.Web.UI.Design.ITemplateEditingService?displayProperty=nameWithType>|テンプレート編集は <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> で処理されるので、この型の使用は推奨されていません。 テンプレート編集をサポートするには、<xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> プロパティのテンプレート データを公開して、<xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType> を呼び出します。|
|<xref:System.Web.UI.Design.ReadWriteControlDesigner?displayProperty=nameWithType>|別の方法として、コンテンツの編集に <xref:System.Web.UI.Design.ContainerControlDesigner?displayProperty=nameWithType> が使用されるので、<xref:System.Web.UI.Design.EditableDesignerRegion?displayProperty=nameWithType> を使用することをお勧めします。 デザイナーの各領域を使用すると、編集する対象のコンテンツの制御を強化できます。|
|<xref:System.Web.UI.Design.TemplateEditingService?displayProperty=nameWithType>|テンプレート編集は <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> で処理されるので、この型の使用は推奨されていません。 テンプレート編集をサポートするには、<xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> プロパティのテンプレート データを公開して、<xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType> を呼び出します。|
|<xref:System.Web.UI.Design.TemplateEditingVerb?displayProperty=nameWithType>|テンプレート編集は <xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> で処理されるので、この型の使用は推奨されていません。 テンプレート編集をサポートするには、<xref:System.Web.UI.Design.ControlDesigner.TemplateGroups%2A?displayProperty=nameWithType> プロパティのテンプレート データを公開して、<xref:System.Web.UI.Design.ControlDesigner.SetViewFlags%2A?displayProperty=nameWithType> を呼び出します。|
|<xref:System.Web.UI.Design.WebControls.CalendarAutoFormatDialog?displayProperty=nameWithType>|[オートフォーマット] ダイアログ ボックスはデザイナー ホストによって起動されるので、この型の使用は推奨されていません。 使用できる [オートフォーマット] の一覧は、<xref:System.Web.UI.Design.ControlDesigner?displayProperty=nameWithType> プロパティの <xref:System.Web.UI.Design.ControlDesigner.AutoFormats%2A?displayProperty=nameWithType> に公開されています。|
|<xref:System.Web.UI.Design.WebControls.PanelDesigner?displayProperty=nameWithType>|別の方法として、コンテンツの編集に <xref:System.Web.UI.Design.WebControls.PanelContainerDesigner?displayProperty=nameWithType> が使用されるので、<xref:System.Web.UI.Design.EditableDesignerRegion?displayProperty=nameWithType> を使用することをお勧めします。 デザイナーの各領域を使用すると、編集する対象のコンテンツの制御を強化できます。|

[先頭に戻る](#introduction)

<a name="system"></a>

### <a name="assembly-systemdll"></a>アセンブリ: System.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.ComponentModel.IComNativeDescriptorHandler?displayProperty=nameWithType>|このインターフェイスの使用は非推奨とされました。 代わりに、<xref:System.ComponentModel.TypeDescriptionProvider?displayProperty=nameWithType> をハンドル型 <xref:System.ComponentModel.TypeDescriptor.ComObjectType%2A?displayProperty=nameWithType> に追加します。|
|<xref:System.ComponentModel.RecommendedAsConfigurableAttribute?displayProperty=nameWithType>|新しい設定モデルを操作するには、代わりに、<xref:System.ComponentModel.SettingsBindableAttribute?displayProperty=nameWithType> を使用してください。|
|<xref:System.ComponentModel.Design.Serialization.RootDesignerSerializerAttribute?displayProperty=nameWithType>|この属性は非推奨とされました。 代わりに <xref:System.ComponentModel.Design.Serialization.DesignerSerializerAttribute?displayProperty=nameWithType> を使用してください|
|<xref:System.Diagnostics.DiagnosticsConfigurationHandler?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。|
|<xref:System.Diagnostics.PerformanceCounterManager?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。 代わりに、<xref:System.Diagnostics.PerformanceCounter?displayProperty=nameWithType> クラスからパフォーマンス カウンターを使用してください。|
|<xref:System.Net.GlobalProxySelection?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。 グローバルな既定のプロキシにアクセスして設定するには、代わりに、<xref:System.Net.WebRequest.DefaultWebProxy%2A?displayProperty=nameWithType> を使用してください。 <xref:System.Net.GlobalProxySelection.GetEmptyWebProxy%2A?displayProperty=nameWithType> の代わりに、'null' を使用してください。|
|<xref:System.Net.Sockets.SocketClientAccessPolicyProtocol?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|

[先頭に戻る](#introduction)

<a name="enterpriseservices"></a>

### <a name="assembly-systementerpriseservicesdll"></a>アセンブリ: System.EnterpriseServices.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.EnterpriseServices.RegistrationHelperTx?displayProperty=nameWithType>|<xref:System.EnterpriseServices.RegistrationHelperTx> クラスの使用は非推奨とされました。|

[先頭に戻る](#introduction)

<a name="net"></a>

### <a name="assembly-systemnetdll"></a>アセンブリ: System.Net.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Net.INetworkProgress?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.IUnsafeWebRequestCreate?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.NetworkProgressChangedEventArgs?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.UiSynchronizationContext?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.Sockets.HttpPolicyDownloaderProtocol?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.Sockets.SecurityCriticalAction?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.Sockets.SocketPolicy?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.Sockets.UdpAnySourceMulticastClient?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Net.Sockets.UdpSingleSourceMulticastClient?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|

[先頭に戻る](#introduction)

<a name="servicemodel"></a>

### <a name="assembly-systemservicemodeldll"></a>アセンブリ: System.ServiceModel.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.ServiceModel.NetPeerTcpBinding?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|
|<xref:System.ServiceModel.Channels.HttpCookieContainerBindingElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型は互換性のために残されています。 Http <xref:System.Net.CookieContainer> を有効にするには、Http バインドか `AllowCookies` で <xref:System.ServiceModel.Channels.HttpTransportBindingElement> プロパティを使用します。|
|<xref:System.ServiceModel.Channels.PeerCustomResolverBindingElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|
|<xref:System.ServiceModel.Channels.PeerTransportBindingElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|
|<xref:System.ServiceModel.Configuration.NetPeerTcpBindingCollectionElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|
|<xref:System.ServiceModel.Configuration.NetPeerTcpBindingElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|
|<xref:System.ServiceModel.Configuration.PeerTransportElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|
|<xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> ピア チャネルの機能は互換性のために残されていますが、今後削除される予定です。|

[先頭に戻る](#introduction)

<a name="web"></a>

### <a name="assembly-systemwebdll"></a>アセンブリ: System.Web.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Web.Configuration.PassportAuthentication?displayProperty=nameWithType>|この型は互換性のために残されています。 パスポート認証製品はサポート対象から除外され、[Microsoft アカウント](https://account.microsoft.com/account/Account?destrt=home-index)に置き換えられました。|
|<xref:System.Web.Mail.MailAttachment?displayProperty=nameWithType>|別の方法として、<xref:System.Net.Mail.Attachment?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.Mail.MailEncoding?displayProperty=nameWithType>|別の方法として、<xref:System.Net.Mime.TransferEncoding?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.Mail.MailFormat?displayProperty=nameWithType>|別の方法として、<xref:System.Net.Mail.MailMessage.IsBodyHtml%2A?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.Mail.MailMessage?displayProperty=nameWithType>|別の方法として、<xref:System.Net.Mail.MailMessage?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.Mail.MailPriority?displayProperty=nameWithType>|別の方法として、<xref:System.Net.Mail.MailPriority?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.Mail.SmtpMail?displayProperty=nameWithType>|別の方法として、<xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType> を使用することをお勧めします。|
|<xref:System.Web.Security.PassportAuthenticationEventArgs?displayProperty=nameWithType>|この型は互換性のために残されています。 パスポート認証製品はサポート対象から除外され、[Microsoft アカウント](https://account.microsoft.com/account/Account?destrt=home-index)に置き換えられました。|
|<xref:System.Web.Security.PassportAuthenticationEventHandler?displayProperty=nameWithType>|この型は互換性のために残されています。 パスポート認証製品はサポート対象から除外され、[Microsoft アカウント](https://account.microsoft.com/account/Account?destrt=home-index)に置き換えられました。|
|<xref:System.Web.Security.PassportAuthenticationModule?displayProperty=nameWithType>|この型は互換性のために残されています。 パスポート認証製品はサポート対象から除外され、[Microsoft アカウント](https://account.microsoft.com/account/Account?destrt=home-index)に置き換えられました。|
|<xref:System.Web.Security.PassportIdentity?displayProperty=nameWithType>|この型は互換性のために残されています。 パスポート認証製品はサポート対象から除外され、[Microsoft アカウント](https://account.microsoft.com/account/Account?destrt=home-index)に置き換えられました。|
|<xref:System.Web.Security.PassportPrincipal?displayProperty=nameWithType>|この型は互換性のために残されています。 パスポート認証製品はサポート対象から除外され、[Microsoft アカウント](https://account.microsoft.com/account/Account?destrt=home-index)に置き換えられました。|
|<xref:System.Web.UI.ObjectConverter?displayProperty=nameWithType>|別の方法として、<xref:System.Convert?displayProperty=nameWithType> および <xref:System.String.Format%2A?displayProperty=nameWithType> を使用することをお勧めします。|

[先頭に戻る](#introduction)

<a name="mobile"></a>

### <a name="assembly-systemwebmobiledll"></a>アセンブリ: System.Web.Mobile.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Web.Mobile.CookielessData?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.DeviceFilterElement?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.DeviceFilterElementCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.DeviceFiltersSection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.ErrorHandlerModule?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.MobileCapabilities?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.MobileDeviceCapabilitiesSectionHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.MobileErrorInfo?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.Mobile.MobileFormsAuthentication?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.Design.MobileControls.IMobileDesigner?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.Design.MobileControls.IMobileWebFormServices?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.Design.MobileControls.MobileResource?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.Design.MobileControls.Converters.DataFieldConverter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.Design.MobileControls.Converters.DataMemberConverter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.AdRotator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Alignment?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ArrayListCollectionBase?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.BaseValidator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.BooleanOption?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Calendar?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Command?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.CommandFormat?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.CompareValidator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Constants?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ControlElement?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ControlElementCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ControlPager?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.CustomValidator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DesignerAdapterAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceElement?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceElementCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceOverridableAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecific?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoice?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceTemplateBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificChoiceTemplateContainer?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.DeviceSpecificControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ErrorFormatterPage?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.FontInfo?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.FontSize?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Form?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.FormControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.FormMethod?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.IControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Image?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.IObjectListFieldCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.IPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ItemPager?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ITemplateable?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Label?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Link?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.List?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListCommandEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListCommandEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListDataBindEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListDataBindEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListDecoration?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ListSelectType?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.LiteralLink?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.LiteralText?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.LiteralTextContainerControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.LiteralTextControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.LoadItemsEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.LoadItemsEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileControl?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileControlsSection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileControlsSectionHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileListItem?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileListItemCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileListItemType?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobilePage?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileTypeNameConverter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.MobileUserControl?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectList?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListCommand?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListCommandCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListCommandEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListCommandEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListDataBindEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListDataBindEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListField?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListFieldCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListItem?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListItemCollection?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListSelectEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListSelectEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListShowCommandsEventArgs?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListShowCommandsEventHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListTitleAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ObjectListViewMode?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.PagedControl?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.PagerStyle?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Panel?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.PanelControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.PersistNameAttribute?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.PhoneCall?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.RangeValidator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.RegularExpressionValidator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.RequiredFieldValidator?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.SelectionList?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Style?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.StyleSheet?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.StyleSheetControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.TemplateContainer?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.TextBox?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.TextBoxControlBuilder?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.TextControl?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.TextView?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.TextViewElement?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.ValidationSummary?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Wrapping?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ChtmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.ControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLabelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlLiteralTextAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlObjectListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPanelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlTextViewAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlValidationSummaryAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.HtmlValidatorAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.MobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.MultiPartWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.UpWmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.UpWmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLabelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlLiteralTextAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlObjectListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPanelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlPostFieldType?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlTextViewAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlValidationSummaryAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.WmlValidatorAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.Doctype?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.StyleSheetLocation?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCalendarAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCommandAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlControlAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlCssHandler?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlFormAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlImageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLabelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLinkAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlLiteralTextAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlMobileTextWriter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlObjectListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPageAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPanelAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlPhoneCallAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlSelectionListAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlTextBoxAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlTextViewAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlValidationSummaryAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|
|<xref:System.Web.UI.MobileControls.Adapters.XhtmlAdapters.XhtmlValidatorAdapter?displayProperty=nameWithType>|System.Web.Mobile.dll アセンブリの使用は非推奨とされました。今後は使用しないでください。 ASP.NET モバイル アプリケーションを開発する方法については、[モバイル向け ASP.NET](/aspnet/mobile/overview) に関するページをご覧ください。|

[先頭に戻る](#introduction)

<a name="workflow_activities"></a>

### <a name="assembly-systemworkflowactivitiesdll"></a>アセンブリ: System.Workflow.Activities.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Workflow.Activities?displayProperty=nameWithType> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Activities.Configuration.ActiveDirectoryRoleFactoryConfiguration?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Activities.Rules.RuleActionTrackingEvent?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Activities.Rules.RuleConditionReference?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Activities.Rules.RuleSetReference?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|

[先頭に戻る](#introduction)

<a name="workflow_componentmodel"></a>

### <a name="assembly-systemworkflowcomponentmodeldll"></a>アセンブリ: System.Workflow.ComponentModel.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Workflow.ComponentModel> と <xref:System.Workflow.ComponentModel.GetValueOverride?displayProperty=nameWithType> を除く <xref:System.Workflow.ComponentModel.SetValueOverride?displayProperty=nameWithType> 名前空間のすべての型。|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Compiler> と <xref:System.Workflow.ComponentModel.Compiler.ValidationError?displayProperty=nameWithType> を除く <xref:System.Workflow.ComponentModel.Compiler.ValidationErrorCollection?displayProperty=nameWithType> 名前空間のすべての型。|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Design> を除く<xref:System.Workflow.ComponentModel.Design.ConnectorEventHandler> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityCodeDomSerializationManager?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityCodeDomSerializer?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityMarkupSerializer?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivitySurrogateSelector?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.ActivityTypeCodeDomSerializer?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.CompositeActivityMarkupSerializer?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.ComponentModel.Serialization.DependencyObjectCodeDomSerializer?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|

[先頭に戻る](#introduction)

<a name="workflow_runtime"></a>

### <a name="assembly-systemworkflowruntimedll"></a>アセンブリ: System.Workflow.Runtime.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Activities.Statements.Interop?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br />Workflow Foundation 3.0 の型の使用は非推奨とされました。 代わりに、<xref:System.Activities>.\* の Workflow 4.0 の型を使用してください。|
|<xref:System.Activities.Tracking.InteropTrackingRecord?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br />Workflow Foundation 3.0 の型の使用は非推奨とされました。 代わりに、<xref:System.Activities>.\* の Workflow 4.0 の型を使用してください。|
|<xref:System.Workflow.Runtime> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Runtime.Configuration> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Runtime.DebugEngine> を除く<xref:System.Workflow.Runtime.DebugEngine.DebugEngineCallback> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Runtime.Hosting> を除く<xref:System.Workflow.Runtime.Hosting.WorkflowCommitWorkBatchService.CommitWorkBatchCallback> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|
|<xref:System.Workflow.Runtime.Tracking> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> System.Workflow.\* 型の使用は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい型を使用してください。|

[先頭に戻る](#introduction)

<a name="workflowservices"></a>

### <a name="assembly-systemworkflowservicesdll"></a>アセンブリ: System.WorkflowServices.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.ServiceModel.WorkflowServiceHost?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Activation.WorkflowServiceHostFactory?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Activities.Description.WorkflowRuntimeEndpoint?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Configuration.ExtendedWorkflowRuntimeServiceElementCollection?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Configuration.PersistenceProviderElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Configuration.WorkflowRuntimeElement?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Description.DurableOperationAttribute?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Description.DurableServiceAttribute?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Description.PersistenceProviderBehavior?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Description.UnknownExceptionAction?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Description.WorkflowRuntimeBehavior?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Dispatcher.DurableOperationContext?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.InstanceLockException?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.InstanceNotFoundException?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.LockingPersistenceProvider?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.PersistenceException?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.PersistenceProvider?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.PersistenceProviderFactory?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.ServiceModel.Persistence.SqlPersistenceProviderFactory?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.Workflow.Activities?displayProperty=nameWithType> 名前空間のすべての型|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|
|<xref:System.Workflow.Runtime.Hosting.ChannelManagerService?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> WF 3 の型は非推奨とされます。 代わりに、<xref:System.Activities>.\* の新しい WF 4 型を使用してください。|

[先頭に戻る](#introduction)

<a name="xaml"></a>

### <a name="assembly-systemxamldll"></a>アセンブリ: System.Xaml.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Windows.Markup.AcceptedMarkupExtensionExpressionTypeAttribute?displayProperty=nameWithType>|これは、XAML パーサーでは使用されなくなりました。 <xref:System.Windows.Markup.XamlSetMarkupExtensionAttribute?displayProperty=nameWithType> を参照してください。|

[先頭に戻る](#introduction)

<a name="xml"></a>

### <a name="assembly-systemxmldll"></a>アセンブリ: System.Xml.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Xml.IApplicationResourceStreamResolver?displayProperty=nameWithType>|最初に .NET Framework 4.5 で非推奨とされました。<br /><br /> この型を使用すると、コンパイラ エラーが発生します。<br /><br /> この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Xml.Schema.XmlSchemaCollection?displayProperty=nameWithType>|スキーマ コンパイルおよび検証には、<xref:System.Xml.Schema.XmlSchemaSet?displayProperty=nameWithType> を使用してください。|
|<xref:System.Xml.XmlValidatingReader?displayProperty=nameWithType>|代わりに、適切な <xref:System.Xml.XmlReader?displayProperty=nameWithType> を使用する <xref:System.Xml.XmlReader.Create%2A?displayProperty=nameWithType> メソッドによって作成された <xref:System.Xml.XmlReaderSettings?displayProperty=nameWithType> を使用してください。|
|<xref:System.Xml.XmlXapResolver?displayProperty=nameWithType>|この型を使用すると、コンパイラ エラーが発生します。 この API は、.NET Framework インフラストラクチャをサポートします。独自に作成したコードから直接使用するためのものではありません。|
|<xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。 代わりに、<xref:System.Xml.Xsl.XslCompiledTransform?displayProperty=nameWithType> を使用してください。|

[先頭に戻る](#introduction)

<a name="WindowsBase"></a>

### <a name="assembly-windowsbasedll"></a>アセンブリ: WindowsBase.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:System.Windows.Markup.IReceiveMarkupExtension?displayProperty=nameWithType>|<xref:System.Windows.Markup.IReceiveMarkupExtension?displayProperty=nameWithType> は非推奨となりました。 このインターフェイスは、使用されなくなりました。|

[先頭に戻る](#introduction)

<a name="obsolete_types_in_microsoft_assemblies"></a>

## <a name="obsolete-types-in-microsoft-assemblies"></a>互換性のために残されている Microsoft アセンブリの型

以下のセクションで、互換性のために残されている Microsoft アセンブリの型を示します。 これらのアセンブリは、個別の言語 (たとえば、Microsoft.JScript.dll や Microsoft.VisualC.dll) を対象とした特殊な目的のアセンブリです。

<a name="IEHost"></a>

### <a name="assembly-iehostdll-and-ieexecexe"></a>アセンブリ: IEHost.dll and IEExec.exe

IEHost.dll および IEExec.exe アセンブリは .NET Framework から削除されています。 そのすべての型およびメンバーは廃止され、.NET Framework 4 以降はサポートされていません。 これらのアセンブリは Internet Explorer で Windows フォーム コントロールをホストし、実行可能ファイルを実行するために使用されていました。 代替手段としては、ClickOnce、XAML ブラウザー アプリケーション (XBAP)、および Microsoft Silverlight をお勧めします。

[先頭に戻る](#introduction)

<a name="Engine"></a>

### <a name="assembly-microsoftbuildenginedll"></a>アセンブリ: Microsoft.Build.Engine.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:Microsoft.Build.BuildEngine.Engine?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。 代わりに、<xref:Microsoft.Build.Evaluation.ProjectCollection?displayProperty=nameWithType>Microsoft.Build*アセンブリの* を使用してください。|
|<xref:Microsoft.Build.BuildEngine.Project?displayProperty=nameWithType>|このクラスの使用は非推奨とされました。 代わりに、<xref:Microsoft.Build.Evaluation.ProjectCollection?displayProperty=nameWithType>Microsoft.Build*アセンブリの* を使用してください。|

[先頭に戻る](#introduction)

<a name="jscript"></a>

### <a name="assembly-microsoftjscriptdll"></a>アセンブリ: Microsoft.JScript.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:Microsoft.JScript.Vsa.BaseVsaEngine?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.BaseVsaSite?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.BaseVsaStartup?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaCodeItem?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaEngine?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaError?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaGlobalItem?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaItem?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaItems?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaPersistSite?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaReferenceItem?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.IJSVsaSite?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.JSVsaError?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.JSVsaException?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.JSVsaItemFlag?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.JSVsaItemType?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.ResInfo?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|
|<xref:Microsoft.JScript.Vsa.VsaEngine?displayProperty=nameWithType>|Visual Studio 2005 で、この型は非推奨となりました。この機能に代わるものはありません。 詳細については、<xref:System.CodeDom.Compiler.ICodeCompiler?displayProperty=nameWithType> のドキュメントを参照してください。|

[先頭に戻る](#introduction)

<a name="VBCompat"></a>

### <a name="assembly-microsoftvisualbasiccompatibilitydll"></a>アセンブリ: Microsoft.VisualBasic.Compatibility.dll

Visual Basic 6 から移行する方法については、「[Visual Basic 6.0 リソース センター](https://docs.microsoft.com/previous-versions/visualstudio/visual-basic-6/visual-basic-6.0-documentation)」を参照してください。

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseControlArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseOcxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ButtonArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CheckBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CheckedListBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ColorDialogArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ComboBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DirListBox?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DirListBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DriveListBox?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DriveListBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FileListBox?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FileListBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FixedLengthString?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FontDialogArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.FormShowConstants?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.GroupBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.HScrollBarArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ImageListArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.LabelArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListBoxItem?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ListViewArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.LoadResConstants?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MaskedTextBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MenuItemArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MouseButtonConstants?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.OpenFileDialogArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PanelArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PictureBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.PrintDialogArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ProgressBarArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.RadioButtonArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.RichTextBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.SaveFileDialogArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ScaleMode?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ShiftConstants?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.StatusBarArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.StatusStripArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.Support?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TabControlArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TextBoxArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TimerArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolBarArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolStripArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ToolStripMenuItemArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.TreeViewArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.VScrollBarArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebBrowserArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClass?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassContainingClassNotOptional?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassCouldNotFindEvent?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassNextItemCannotBeCurrentWebItem?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassNextItemRespondNotFound?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassUserWebClassNameNotOptional?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassWebClassFileNameNotOptional?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebClassWebItemNotValid?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItem?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemAssociatedWebClassNotOptional?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemClosingTagNotFound?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemCouldNotLoadEmbeddedResource?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemCouldNotLoadTemplateFile?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemNameNotOptional?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemNoTemplateSpecified?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemTooManyNestedTags?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.WebItemUnexpectedErrorReadingTemplateFile?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ZOrderConstants?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|

[先頭に戻る](#introduction)

<a name="VBCompatData"></a>

### <a name="assembly-microsoftvisualbasiccompatibilitydatadll"></a>アセンブリ: Microsoft.VisualBasic.Compatibility.Data.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.BOFActionEnum?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.EndOfRecordsetDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.EOFActionEnum?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.ErrorDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FetchCompleteDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FetchProgressDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.FieldChangeCompleteDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.MoveCompleteDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.OrientationEnum?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.RecordChangeCompleteDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.RecordsetChangeCompleteDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeFieldDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeRecordDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillChangeRecordsetDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODC.WillMoveDelegate?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.ADODCArray?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BaseDataEnvironment?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.BindingCollectionEnumerator?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.CONNECTDATA?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBBINDING?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBCOLUMNINFO?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBID?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBinding?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBindingCollection?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBKINDENUM?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.DBPROPIDSET?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IAccessor?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IChapteredRowset?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IColumnsInfo?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IConnectionPoint?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IConnectionPointContainer?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IDataFormat?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IDataFormatDisp?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IEnumConnectionPoints?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IEnumConnections?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowPosition?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowPositionChange?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowset?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetChange?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetIdentity?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetInfo?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.IRowsetNotify?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MBinding?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.MBindingCollection?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.SRDescriptionAttribute?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UGUID?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UNAME?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|
|<xref:Microsoft.VisualBasic.Compatibility.VB6.UpdateMode?displayProperty=nameWithType>|このメンバーは使用されなくなりました。|

[先頭に戻る](#introduction)

<a name="visualc"></a>

### <a name="assembly-microsoftvisualcdll"></a>アセンブリ: Microsoft.VisualC.dll

|[種類]|[メッセージ]|
|----------|-------------|
|<xref:Microsoft.VisualC.DebugInfoInPDBAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.DecoratedNameAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.IsConstModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.IsCXXReferenceModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.IsLongModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.IsSignedModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.IsVolatileModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.MiscellaneousBitsAttribute?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.NeedsCopyConstructorModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|
|<xref:Microsoft.VisualC.NoSignSpecifiedModifier?displayProperty=nameWithType>|Microsoft.VisualC.dll は互換性のために残されているアセンブリであり、下位互換性のためだけに存在します。|

## <a name="see-also"></a>参照

- [クラス ライブラリ内にある旧版のもの](whats-obsolete.md)
- [互換性のために残されているメンバー](obsolete-members.md)
