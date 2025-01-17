---
title: Azure Kubernetes Service でサポートされている Kubernetes のバージョン
description: Azure Kubernetes Service (AKS) の Kubernetes バージョン サポート ポリシーとクラスターのライフサイクルを理解する
services: container-service
ms.topic: article
ms.date: 08/09/2021
author: palma21
ms.author: jpalma
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d4a2d90c797585ad025a9540045fe65dee385000
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2021
ms.locfileid: "132493606"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) でサポートされている Kubernetes のバージョン

Kubernetes コミュニティでは、おおよそ 3 か月おきにマイナー バージョンをリリースしています。 最近、Kubernetes コミュニティでは、バージョン 1.19 以降の[各バージョンのサポート期間が 9 か月間から 12 か月間に延長](https://kubernetes.io/blog/2020/08/31/kubernetes-1-19-feature-one-year-support/)されました。

マイナー バージョンのリリースには、新しい機能と機能強化が含まれます。 修正プログラムのリリースは、より頻繁な (場合によっては毎週)、マイナー バージョンでの重要なバグ修正を目的としています。 パッチ リリースには、セキュリティの脆弱性または重大なバグの修正が含まれています。

## <a name="kubernetes-versions"></a>Kubernetes のバージョン

Kubernetes では、各バージョンに対して標準の[セマンティック バージョニング](https://semver.org/)のバージョン管理スキームを使用します。

```
[major].[minor].[patch]

Example:
  1.17.7
  1.17.8
```

バージョンのそれぞれの数字は、前のバージョンとの一般的な互換性を示します。

* **メジャー バージョン** は、互換性のない API の更新や下位互換性が破棄されている可能性があるときに変更されます。
* **マイナー バージョン** は、その他のマイナー リリースに対する下位互換性のある機能の更新が行われたときに変更されます。
* **修正プログラムのバージョン** は、下位互換性のあるバグ修正が行われたときに変更されます。

実行しているマイナー バージョンの最新のパッチ リリースを実行するよう努める必要があります。 たとえば、運用クラスターが **`1.17.7`** であるとします。 *1.17* シリーズに利用できる最新のパッチ バージョンは **`1.17.8`** です。 クラスターに完全にパッチが適用されてサポートされるように、できるだけ早く **`1.17.8`** にアップグレードする必要があります。

## <a name="kubernetes-version-support-policy"></a>Kubernetes バージョン サポート ポリシー

AKS では、すべての SLO または SLA 測定で有効であり、すべてのリージョンで利用可能なバージョンを一般提供バージョンとして定義しています。 AKS では、Kubernetes の 3 つの GA マイナー バージョンがサポートされています。

* AKS でリリースされた最新の GA マイナー バージョン (N と呼ばれます)。
* 2 つの以前のマイナー バージョン。
    * サポートされている各マイナー バージョンでは、最大 2 つの安定性の高い修正プログラムもサポートされています。

さらに、AKS では、明示的にラベル付けされ、[プレビューの利用規約][preview-terms]の対象となるプレビュー バージョンがサポートされる場合もあります。

> [!NOTE]
> AKS では、段階的なリージョン デプロイを含む安全なデプロイ プラクティスを使用します。 つまり、新しいリリースまたは新しいバージョンをすべてのリージョンで利用できるようになるまでに、最大で 10 営業日かかることがあるということです。

AKS でサポートされている Kubernetes バージョンの対象期間は、"N-2" と呼ばれます:(N (最新リリース) - 2 (マイナー バージョン))。

たとえば、AKS で *1.17.a* が本日導入された場合は、次のバージョンのサポートが提供されます。

新しいマイナー バージョン    |    サポートされているバージョンの一覧
-----------------    |    ----------------------
1.17.a               |    1.17.a、1.17.b、1.16.c、1.16.d、1.15.e、1.15.f

ここで、".<英字>" はパッチ バージョンを表します。

新しいマイナー バージョンが導入されると、サポートされている最も古いマイナー バージョンと修正プログラムのリリースは、非推奨となり削除されます。 たとえば、現在次のバージョンがサポートされているとします。

```
1.17.a
1.17.b
1.16.c
1.16.d
1.15.e
1.15.f
```

1\.18.\* がリリースされると、すべての 1.15.\* バージョンが削除され、30 日以内にサポート対象外になります。

> [!NOTE]
> お客様がサポートされていない Kubernetes バージョンを実行している場合は、クラスターのサポートを要求したときにアップグレードするよう求められます。 サポートされていない Kubernetes リリースを実行しているクラスターは、[AKS サポート ポリシー](./support-policies.md)の対象ではありません。

上記に加えて、AKS では、特定のマイナー バージョンのリリースの **修正プログラム** が最大 2 つサポートされています。 次のようなサポートされているバージョンがあるとします。

```
Current Supported Version List
------------------------------
1.17.8, 1.17.7, 1.16.10, 1.16.9
```

AKS で `1.17.9` と `1.16.11` がリリースされた場合、最も古い修正プログラムのバージョンは非推奨となって削除され、サポートされているバージョンの一覧は次のようになります。

```
New Supported Version List
----------------------
1.17.*9*, 1.17.*8*, 1.16.*11*, 1.16.*10*
```

### <a name="supported-kubectl-versions"></a>サポートされている `kubectl` バージョン

*kube-apiserver* バージョンに対して 1 つ新しいまたは古い `kubectl` のマイナー バージョンを使用します。これは、[kubectl の Kubernetes サポート ポリシー](https://kubernetes.io/docs/setup/release/version-skew-policy/#kubectl)に一致しています。

たとえば、*kube-apiserver* が *1.17* の場合は、その *kube-apiserver* と共に `kubectl` の *1.16* から *1.18* を使用できます。

`kubectl` をインストールまたは最新バージョンに更新するには、以下を実行します。

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az aks install-cli
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```powershell
Install-AzAksKubectl -Version latest
```
---

## <a name="release-and-deprecation-process"></a>リリースと非推奨のプロセス

今後のバージョンのリリースと非推奨は、「[AKS Kubernetes リリース予定表](#aks-kubernetes-release-calendar)」で参照できます。

新しい **マイナー** バージョンの Kubernetes の場合:
  * AKS では、新しいバージョンのリリース予定日と、その旧バージョンの非推奨予定日を含む事前通知を、削除日の 30 日前までに [AKS リリース ノート](https://aka.ms/aks/releasenotes)で公開します。
  * 推奨されなくなった API が原因で、新しいバージョンによってクラスターで問題が発生した場合、AKS は、[Azure Advisor](../advisor/advisor-overview.md) を使用してユーザーに警告します。 Azure Advisor は、現在サポートされていない場合にも、ユーザーに警告するために使用されます。
  * AKS によって、AKS とポータルのアクセス権を持つすべてのユーザーが使用できる[サービスの正常性通知](../service-health/service-health-overview.md)が発行され、サブスクリプション管理者宛にバージョンの削除予定日が記載されたメールが送信されます。

    > [!NOTE]
    > サブスクリプション管理者を見つけるか、サブスクリプションを変更するには、[Azure サブスクリプションの管理](../cost-management-billing/manage/add-change-subscription-administrator.md#assign-a-subscription-administrator)に関するページを参照してください。

  * 今後もサポートを受けるには、ユーザーは、バージョンの削除から **30 日** 以内にサポートされるマイナー バージョンのリリースにアップグレードする必要があります。

新しい **パッチ** バージョンの Kubernetes の場合:
  * パッチ バージョンは緊急を要するため、これらは利用可能になり次第サービスに導入されることがあります。
  * 通常、AKS では新しいパッチ バージョンについて広く通知していません。 しかし、AKS では、利用可能な CVE を AKS でタイムリーにサポートできるよう、この修正プログラムを常に監視および検証しています。 重要な修正プログラムが見つかった場合、またはユーザー アクションが必要な場合、AKS では、使用可能な新しい修正プログラムにアップグレードするようユーザーに通知します。
  * サポートを継続して受けるには、AKS からパッチ リリースが削除されてから **30 日** 以内に、サポートされているパッチにアップグレードする必要があります。

### <a name="supported-versions-policy-exceptions"></a>サポートされているバージョン ポリシーの例外

AKS は、運用環境に影響を与える 1 つ以上の重大なバグまたはセキュリティ上の問題がある新しいバージョンまたは既存のバージョンを、予告なしに追加または削除する権利を留保します。

特定のパッチ リリースは、バグまたはセキュリティの問題の重大度に応じて、スキップされるか、ロールアウトが促進される場合があります。

## <a name="azure-portal-and-cli-versions"></a>Azure portal と CLI のバージョン

AKS クラスターをポータル、Azure CLI、または Azure PowerShell でデプロイすると、クラスターは既定で N-1 マイナー バージョンおよび最新パッチに設定されます。 たとえば、AKS でサポートされているのが *1.17.a*、*1.17.b*、*1.16.c*、*1.16.d*、*1.15.e*、および *1.15.f* であれば、選択される既定のバージョンは *1.16.c* となります。

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

ご使用のサブスクリプションとリージョンで現在使用可能なバージョンを確認するには、[az aks get-versions][az-aks-get-versions] コマンドを使用します。 次の例では、*EastUS* リージョンで使用可能な Kubernetes のバージョンが一覧表示されます。

```azurecli-interactive
az aks get-versions --location eastus --output table
```


### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

ご使用のサブスクリプションとリージョンで現在使用可能なバージョンを確認するには、[Get-AzAksVersion][get-azaksversion] コマンドレットを使用します。 次の例では、*EastUS* リージョンで使用可能な Kubernetes のバージョンが一覧表示されます。

```azurepowershell-interactive
Get-AzAksVersion -Location eastus
```

---

## <a name="aks-kubernetes-release-calendar"></a>AKS Kubernetes リリース予定表

過去のリリース履歴については、[Kubernetes](https://en.wikipedia.org/wiki/Kubernetes#History) に関するページを参照してください。

|  K8s バージョン | アップストリームのリリース  | AKS プレビュー  | AKS GA  | サポート終了 |
|--------------|-------------------|--------------|---------|-------------|
| 1.19*  | 2020 年 8 月 4 日  | 2020 年 9 月   | 2020 年 11 月  | 1.22 GA |
| 1.20  | 2020 年 12 月 8 日  | 2021 年 1 月   | 2021 年 3 月  | 1.23 GA |
| 1.21  | 2021 年 4 月 8 日 | 2021 年 5 月   | 2021 年 7 月  | 1.24 GA |
| 1.22  | 2021 年 8 月 4 日 | 2021 年 9 月   | 2021 年 11 月  | 1.25 GA |
| 1.23  | 2021 年 12 月 | 2022 年 1 月   | 2022 年 2 月  | 1.26 GA |

> [!NOTE]
> 1.19 は非推奨となり、2022 年 1 月末に AKS から削除されます。

## <a name="faq"></a>よく寄せられる質問

**Kubernetes の新バージョンについては、Microsoft からどのように通知されますか?**

AKS チームは、Kubernetes の新バージョンの予定日の事前発表を、ドキュメントや [GitHub](https://github.com/Azure/AKS/releases) で公開します。また、サポート対象外となるクラスターを所有するサブスクリプション管理者に電子メールで通知します。  発表に加えて、AKS では、[Azure Advisor](../advisor/advisor-overview.md) を使用して、ユーザーがサポート対象外であるかどうかの警告と、ユーザーのアプリケーションや開発プロセスに影響する非推奨化された API に関する警告を、 Azure portal 内で顧客に通知します。

**サポートを利用し続けるには、どのくらいの頻度で Kubernetes のバージョンをアップグレードする必要がありますか?**

Kubernetes 1.19 以降では、[オープン ソース コミュニティへのサポートが 1 年間延長](https://kubernetes.io/blog/2020/08/31/kubernetes-1-19-feature-one-year-support/)されています。 AKS により、アップストリーム コミットメントに一致するパッチとサポートを有効にすることがコミットされています。 1\.19 以降の AKS クラスターでは、サポートされているバージョンを利用し続けることができるように、1 年に 1 回以上アップグレードできます。

**ユーザーがサポートされていないマイナー バージョンの Kubernetes クラスターをアップグレードするとどうなりますか。**

*n-3* バージョン以前を使用している場合は、サポート外であり、アップグレードするよう求められます。 バージョン n-3 から n-2 へのアップグレードに成功すると、サポート ポリシーの対象になります。 次に例を示します。

- サポートされている最も古い AKS のバージョンが *1.15.a* で、使用しているバージョンが *1.14.b* 以前の場合は、サポート対象外です。
- *1.14.b* から *1.15.a* 以降へ正常にアップグレードすると、サポート ポリシー対象に戻ります。

ダウングレードはサポートされていません。

**"サポート外" とは**

"サポート外" とは次のことを意味します。
* 実行しているバージョンがサポートされているバージョンの一覧に含まれていない。
* サポートを要求すると、サポートされているバージョンにクラスターをアップグレードするよう求められる (ただし、バージョンの非推奨後、30 日間の猶予期間中である場合を除く)。

さらに、AKS では、サポートされているバージョンの一覧に含まれていないクラスターのランタイムなどは保証されません。

**ユーザーがサポートされていないマイナー バージョンの Kubernetes クラスターをスケールするとどうなりますか。**

AKS でサポートされていないマイナー バージョンについては、スケールインまたはアウトは引き続き機能します。 サービスの品質が保証されないため、アップグレードしてクラスターをサポート対象に戻すことをお勧めします。

**ユーザーが Kubernetes バージョンをそのまま永久に使用し続けることはできますか。**

3 つ以上のマイナー バージョンにわたってクラスターがサポート外であり、セキュリティ リスクがあることがわかっている場合、Azure はクラスターを事前にアップグレードするようにお客様に連絡します。 お客様が措置を講じない場合、Azure は、お客様に代わってクラスターを自動的にアップグレードする権利を保持します。

**ノード プールがサポートされている AKS バージョンのいずれかにない場合、コントロール プレーンはどのバージョンをサポートしますか。**

コントロール プレーンは、すべてのノード プールのバージョンの期間内になければなりません。 コントロール プレーンまたはノード プールのアップグレードの詳細については、[ノード プールのアップグレード](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools)に関するドキュメントを参照してください。

**クラスターのアップグレード中に複数の AKS バージョンをスキップできますか。**

サポートされている AKS クラスターをアップグレードする場合は、Kubernetes マイナー バージョンをスキップすることはできません。 Kubernetes コントロール プレーンの[バージョン スキュー ポリシー](https://kubernetes.io/releases/version-skew-policy/) (バージョン差異に関する方針) では、マイナー バージョンをスキップできません。 以下の場合のアップグレードを例に挙げます。

  * *1.12.x* -> *1.13.x*: 許可されます。
  * *1.13.x* -> *1.14.x*: 許可されます。
  * *1.12.x* -> *1.14.x*: 許可されません。

*1.12.x* から *1.14.x* へアップグレードする場合:
1. *1.12.x* から *1.13.x* へアップグレードします。
1. *1.13.x* から *1.14.x* へアップグレードします。

複数のバージョンのスキップは、サポートされていないバージョンからサポートされている最小バージョンにアップグレードする場合にのみ可能です。 たとえば、*1.15* がサポートされている最小マイナー バージョンの場合、サポートされていない *1.10.x* からサポートされている *1.15.x* へアップグレードできます。

**30 日間のサポート期間中に新しい 1.xx.x クラスターを作成できますか?**

いいえ。 バージョンが非推奨になるか、または削除されると、そのバージョンを使用してクラスターを作成することはできません。 変更が公開されると、バージョン リストから古いバージョンが削除されたことがわかります。 このプロセスは、リージョンごとに、発表から最大 2 週間かかる場合があります。

**非推奨になったばかりのバージョンを使用していますが、新しいノード プールを追加できますか? または、アップグレードする必要がありますか?**

いいえ。 非推奨のバージョンのノード プールをクラスターに追加することはできません。 新しいバージョンのノード プールを追加することはできます。 ただし、そのためには、まず、コントロール プレーンを更新する必要があります。

## <a name="next-steps"></a>次のステップ

クラスターをアップグレードする方法の詳細については、「[Azure Kubernetes Service (AKS) クラスターのアップグレード][aks-upgrade]」をご覧ください。

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az_aks_get_versions
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[get-azaksversion]: /powershell/module/az.aks/get-azaksversion
