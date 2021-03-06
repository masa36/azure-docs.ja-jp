---
title: Windows VM を作成するさまざまな方法 | Microsoft Docs
description: リソース マネージャーで Windows 仮想マシンを作成するさまざまな方法を紹介します。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/27/2016
ms.author: cynthn

---
# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>リソース マネージャーで Windows 仮想マシンを作成するさまざまな方法
仮想マシンは多様なユーザーや目的に適しているため、Azure には仮想マシンを作成するためのさまざまな方法が用意されています。 これは、仮想マシンとその作成方法について、いくつかの選択を行う必要があることを意味します。 この記事には、これらの選択肢の概要と、手順へのリンクが記載されています。

## <a name="azure-portal"></a>Azure ポータル
Azure ポータルを使用すれば、特に Azure を初めて使用する場合に、仮想マシンを簡単に試すことができます。 

[ポータルを使用して Windows を実行する仮想マシンを作成する](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>テンプレート
仮想マシンでは、リソース (可用性セットやストレージ アカウントなど) を組み合わせる必要があります。 各リソースを個別にデプロイして管理するのではなく、1 回の連携した操作ですべてのリソースをデプロイしてプロビジョニングする Azure Resource Manager テンプレートを作成できます。

* [リソース マネージャー テンプレートで Windows 仮想マシンを作成する](virtual-machines-windows-ps-template.md)

## <a name="azure-powershell"></a>Azure PowerShell
コマンド シェルを使用する場合は、Azure PowerShell を使用することができます。

* [PowerShell を使用して Windows VM を作成する](virtual-machines-windows-ps-create.md)

## <a name="visual-studio"></a>Visual Studio
Visual Studio を使用し、Visual Studio と Azure SDK の Azure Tools で VM の構築、管理、および デプロイを行います。

[Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

<!--HONumber=Oct16_HO2-->


