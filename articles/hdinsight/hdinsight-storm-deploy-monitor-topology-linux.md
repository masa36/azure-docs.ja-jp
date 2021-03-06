---
title: "Linux ベースの HDInsight での Apache Storm トポロジのデプロイと管理 | Microsoft Docs"
description: "Linux ベースの HDInsight で Storm ダッシュボードを使用して Apache Storm トポロジをデプロイ、監視、管理する方法について説明します。 Hadoop Tools for Visual Studio を使用します。"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/07/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d5c33fc7d24eb2b14db9cdf3211a06e6fcc1cb68


---
# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Linux ベースの HDInsight での Apache Storm トポロジのデプロイと管理
このドキュメントでは、HDInsight クラスターで Linux ベースの Storm を実行している Storm トポロジを管理および監視する方法の基本について説明します。

> [!IMPORTANT]
> この記事の手順では、HDInsight クラスター上の Linux ベースの Storm が必要です。 Windows ベースの HDInsight でトポロジをデプロイおよび監視する方法については、「 [HDInsight での Apache Storm トポロジのデプロイと管理](hdinsight-storm-deploy-monitor-topology.md)
> 
> 

## <a name="prerequisites"></a>前提条件
* **HDInsight クラスター上の Linux ベースの Storm**: クラスターの作成手順については、「 [HDInsight での Apache Storm の使用](hdinsight-apache-storm-tutorial-get-started-linux.md) 」を参照してください。
* **SSH と SCP を熟知していること**: HDInsight で SSH と SCP を使用する方法については、以下を参照してください。
  
  * **Linux、Unix、または OS X クライアント**: See [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する (プレビュー)](hdinsight-hadoop-linux-use-ssh-unix.md)
  * **Windows クライアント**: [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する (プレビュー)](hdinsight-hadoop-linux-use-ssh-windows.md)
* **SCP クライアント**: Linux、Unix、OS X システムのすべてに付属しています。 Windows クライアントの場合は PSCP をお勧めします。PSCP は [PuTTY ダウンロード ページ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)から入手できます。

## <a name="start-a-storm-topology"></a>Storm トポロジを開始する
### <a name="using-ssh-and-the-storm-command"></a>SSH と Storm コマンドを使用する
1. SSH を使用して、HDInsight クラスターに接続します。 **USERNAME** には、SSH ログイン名を指定します。 **CLUSTERNAME** には、HDInsight クラスター名を指定します。
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    SSH を使用して HDInsight クラスターに接続する方法の詳細については、次のドキュメントを参照してください。
   
   * **Linux、Unix、または OS X クライアント**: See [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する (プレビュー)](hdinsight-hadoop-linux-use-ssh-unix.md)
   * **Windows クライアント**: See [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する (プレビュー)](hdinsight-hadoop-linux-use-ssh-windows.md)
2. 次のコマンドを実行してトポロジの例を開始します。
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount
   
    クラスターで WordCount トポロジの例が開始されます。 文はランダムに生成され、文中の各単語の出現回数がカウントされます。
   
   > [!NOTE]
   > トポロジをクラスターに送信する場合、まずクラスターを含む jar ファイルをコピーしてから、`storm` コマンドを実行します。 この場合、ファイルが保存されているクライアントから `scp` コマンドを実行します。 たとえば、 `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
   > 
   > WordCount の例と他の Storm スターターの例は、 `/usr/hdp/current/storm-client/contrib/storm-starter/`のクラスターに既に含まれています。
   > 
   > 

### <a name="programmatically"></a>プログラムを使用する
クラスターでホストされている Nimbus サービスと通信することにより、プログラムを使用して HDInsight の Storm にトポロジをデプロイできます。 [https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) に用意されている Java アプリケーションの例を参照してください。

## <a name="monitor-and-manage-using-the-storm-command"></a>Storm コマンドを使用して監視および管理する
`storm` ユーティリティを使用すると、コマンド ラインから実行中のトポロジを操作できます。 次に、一般的に使用されるコマンドの一覧を示します。 完全なコマンド一覧を表示するには、 `storm -h` を使用してください。

### <a name="list-topologies"></a>トポロジの一覧を表示する
実行中のすべてのトポロジを一覧表示するには、次のコマンドを使用します。

    storm list

次のような情報が返されます。

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>アクティブ化の解除と再アクティブ化
トポロジが強制終了または再アクティブ化されるまで、トポロジのアクティブ化の解除は一時停止されます。 次のコマンドを使用して、アクティブ化の解除と再アクティブ化を行います。

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>実行中のトポロジを強制終了する
Storm トポロジが開始されると、停止されるまで実行が継続されます。 トポロジを停止するには、次のコマンドを使用します。

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>再調整
トポロジを再調整すると、トポロジの並列処理が変更されます。 たとえば、クラスターのサイズを変更してノートを追加すると、再調整によって実行中のトポロジが新しいノードを利用できるようになります。

> [!WARNING]
> トポロジを再調整すると、まずトポロジのアクティブ化が解除され、worker がクラスター全体に平均的に再分配され、最終的に、トポロジは再調整の発生前の状態に戻ります。 そのため、トポロジがアクティブだった場合は、アクティブに戻ります。 非アクティブだった場合は、非アクティブのままになります。
> 
> 

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Storm UI を使用して監視および管理する
Storm UI には、トポロジの実行を操作する Web インターフェイスがあり、HDInsight クラスターに含まれています。 Storm UI を表示するには、Web ブラウザーを使用して **https://CLUSTERNAME.azurehdinsight.net/stormui** を開きます。**CLUSTERNAME** は実際のクラスターの名前です。

> [!NOTE]
> ユーザー名とパスワードの入力が求められたら、クラスターの作成時に使用したクラスター管理者名 (admin) とパスワードを入力します。
> 
> 

### <a name="main-page"></a>メイン ページ
Storm UI のメイン ページには、次の情報が表示されます。

* **クラスターの概要**: Storm クラスターの基本情報
* **トポロジの概要**: 実行中のトポロジの一覧 このセクションのリンクを使用して、各トポロジの詳細情報を表示します。
* **スーパーバイザの概要**: Storm のスーパーバイザに関する情報
* **Nimbus 構成**: クラスターの Nimbus 構成

### <a name="topology-summary"></a>トポロジの概要
**[トポロジの概要]** セクションのリンクを選択すると、トポロジに関する次の情報が表示されます。

* **トポロジの概要**: トポロジの基本情報
* **トポロジのアクション**: トポロジで実行できる管理アクション
  
  * **アクティブ化**: アクティブ化が解除されたトポロジの処理を再開します
  * **アクティブ化の解除**: 実行中のトポロジを一時停止します
  * **再調整**: トポロジの並列処理を調整します。 クラスターのノード数を変更した場合は、実行中のトポロジを再調整する必要があります。 この操作で、クラスター内のノード数の増減に合わせて、トポロジの並列処理を調整できます。
    
    詳細については、「<a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology (Storm トポロジの並列処理)</a>」をご覧ください。
  * **強制終了**: 指定したタイムアウト後に Storm トポロジを停止します。
* **トポロジの統計**: トポロジに関する統計。 **[Window]** 列にあるリンクを使用して、ページで残りのエントリの時間枠を設定します。
* **スパウト**: トポロジで使用するスパウト。 このセクションにあるリンクを使用して、各スパウトの詳細情報を表示します。
* **ボルト**: トポロジで使用するボルト。 このセクションにあるリンクを使用して、各ボルトの詳細情報を表示します。
* **トポロジの構成**: 選択したトポロジの構成

### <a name="spout-and-bolt-summary"></a>スパウトとボルトの概要
**[スパウト]** または **[ボルト]** セクションでアイテムを選択すると、そのアイテムに関する次の情報が表示されます。

* **コンポーネントの概要**: スパウトやボルトの基本情報
* **スパウト/ボルトの統計**: スパウトやボルトの統計。 **[Window]** 列にあるリンクを使用して、ページで残りのエントリの時間枠を設定します。
* **入力の状態** (ボルトのみ): ボルトが消費する入力ストリームに関する情報
* **出力の状態**: このスパウトやボルトから出力されるストリームに関する情報
* **エグゼキュータ**: スパウトやボルトのインスタンスに関する情報 特定のエグゼキュータの **[ポート]** エントリを選択して、このインスタンスで生成された診断情報のログを閲覧します
* **エラー**: このスパウトやボルトのエラー情報。

## <a name="rest-api"></a>REST API
Storm UI は、REST API を基に構築されているため、REST API を使用して同様の管理や監視機能を実行できます。 REST API を使用して、Storm トポロジの管理や監視用のカスタム ツールを作成できます。

詳細については、「 [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html)」を参照してください。 以下は、HDInsight での Apache Storm で REST API を使用する場合の情報です。

> [!IMPORTANT]
> Storm REST API は、インターネットでパブリックに使用できません。SSH トンネルを使用して HDInsight クラスター ヘッド ノードにアクセスする必要があります。 SSH トンネルの作成と使用については、[SSH トンネリングを使用して Ambari Web UI、ResourceManager、JobHistory、NameNode、Oozie、およびその他の Web UI にアクセスする方法](hdinsight-linux-ambari-ssh-tunnel.md)に関するページを参照してください。
> 
> 

### <a name="base-uri"></a>ベース URI
Linux ベースの HDInsight クラスターの REST API のベース URI はヘッド ノード (**https://HEADNODEFQDN:8744/api/v1/**) で確認できますが、ヘッド ノードのドメイン名はクラスター作成時に生成され、静的ではありません。

クラスター ヘッド ノードの完全修飾ドメイン名 (FQDN) はいくつかの方法で確認できます。

* **SSH セッションから**: SSH セッションからクラスターに `headnode -f` コマンドを使用します。
* **Ambari Web から**: ページの一番上から **[サービス]** を選択し、**[Storm]** を選択します。 **[概要 ]** タブで **[Storm UI Server]** を選択します。 Storm UI と REST API が実行されているノードの FQDN はページの一番上で確認できます。
* **Ambari REST API から**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` コマンドを使用し、Storm UI と REST API が実行されているノードに関する情報を取得します。 **PASSWORD** をクラスターの管理者パスワードに替えます。 **CLUSTERNAME** をクラスター名に置き換えます。 応答の「host_name」エントリにノードの FQDN が含まれます。

### <a name="authentication"></a>認証
REST API への要求では、HDInsight クラスターの管理者名とパスワードを使用して、 **基本認証**を使用する必要があります。

> [!NOTE]
> 基本認証はクリア テキストを使用して送信されるため、 **常に** HTTPS を使用してクラスターとの通信を保護する必要があります。
> 
> 

### <a name="return-values"></a>戻り値
REST API から返される情報は、クラスターと同じ Azure Virtual Network 上にあるクラスターや仮想マシン上でのみ使用できます。 たとえば、Zookeeper サービスに返された完全修飾ドメイン名 (FQDN) は、インターネットからはアクセスできません。

## <a name="next-steps"></a>次のステップ
ここでは、Storm ダッシュボードを使用してトポロジをデプロイおよび監視する方法について説明しました。次に、[Maven を使用して Java ベースのトポロジを開発する](hdinsight-storm-develop-java-topology.md)方法について説明します。

その他の Storm トポロジ例は、「 [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)」をご覧ください。




<!--HONumber=Nov16_HO3-->


