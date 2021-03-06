---
title: "Microsoft Power BI Embedded とは何ですか?"
description: "Power BI Embedded を利用すると、Web アプリケーションやモバイル アプリケーションに Power BI レポートを統合できるため、ユーザー向けにデータを視覚化するカスタム ソリューションを作成する必要はありません"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 10/04/2016
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: 830eb6627cae71f358b9790791b1d86f7c82c566
ms.openlocfilehash: 75994d06c147fe0554dc5549df5816e6faaf8fb6


---
# <a name="what-is-microsoft-power-bi-embedded"></a>Microsoft Power BI Embedded とは何ですか?
**Power BI Embedded**を使用すると、Web アプリケーションやモバイル アプリケーションに Power BI レポートを統合することができます。

![](media\\powerbi-embedded-whats-is\\what-is.png)

**Power BI Embedded** は、ISV やアプリケーション開発者がアプリケーション内で Power BI データ エクスペリエンスを実現することを可能にする Azure サービスです。 開発者が作成したアプリケーションは、それぞれにユーザーが存在し、異なる機能セットを備えています。 これらのアプリケーションには、Microsoft Power BI Embedded に対応できるようになったグラフやレポートなどのデータ要素が組み込まれていることもあります。 ユーザーは、アプリケーションを使用するために Power BI アカウントを必要としません。 これまでと同様にアプリケーションにサインインし、Power BI のレポートを表示して操作できます。追加のライセンスは不要です。

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Microsoft Power BI Embedded のライセンス
**Microsoft Power BI Embedded** の使用モデルでは、Power BI のライセンスはエンドユーザーの責任ではありません。  代わりに、ビジュアルを使用するアプリケーションの開発者が**セッション**を購入します。レンダーの料金は、それらのリソースを所有するサブスクリプションに課金されます。 詳細については、価格に関するページ (https://azure.microsoft.com/ja-jp/pricing/details/power-bi-embedded/) を参照してください。

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI Embedded の概念モデル
![](media\\powerbi-embedded-whats-is\\model.png)

Azure の他のサービスと同様に、 [Azure ARM API](https://msdn.microsoft.com/library/mt712306.aspx)を使用して Power BI Embedded のリソースがプロビジョニングされます。 この場合、プロビジョニング対象のリソースは **Power BI ワークスペース コレクション**です。

## <a name="workspace-collection"></a>ワークスペース コレクション
**ワークスペース コレクション**は、0 個以上の**ワークスペース**が含まれた、リソースの最上位 Azure コンテナーです。   **ワークスペース** **コレクション** has all of the standard zure properties, as well as the following:

* **アクセス キー** – Power BI API を安全に呼び出すときに使用されるキーです (後述)。
* **ユーザー** – Azure ポータルまたは ARM API を使用して Power BI ワークスペース コレクションを管理する管理者権限を持つ Azure Active Directory (AAD) ユーザーです。
* **リージョン** – **ワークスペース コレクション**のプロビジョニングの一環として、プロビジョニング先のリージョンを選択できます。 詳細については、「 [Azure のリージョン](https://azure.microsoft.com/regions/)」をご覧ください。

## <a name="workspace"></a>ワークスペース
**ワークスペース** は、Power BI コンテンツのコンテナーであり、データセット、レポート、ダッシュボードを含めることができます。 **ワークスペース** は、最初に作成された時点では空です。 プレビュー期間中は、Power BI Desktop を使用してすべてのコンテンツを作成し、 [Power BI REST API](http://docs.powerbi.apiary.io/reference)を使用してコンテンツをワークスペースのいずれかにアップロードします。

## <a name="using-workspace-collections-and-workspaces"></a>ワークスペース コレクションとワークスペースの使用
**ワークスペース コレクション**と**ワークスペース**は、作成するアプリケーションの設計に最適な方法で使用、構成されるコンテンツのコンテナーです。 ワークスペース コレクションとワークスペースの中では、多様な方法でコンテンツを構成できます。 1 つのワークスペースにすべてのコンテンツを配置してから、後でアプリ トークンを使用して、顧客の間でコンテンツをさらに細かく分割することができます。 顧客が分離されるように、複数の個別ワークスペースにすべての顧客を配置することもできます。 または、顧客別ではなくリージョン別にユーザーを構成することもできます。 この柔軟な設計により、コンテンツの最適な構成方法を選択できます。

## <a name="cached-datasets"></a>キャッシュされたデータセット
キャッシュされたデータセットをプレビューに使用できます。  ただし、キャッシュされたデータは、 **Microsoft Power BI Embedded**への読み込み後に更新することはできません。

## <a name="authentication-and-authorization-with-app-tokens"></a>アプリ トークンを使用した認証と承認
**Microsoft Power BI Embedded** では、必要なユーザーの認証と承認の実行をすべてアプリケーションに委ねます。 エンド ユーザーは Azure Active Directory (Azure AD) の顧客でなければならないという明示的な要件はありません。  代わりに、アプリケーションが**アプリケーション認証トークン (アプリ トークン)** を使用して、Power BI レポートのレンダリングの承認を **Microsoft Power BI Embedded** に伝えます。  これらの **アプリ トークン** は、アプリケーションがレポートをレンダリングするときに必要に応じて生成されます。

![](media\\powerbi-embedded-whats-is\\app-tokens.png)

**アプリケーション認証トークン (アプリ トークン)** は、**Microsoft Power BI Embedded** に対する認証に使用されます。  **アプリ トークン**には、次の 3 種類があります。

1. プロビジョニング用トークン - **ワークスペース コレクション**に新しい**ワークスペース**をプロビジョニングするときに使用されます。
2. 開発用トークン - **Power BI REST API**
3. 埋め込み用トークン - 埋め込まれた iframe でのレポートのレンダリングのために呼び出しを行うときに使用されます。

これらのトークンは、 **Microsoft Power BI Embedded**とのやり取りのさまざまなフェーズに使用されます。  トークンは、許可をアプリから Power BI に委任できるように設計されています。 詳細については、 [アプリ トークンのフロー](power-bi-embedded-app-token-flow.md)に関するページを参照してください。

## <a name="see-also"></a>関連項目
* [Common Microsoft Power BI Embedded scenarios (Microsoft Power BI Embedded の一般的なシナリオ)](power-bi-embedded-scenarios.md)
* [Microsoft Power BI Embedded の概要](power-bi-embedded-get-started.md)



<!--HONumber=Nov16_HO3-->


