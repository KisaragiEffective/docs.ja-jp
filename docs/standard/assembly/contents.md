---
title: アセンブリの内容
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], multifile
- assemblies [.NET Framework], single-file
- assemblies [.NET Core]
- single-file assemblies
- multifile assemblies [.NET Framework]
ms.assetid: 28116714-da77-45f7-826d-fa035d121948
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 6d9268b0ec1ea919730a2ebcd209507692284cc6
ms.sourcegitcommit: 7b1ce327e8c84f115f007be4728d29a89efe11ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2019
ms.locfileid: "70972823"
---
# <a name="assembly-contents"></a><span data-ttu-id="f3593-102">アセンブリの内容</span><span class="sxs-lookup"><span data-stu-id="f3593-102">Assembly contents</span></span>
<span data-ttu-id="f3593-103">一般に、静的アセンブリは次の 4 つの要素から構成されます。</span><span class="sxs-lookup"><span data-stu-id="f3593-103">In general, a static assembly can consist of four elements:</span></span>

- <span data-ttu-id="f3593-104">アセンブリ メタデータを保持する[アセンブリ マニフェスト](manifest.md)。</span><span class="sxs-lookup"><span data-stu-id="f3593-104">The [assembly manifest](manifest.md), which contains assembly metadata.</span></span>

- <span data-ttu-id="f3593-105">型メタデータ。</span><span class="sxs-lookup"><span data-stu-id="f3593-105">Type metadata.</span></span>  

- <span data-ttu-id="f3593-106">型を実装する MSIL (Microsoft Intermediate Language) コード。</span><span class="sxs-lookup"><span data-stu-id="f3593-106">Microsoft intermediate language (MSIL) code that implements the types.</span></span> <span data-ttu-id="f3593-107">コンパイラによって 1 つ以上のソース コード ファイルから生成されます。</span><span class="sxs-lookup"><span data-stu-id="f3593-107">It is generated by the compiler from one or more source code files.</span></span>

- <span data-ttu-id="f3593-108">一連のリソース。</span><span class="sxs-lookup"><span data-stu-id="f3593-108">A set of resources.</span></span>  

<span data-ttu-id="f3593-109">このうち必須なのはアセンブリ マニフェストだけですが、アセンブリになんらかの機能を与えるには、型またはリソースのいずれかが必要になります。</span><span class="sxs-lookup"><span data-stu-id="f3593-109">Only the assembly manifest is required, but either types or resources are needed to give the assembly any meaningful functionality.</span></span>

<span data-ttu-id="f3593-110">次の図では、これらの要素を 1 つの物理ファイルにグループ化したものが示されています。</span><span class="sxs-lookup"><span data-stu-id="f3593-110">The following illustration shows these elements grouped in a single physical file.</span></span>

![MyAssembly.dll という名前のシングルファイル アセンブリを示す図。](./media/contents/single-file-assembly.gif)

<span data-ttu-id="f3593-112">この例では、MyAssembly.dll に含まれるアセンブリ マニフェストに記述されているように、3 つのファイルがすべて 1 つのアセンブリに属しています。</span><span class="sxs-lookup"><span data-stu-id="f3593-112">In this illustration, all three files belong to an assembly, as described in the assembly manifest contained in MyAssembly.dll.</span></span> <span data-ttu-id="f3593-113">ファイル システムにとっては、これらは 3 つの独立したファイルです。</span><span class="sxs-lookup"><span data-stu-id="f3593-113">To the file system, they are three separate files.</span></span> <span data-ttu-id="f3593-114">Util.netmodule というファイルは、アセンブリ情報を含んでいないため、モジュールとしてコンパイルされています。</span><span class="sxs-lookup"><span data-stu-id="f3593-114">Note that the file Util.netmodule was compiled as a module because it contains no assembly information.</span></span> <span data-ttu-id="f3593-115">アセンブリの作成時に、アセンブリ マニフェストが MyAssembly.dll に追加され、その Util.netmodule および Graphic.bmp との関係が示されます。</span><span class="sxs-lookup"><span data-stu-id="f3593-115">When the assembly was created, the assembly manifest was added to MyAssembly.dll, indicating its relationship with Util.netmodule and Graphic.bmp.</span></span>

<span data-ttu-id="f3593-116">ソース コードをデザインするときは、作成するアプリケーションの機能を 1 つのファイルにまとめるのか複数のファイルに分割するのか、分割する場合は機能をどのように分けるのかを明確に決める必要があります。</span><span class="sxs-lookup"><span data-stu-id="f3593-116">As you currently design your source code, you make explicit decisions about how to partition the functionality of your application into one or more files.</span></span> <span data-ttu-id="f3593-117">.NET Framework コードをデザインする場合も同様に、機能を 1 つのアセンブリにまとめるのか複数のアセンブリに分割するのか、分割する場合は機能をどのように分割するのかを決めておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="f3593-117">When designing .NET Framework code, you will make similar decisions about how to partition the functionality into one or more assemblies.</span></span>

## <a name="see-also"></a><span data-ttu-id="f3593-118">関連項目</span><span class="sxs-lookup"><span data-stu-id="f3593-118">See also</span></span>

- [<span data-ttu-id="f3593-119">.NET のアセンブリ</span><span class="sxs-lookup"><span data-stu-id="f3593-119">Assemblies in .NET</span></span>](index.md)
- [<span data-ttu-id="f3593-120">アセンブリ マニフェスト</span><span class="sxs-lookup"><span data-stu-id="f3593-120">Assembly manifest</span></span>](manifest.md)
- [<span data-ttu-id="f3593-121">アセンブリのセキュリティに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="f3593-121">Assembly security considerations</span></span>](security-considerations.md)