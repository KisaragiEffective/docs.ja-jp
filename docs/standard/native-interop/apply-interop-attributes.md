---
title: 相互運用固有の属性の適用
ms.date: 03/30/2017
helpviewer_keywords:
- design-time attributes
- .NET Framework, exposing components to COM
- attributes [.NET Framework], design-time functionality
- conversion-tool attributes
- attributes [.NET Framework], interop-specific
- attributes [.NET Framework], conversion-tool
- interoperation with unmanaged code, applying attributes
- interoperation with unmanaged code, exposing .NET Framework components
- COM interop, exposing COM components
- COM interop, applying attributes
ms.assetid: b6014613-641c-4912-9e2f-83a99210a037
ms.openlocfilehash: ca104c512641774217de5e270dc50b7393fc5725
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159248"
---
# <a name="applying-interop-attributes"></a>相互運用固有の属性の適用
<xref:System.Runtime.InteropServices> 名前空間では、デザイン時にユーザーが適用する属性、変換処理中に COM 相互運用ツールおよび API によって適用される属性、ユーザーまたは COM 相互運用機能によって適用される属性という 3 つのカテゴリの相互運用固有の属性が提供されます。  
  
 マネージド コードに属性を適用する作業に慣れていない場合は、「[属性を使用したメタデータの拡張](../../../docs/standard/attributes/index.md)」を参照してください。 他のカスタム属性の場合と同様に、相互運用固有の属性は、型、メソッド、プロパティ、パラメーター、フィールド、およびその他のメンバーに対して適用できます。  
  
## <a name="design-time-attributes"></a>デザイン時属性  
 COM 相互運用ツールおよび API によって実行された変換処理の結果を調整するには、デザイン時属性を使用します。 マネージド ソース コードに適用できる属性の説明を次の表に示します。 COM 相互運用ツールで、この表に示す属性を適用できる場合もあります。  
  
|Attribute|Description|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.AutomationProxyAttribute>|Automation Marshaler またはカスタムのプロキシとスタブを使用して、型をマーシャリングするかどうかを指定します。|  
|<xref:System.Runtime.InteropServices.ClassInterfaceAttribute>|クラスに対して生成されたインターフェイスの型を制御します。|  
|<xref:System.Runtime.InteropServices.CoClassAttribute>|タイプ ライブラリからインポートされた元のコクラスの CLSID を識別します。<br /><br /> COM 相互運用ツールでは、通常、この属性が適用されます。|  
|<xref:System.Runtime.InteropServices.ComImportAttribute>|コクラスまたはインターフェイス定義が COM タイプ ライブラリからインポートされたことを示します。 ランタイムはこのフラグを使用して、型をアクティブ化およびマーシャリングする方法を認識します。 この属性は、型がタイプ ライブラリにエクスポートされることを禁止します。<br /><br /> COM 相互運用ツールでは、通常、この属性が適用されます。|  
|<xref:System.Runtime.InteropServices.ComRegisterFunctionAttribute>|COM から使用するためにアセンブリを登録するときに、メソッドが呼び出されるようにします。これで、登録処理中にユーザー作成コードを実行できます。|  
|<xref:System.Runtime.InteropServices.ComSourceInterfacesAttribute>|クラスのイベントの発生元になるインターフェイスを識別します。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.ComUnregisterFunctionAttribute>|アセンブリが COM から登録解除されるときにメソッドが呼び出されるようにします。これで、処理中にユーザー作成コードを実行できます。|  
|<xref:System.Runtime.InteropServices.ComVisibleAttribute>|属性値が **false** の場合、型は COM から参照できなくなります。 この属性は、COM の参照可能範囲を制御するために、個別の型またはアセンブリ全体に適用できます。 既定では、すべてのパブリックなマネージド型は参照可能なので、この属性でこれらの型を参照可能にする必要はありません。|  
|<xref:System.Runtime.InteropServices.DispIdAttribute>|メソッドまたはフィールドの COM ディスパッチ識別子 (DISPID) を指定します。 この属性には、対象のメソッド、フィールド、またはプロパティの DISPID が含まれています。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|
|<xref:System.Runtime.InteropServices.ComDefaultInterfaceAttribute>|.NET で実装されている COM クラスの既定のインターフェイスを示します。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|
|<xref:System.Runtime.InteropServices.FieldOffsetAttribute>|**StructLayoutAttribute** と共に使用される場合は、クラス内の各フィールドの物理的位置を示します。**LayoutKind** は Explicit に設定されます。|  
|<xref:System.Runtime.InteropServices.GuidAttribute>|クラス、インターフェイス、またはタイプ ライブラリ全体のグローバル一意識別子 (GUID) を指定します。 属性に渡される文字列は、**System.Guid** 型の受け入れ可能なコンストラクター引数の形式である必要があります。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.IDispatchImplAttribute>|COM にデュアル インターフェイスやディスパッチ インターフェイスを公開するときに、共通言語ランタイムで使用する **IDispatch** インターフェイスの実装の種類を示します。|  
|<xref:System.Runtime.InteropServices.InAttribute>|呼び出し元にデータをマーシャリングすることを示します。 属性パラメーターに使用できます。|  
|<xref:System.Runtime.InteropServices.InterfaceTypeAttribute>|マネージド インターフェイスを COM クライアントに公開する方法を制御します (デュアル、IUnknown から派生、または IDispatch のみ)。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.LCIDConversionAttribute>|アンマネージ メソッド シグネチャで LCID パラメーターが必要であることを示します。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.MarshalAsAttribute>|マネージド コードとアンマネージド コードとの間で、フィールドまたはパラメーター内のデータをマーシャリングする方法を示します。 各データ型には既定のマーシャリング動作があるため、この属性は常に省略可能です。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.OptionalAttribute>|パラメーターが省略可能であることを示します。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.OutAttribute>|フィールドまたはパラメーター内のデータが、呼び出されたオブジェクトから呼び出し元に返されるときに、マーシャリングされる必要があることを示します。|  
|<xref:System.Runtime.InteropServices.PreserveSigAttribute>|通常は相互運用呼び出し時に発生する、HRESULT または retval シグネチャ変換を抑止します。 この属性は、タイプ ライブラリのエクスポートだけでなく、マーシャリングにも影響します。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
|<xref:System.Runtime.InteropServices.ProgIdAttribute>|.NET Framework クラスの ProgID を指定します。 属性パラメーターに使用できます。|  
|<xref:System.Runtime.InteropServices.StructLayoutAttribute>|クラスのフィールドの物理レイアウトを制御します。<br /><br /> COM 相互運用ツールでは、この属性を適用できます。|  
  
## <a name="conversion-tool-attributes"></a>変換ツール属性  
 変換処理中に COM 相互運用ツールが適用する属性の説明を次の表に示します。 これらの属性は、デザイン時には適用しません。  
  
|Attribute|Description|  
|---------------|-----------------|  
|<xref:System.Runtime.InteropServices.ComAliasNameAttribute>|パラメーターまたはフィールドの種類の COM エイリアスを示します。 属性パラメーター、フィールド、または戻り値に使用できます。|  
|<xref:System.Runtime.InteropServices.ComConversionLossAttribute>|クラスまたはインターフェイスに関する情報が、タイプ ライブラリからアセンブリにインポートされたときに失われたことを示します。|  
|<xref:System.Runtime.InteropServices.ComEventInterfaceAttribute>|ソース インターフェイスと、イベント インターフェイスのメソッドを実装するクラスを識別します。|  
|<xref:System.Runtime.InteropServices.ImportedFromTypeLibAttribute>|アセンブリが元は COM タイプ ライブラリからインポートされたことを示します。 この属性には、元のタイプ ライブラリのタイプ ライブラリ定義が含まれます。|  
|<xref:System.Runtime.InteropServices.TypeLibFuncAttribute>|元はこの関数のために COM タイプ ライブラリからインポートされた **FUNCFLAGS** が含まれます。|  
|<xref:System.Runtime.InteropServices.TypeLibTypeAttribute>|元はこの型のために COM タイプ ライブラリからインポートされた **TYPEFLAGS** が含まれます。|  
|<xref:System.Runtime.InteropServices.TypeLibVarAttribute>|元はこの変数のために COM タイプ ライブラリからインポートされた **VARFLAGS** が含まれます。|  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices>
- [COM への .NET Framework コンポーネントの公開](../../../docs/framework/interop/exposing-dotnet-components-to-com.md)
- [属性](../../../docs/standard/attributes/index.md)
- [要件 (相互運用のための .NET 型の)](../../../docs/standard/native-interop/qualify-net-types-for-interoperation.md)
- [COM 用の .NET Framework アセンブリのパッケージ化](../../../docs/framework/interop/packaging-an-assembly-for-com.md)
