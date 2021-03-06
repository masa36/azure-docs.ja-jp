---
title: "Web UI を使用した Azure Container Service のコンテナー管理| Microsoft Docs"
description: "Marathon Web UI を使用して Azure コンテナー サービスのクラスター サービスにコンテナーをデプロイします。"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker、コンテナー、マイクロ サービス、Mesos、Azure"
ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2016
ms.author: timlt
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ae2945b0025da3bc933a1e4a8f10f21fd35cb51c


---
# <a name="container-management-through-the-web-ui"></a>Web UI 経由のコンテナー管理
DC/OS はクラスター化されたワークロードをデプロイし、スケールするための環境を提供すると共に、基礎となるハードウェアを抽象化します。 DC/OS に加え、コンピューティング ワークロードのスケジュールと実行を管理するフレームワークもあります。

一般的な各種ワークロードに対応したフレームワークがあるものの、このドキュメントでは、Marathon を使ってコンテナー デプロイを作成し、スケールする方法について説明します。 これらの例を見ていく前に、Azure コンテナー サービスで構成された DC/OS クラスターが必要です。 また、このクラスターへのリモート接続も必要です。 これらの項目の詳細については、次の記事を参照してください。

* [Azure コンテナー サービス クラスターのデプロイ](container-service-deployment.md)
* [Azure コンテナー サービス クラスターに接続する](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>DC/OS UI を操作する
Secure Shell (SSH) トンネルを確立したら、http://localhost/ に移動します。 DC/OS Web UI が読み込まれ、使用リソース、アクティブなエージェント、実行中のサービスなど、クラスターに関する情報が表示されます。

![DC/OS UI](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Marathon UI について知る
Marathon UI を表示するには、http://localhost/Marathon に移動します。 この画面から、Azure コンテナー サービス DC/OS クラスターで新しいコンテナーやその他のアプリケーションを開始できます。 コンテナーとアプリケーションの実行に関する情報も確認できます。  

![Marathon UI](media/dcos/dcos3.png)

## <a name="deploy-a-dockerformatted-container"></a>Docker 形式のコンテナーのデプロイ
Marathon を使用して新しいコンテナーをデプロイするには、 **[Create Application (アプリケーションの作成)]** ボタンをクリックし、次の情報をフォームに入力します。

| フィールド | 値 |
| --- | --- |
| ID |nginx |
| イメージ |nginx |
| ネットワーク |ブリッジ |
| ホスト ポート |80 |
| プロトコル |TCP |

![New Application UI--General](media/dcos/dcos4.png)

![New Application UI--Docker Container](media/dcos/dcos5.png)

![New Application UI--Ports and Service Discovery](media/dcos/dcos6.png)

コンテナー ポートをエージェント上のポートに対して静的にマップするには、JSON モードを使用する必要があります。 トグル スイッチを使用して、New Application (新しいアプリケーション) ウィザードを [ **JSON Mode (JSON モード)** ] に切り替えてください。 その後、アプリケーション定義の `portMappings` セクションに次のように入力します。 この例では、コンテナーのポート 80 を DC/OS エージェントのポート 80 にバインドしています。 JSON モードで変更が済んだらウィザードを元のモードに切り替えてかまいません。

```none
"hostPort": 80,
```

![New Application UI--port 80 example](media/dcos/dcos13.png)

DC/OS クラスターは、プライベート エージェントおよびパブリック エージェントのセットと共にデプロイされます。 クラスターがインターネットからアプリケーションにアクセスできるようにするには、アプリケーションをパブリック エージェントにデプロイする必要があります。 そのためには、新しいアプリケーション ウィザードの **[オプション]** タブを選択し、**[Accepted Resource Roles (承認されたリソース ロール)]** に「**slave_public**」と入力します。

![New Application UI--public agent setting](media/dcos/dcos14.png)

Marathon メイン ページに戻ると、コンテナーのデプロイの状態を確認できます。

![Marathon main page UI--container deployment status](media/dcos/dcos7.png)

DC/OS の Web UI (http://localhost/) に戻ると、タスク (この場合は Docker 形式のコンテナー) が DC/OS クラスターで実行されていることがわかります。

![DC/OS web UI--task running on the cluster](media/dcos/dcos8.png)

タスクが実行されているクラスター ノードを確認することもできます。

![DC/OS web UI--task cluster node](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>コンテナーのスケール
Marathon UI では、コンテナーのインスタンス数をスケールすることができます。 この操作を実行するには、**Marathon** ページに移動し、スケールするコンテナーを選択して、**[スケール]** ボタンをクリックします。 **[Scale Application (アプリケーションのスケール)]** ダイアログ ボックスで、任意のコンテナー インスタンス数を入力し、**[Scale Application (アプリケーションのスケール)]** を選択します。

![Marathon UI--Scale Application dialog box](media/dcos/dcos10.png)

スケール操作が完了したら、同じタスクの複数のインスタンスが DC/OS エージェントに分散されているのを確認できます。

![DC/OS web UI dashboard--task spread across agents](media/dcos/dcos11.png)

![DC/OS web UI--nodes](media/dcos/dcos12.png)

## <a name="next-steps"></a>次のステップ
* [DC/OS と Marathon API の使用](container-service-mesos-marathon-rest.md)

Mesos と共に Azure Container Service を使用する方法の詳細

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
> 
> 




<!--HONumber=Nov16_HO2-->


