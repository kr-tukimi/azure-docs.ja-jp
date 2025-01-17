---
title: MLflow モデルを Web サービスとしてデプロイする
titleSuffix: Azure Machine Learning
description: Azure Machine Learning で MLflow を設定して、ML モデルを Azure Web サービスとしてデプロイします。
services: machine-learning
author: cjgronlund
ms.author: cgronlun
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.date: 10/25/2021
ms.topic: how-to
ms.custom: devx-track-python
ms.openlocfilehash: e77c2729d9cab3fb7c50a808c2220412e287dec5
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2021
ms.locfileid: "132719886"
---
# <a name="deploy-mlflow-models-as-azure-web-services"></a>MLflow モデルを Azure Web サービスとしてデプロイする

この記事では、[MLflow](https://www.mlflow.org) モデルを Azure Web サービスとしてデプロイする方法について説明します。これにより、Azure Machine Learning のモデル管理とデータ ドリフト検出の機能を活用し、実稼働モデルに適用できるようになります。 MLflow と Azure Machine Learning のその他の機能統合については、[MLflow と Azure Machine Learning](concept-mlflow.md) に関する記事をご覧ください。

Azure Machine Learning では、次のデプロイ構成が提供されます。
* Azure コンテナー インスタンス (ACI)。これは、迅速な開発/テスト デプロイに適した選択肢です。
* Azure Kubernetes Service (AKS)。これは、スケーラブルな運用環境デプロイに推奨されます。

[!INCLUDE [endpoints-option](../../includes/machine-learning-endpoints-preview-note.md)]

> [!TIP]
> このドキュメントの情報は、MLflow モデルを Azure Machine Learning Web サービス エンドポイントにデプロイするデータ サイエンティストと開発者を主に対象としています。 Azure Machine Learning からリソース使用状況やイベント (クォータ、トレーニング実行の完了、モデル デプロイの完了など) を監視することに関心がある管理者の方は、「[Azure Machine Learning の監視](monitor-azure-machine-learning.md)」を参照してください。

## <a name="mlflow-with-azure-machine-learning-deployment"></a>Azure Machine Learning デプロイでの MLflow

MLflow は、機械学習の実験のライフ サイクルを管理するためのオープンソース ライブラリです。 Azure Machine Learning との統合により、モデルのトレーニングを超えて、実稼働モデルのデプロイ フェーズまでこの管理を拡張できます。

次の図は、MLflow デプロイ API と Azure Machine Learning を使用して、一般的なフレームワーク (PyTorch、Tensorflow、scikit-learn など) で作成されたモデルを Azure Web サービスとしてデプロイし、ワークスペース内で管理できることを示しています。 

![ Azure Machine Learning を使用して MLflow モデルをデプロイする](./media/how-to-deploy-mlflow-models/mlflow-diagram-deploy.png)

## <a name="prerequisites"></a>前提条件

* 機械学習モデル。 トレーニング済みのモデルがない場合は、[こちらのリポジトリ](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/using-mlflow)で実際のコンピューティング シナリオに最適なノートブックの例を見つけて、その手順に従います。 
* [Azure Machine Learning に接続するための MLflow 追跡 URI を設定します](how-to-use-mlflow.md#track-local-runs)。
* `azureml-mlflow` パッケージをインストールします。 
    * このパッケージからは自動的に、MLflow がワークスペースにアクセスするための接続を提供する、[Azure Machine Learning Python SDK](/python/api/overview/azure/ml/install) の `azureml-core` が持ち込まれます。
* [ワークスペースで MLflow 操作を実行するために必要なアクセス許可](how-to-assign-roles.md#mlflow-operations)を確認します。 

## <a name="deploy-to-azure-container-instance-aci"></a>Azure コンテナー インスタンス (ACI) にデプロイする

MLflow モデルを Azure Machine Learning Web サービスにデプロイするには、[Azure Machine Learning に接続するための MLflow 追跡 URI](how-to-use-mlflow.md) を使用してモデルを設定する必要があります。 

ACI にデプロイする場合は、デプロイ構成を定義する必要はありません。構成が指定されていない場合、サービスによって既定で ACI にデプロイされます。
次に、Azure Machine Learning 用の MLflow の [deploy](https://www.mlflow.org/docs/latest/python_api/mlflow.azureml.html#mlflow.azureml.deploy) メソッドを使用して、1 つの手順でモデルを登録し、デプロイします。 


```python
from mlflow.deployments import get_deploy_client

# set the tracking uri as the deployment client
client = get_deploy_client(mlflow.get_tracking_uri())

# set the model path 
model_path = "model"

# define the model path and the name is the service name
# the model gets registered automatically and a name is autogenerated using the "name" parameter below 
client.create_deployment(model_uri='runs:/{}/{}'.format(run.id, model_path),
                         name="mlflow-test-aci")
```

### <a name="customize-deployment-configuration"></a>デプロイの構成をカスタマイズする

既定値を使用したくない場合は、[deploy_configuration()](/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) メソッドのパラメーターを参照として使用するデプロイ構成 JSON ファイルを使用して、デプロイの構成を設定できます。 

デプロイ構成 JSON ファイルでは、各デプロイ構成パラメーターをディクショナリの形式で定義する必要があります。 以下に例を示します。 [デプロイ構成 JSON ファイルに含めることができるものの詳細については、こちらを参照してください](reference-azure-machine-learning-cli.md#azure-container-instance-deployment-configuration-schema)。

```json
{"computeType": "aci",
 "containerResourceRequirements": {"cpu": 1, "memoryInGB": 1},
 "location": "eastus2"
}
```

その後、JSON ファイルを使用してデプロイを作成できます。

```python
# set the deployment config
deploy_path = "deployment_config.json"
test_config = {'deploy-config-file': deploy_path}

client.create_deployment(model_uri='runs:/{}/{}'.format(run.id, model_path),
                         config=test_config,
                         name="mlflow-test-aci")                                       
```


## <a name="deploy-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) へのデプロイ

MLflow モデルを Azure Machine Learning Web サービスにデプロイするには、[Azure Machine Learning に接続するための MLflow 追跡 URI](how-to-use-mlflow.md) を使用してモデルを設定する必要があります。 

AKS にデプロイするには、まず AKS クラスターを作成します。 [ComputeTarget.create()](/python/api/azureml-core/azureml.core.computetarget#create-workspace--name--provisioning-configuration-) メソッドを使用して、AKS クラスターを作成します。 新しいクラスターの作成には 20 分から 25 分かかる場合があります。

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aks-mlflow'

# Create the cluster
aks_target = ComputeTarget.create(workspace=ws, 
                                  name=aks_name, 
                                  provisioning_configuration=prov_config)

aks_target.wait_for_completion(show_output = True)

print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```
[deploy_configuration()](/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration#parameters) メソッド値を参照として使用して、デプロイ構成 json を作成します。 各デプロイ構成パラメーターは、単にディクショナリとして定義する必要があります。 例を次に示します。

```json
{"computeType": "aks", "computeTargetName": "aks-mlflow"}
```

次に、MLflow の[デプロイ クライアント](https://www.mlflow.org/docs/latest/python_api/mlflow.deployments.html)を使用して、1 つの手順でモデルを登録し、デプロイします。 

```python
from mlflow.deployments import get_deploy_client

# set the tracking uri as the deployment client
client = get_deploy_client(mlflow.get_tracking_uri())

# set the model path 
model_path = "model"

# set the deployment config
deploy_path = "deployment_config.json"
test_config = {'deploy-config-file': deploy_path}

# define the model path and the name is the service name
# the model gets registered automatically and a name is autogenerated using the "name" parameter below 
client.create_deployment(model_uri='runs:/{}/{}'.format(run.id, model_path),
                         config=test_config,
                         name="mlflow-test-aci")
```

サービスのデプロイには数分かかることがあります。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

デプロイした Web サービスを使用する予定がない場合は、`service.delete()` を使用してノートブックから削除します。  詳細については、[WebService.delete()](/python/api/azureml-core/azureml.core.webservice%28class%29#delete--) に関するドキュメントをご覧ください。

## <a name="example-notebooks"></a>サンプルの Notebook

[Azure Machine Learning ノートブックでの MLflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/using-mlflow) は、この記事で提示した概念を示し、さらに詳しく説明します。

> [!NOTE]
> mlflow を使用したコミュニティ主導の例のリポジトリについては、 https://github.com/Azure/azureml-examples を参照してください。

## <a name="next-steps"></a>次のステップ

* [モデルを管理します](concept-model-management-and-deployment.md)。
* [データの誤差](./how-to-enable-data-collection.md)について実稼働モデルを監視します。
* [MLflow を使用して Azure Databricks 実行を追跡する](how-to-use-mlflow-azure-databricks.md)。
