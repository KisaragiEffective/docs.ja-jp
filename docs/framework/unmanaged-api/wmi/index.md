---
title: WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)
description: WMI 用の .NET Framework アンマネージド API およびパフォーマンス カウンターに関する概要を示します。
ms.date: 11/06/2017
ms.openlocfilehash: f28cd25ee6c3511dc5ac8a6dd4076c81f43fe74a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73127420"
---
# <a name="windows-management-instrumentation-wmi-and-performance-counters-unmanaged-api-reference"></a>Windows Management Instrumentation (WMI) およびパフォーマンス カウンター (アンマネージ API リファレンス)

.NET Framework WMI およびパフォーマンス カウンターのアンマネージド API は、[ネイティブの Windows Management Instrumentation API](/windows/desktop/WmiSdk/com-api-for-wmi) の呼び出しをラップする一連の関数によって構成されています。 これを使用することで、リモート コンピューター システムの管理および監視を行うツールおよびライブラリを開発することができます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

API には次の関数が含まれています。

| Function | [説明] |
|---------|---------|
| [BeginEnumeration 関数](beginenumeration.md) | 列挙子が WMI オブジェクト プロパティの列挙型の先頭にリセットされます。 |
| [BeginMethodEnumeration 関数](beginmethodenumeration.md) |  オブジェクトで使用可能なメソッドの列挙型が開始されます。 |
| [BlessIWbemServices 関数](blessiwbemservices.md) | 指定した IWbemServices クラスへのアクセスがユーザーの資格情報によって許可されるかどうかを示されます。 |
| [BlessIWbemServicesObject 関数](blessiwbemservicesobject.md) | 指定した IWbem サービス オブジェクリックへのアクセスがユーザーの資格情報によって許可されるかどうかを示されます。 |
| [Clone 関数](clone.md) | 現在のオブジェクトの完全な複製である新しいオブジェクトが返されます。 |
| [CloneEnumWbemClassObject 関数](cloneenumwbemclassobject.md) | 列挙型内での位置を維持して、列挙子の論理コピーが作成されます。 |
| [CompareTo 関数](compareto.md) | オブジェクトが、別の Windows 管理オブジェクトと比較されます。 |
| [ConnectServerWmi 関数](connectserverwmi.md) | 指定したコンピューターにある WMI 名前空間との接続が DCOM 経由で作成されます。 |
| [CreateClassEnumWmi 関数](createclassenumwmi.md) | 指定した選択条件を満たしたすべてのクラスに対する列挙子が返されます。 |
| [CreateInstanceEnumWmi 関数](createinstanceenumwmi.md) | 指定したクラスのインスタンスの中から指定した選択条件を満たすものを返す列挙子が返されます。 |
| [Delete 関数](delete.md) | クラスの定義とそのすべての修飾子から、指定したプロパティが削除されます。 |
| [DeleteMethod 関数](deletemethod.md) | CIM クラスの定義から、指定したメソッドが削除されます。 |
| [EndEnumeration 関数](endenumeration.md) | 列挙型シーケンスが終了されます。 |
| [EndMethodEnumeration 関数](endmethodenumeration.md) | [BeginMethodEnumeration 関数](beginmethodenumeration.md)を呼び出して開始された列挙型シーケンスが終了されます。 |
| [ExecNotificationQueryWmi 関数](execnotificationquerywmi.md) | イベントを受信するクエリが実行されます。 |
| [ExecQueryWmi 関数](execquerywmi.md) | オブジェクトを取得するクエリが実行されます。 |
| [FormatFromRawValue 関数](formatfromrawvalue.md) | 1 つの生のパフォーマンス データ値が指定した形式に変換されます。この形式変換が時間ベースである場合は、2 つの生のパフォーマンス データ値が変換されます。 |
| [Get 関数](get.md) | 指定したプロパティ値が存在する場合、その値が取得されます。 |
| [GetCurrentApartmentType 関数](getcurrentapartmenttype.md) | 呼び出し元が実行されているアパートメントの種類が取得されます。 |
| [GetDemultiplexedStub 関数](getdemultiplexedstub.md) | Windows 管理から非同期呼び出しを受信する際にクライアントを支援するオブジェクト転送シンクが作成されます。 |
| [GetErrorInfo 関数](geterrorinfo.md) | 前の関数呼び出しからエラー情報が取得されます。 |
| [GetMethod 関数](getmethod.md) | 指定したメソッドに関する情報が取得されます。 |
| [GetMethodOrigin 関数](getmethodorigin.md) | メソッドを宣言しているクラスが特定されます。 |
| [GetMethodQualifierSet 関数](getmethodqualifierset.md) | 特定のメソッドで設定された修飾子が取得されます。 |
| [GetNames 関数](getnames.md) | オブジェクトのプロパティの名前の一部またはすべてが取得されます。 |
| [GetObjectText 関数](getobjecttext.md) | MOF 構文内のオブジェクトのテキストのレンダリングが返されます。 |
| [GetPropertyHandle 関数](getpropertyhandle.md) | プロパティを識別する一意のハンドルが返されます。 |
| [GetPropertyOrigin 関数](getpropertyorigin.md) | プロパティを宣言しているクラスが特定されます。 |
| [GetPropertyQualifierSet 関数](getpropertyqualifierset.md) | 特定のプロパティで設定された修飾子が取得されます。  |
| [GetQualifierSet 関数](getqualifierset.md) | クラス インスタンスまたはクラス定義で設定された修飾子が取得されます。 |
| [InheritsFrom 関数](inheritsfrom.md) | 指定した親クラスから現在のクラスまたはインスタンスが派生しているかどうかが判定されます。 |
| [Initialize 関数](initialize.md) | WMI の初期化が実行されます。 |
| [Next 関数](next.md) | 列挙型内の次のプロパティが取得されます。 |
| [NextMethod 関数](nextmethod.md) | 列挙型内の次のメソッドが取得されます。 |
| [Put 関数](put.md) | 名前付きプロパティが新しい値に設定されます。 |
| [PutClassWmi 関数](putclasswmi.md) | 新しいクラスが作成されるか、既存のクラスが更新されます。 |
| [PutInstanceWmi 関数](putinstancewmi.md) | 既存のクラスのインスタンスが作成または更新されます。 インスタンスは、WMI リポジトリに書き込まれます。 |
| [PutMethod 関数](putmethod.md) | メソッドが作成されます。 |
| [QualifierSet_BeginEnumeration 関数](qualifierset-beginenumeration.md) | オブジェクトの修飾子の列挙子が列挙型の先頭にリセットされます。 |
| [QualifierSet_Delete 関数](qualifierset-delete.md) | 名前によって指定した修飾子が削除されます。  |
| [QualifierSet_EndEnumeration 関数](qualifierset-endenumeration.md) | `QualifierSet_BeginEnumeration`関数の呼び出しで開始された列挙型が終了されます。 |
| [QualifierSet_Get 関数](qualifierset-get.md) | 指定した名前付き修飾子が取得されます。  |
| [QualifierSet_GetNames 関数](qualifierset-getnames.md) | 現在のオブジェクトまたはプロパティから使用できるすべての修飾子または特定の修飾子の名前が取得されます。 |
| [QualifierSet_Next 関数](qualifierset-next.md) | [QualifierSet_BeginEnumeration](qualifierset-beginenumeration.md) 関数の呼び出しによって開始された列挙型内の次の修飾子が返されます。 |
| [QualifierSet_Put 関数](qualifierset-put.md) | 名前付き修飾子と値が書き込まれます。 |
| [ResetSecurity 関数](resetsecurity.md) | 指定した偽装トークンが現在のスレッドに割り当てられます。 |
| [SetSecurity 関数](setsecurity.md) | 現在のスレッドに関連付けられている偽装トークンが取得されます。 |
| [SpawnDerivedClass 関数](spawnderivedclass.md) | 指定したオブジェクトから新たに派生するクラス オブジェクトが作成されます。 |
| [SpawnInstance 関数](spawninstance.md) | クラスの新しいインスタンスが作成されます。 |
| [VerifyClient 関数](verifyclientkey.md) | クライアント キーに適切なセキュリティが確実に含められます。 |
| [WritePropertyValue 関数](writepropertyvalue.md) | 指定したバイト数が、プロパティ ハンドルによって識別されるプロパティに書き込まれます。 |

## <a name="see-also"></a>参照

- [アンマネージド API リファレンス](../index.md)
