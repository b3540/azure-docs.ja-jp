---
title: "Azure CLI を使用したカスタム VM イメージの作成 | Microsoft Docs"
description: "チュートリアル - Azure CLI を使用してカスタム VM イメージを作成する"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: 44eac1ae8676912bc0eb461e7e38569432ad3393
ms.openlocfilehash: de8ffb5ef81ac9ef4a9217f275f2c96973948eb1
ms.contentlocale: ja-jp
ms.lasthandoff: 05/17/2017

---

# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>CLI を使用した Azure VM のカスタム イメージの作成

カスタム イメージは Marketplace のイメージに似ていますが、カスタム イメージは自分で作成します。 カスタム イメージは、アプリケーションのプリロード、アプリケーションの構成、その他の OS 構成などの構成のブートストラップを実行するために使用できます。 このチュートリアルでは、Azure 仮想マシンの独自のカスタム イメージを作成します。 学習内容は次のとおりです。

> [!div class="checklist"]
> * VM のプロビジョニングを解除して汎用化する
> * カスタム イメージを作成する
> * カスタム イメージから VM を作成する
> * サブスクリプション内のすべてのイメージを一覧表示する
> * イメージを削除する

このチュートリアルには、Azure CLI バージョン 2.0.4 以降が必要です。 バージョンを確認するには、`az --version` を実行します。 アップグレードする必要がある場合は、「[Azure CLI 2.0 のインストール]( /cli/azure/install-azure-cli)」を参照してください。 ブラウザーから [Cloud Shell](/azure/cloud-shell/quickstart) を使用することもできます。

## <a name="before-you-begin"></a>開始する前に

以下の手順では、既存の VM を取得し、新しい VM インスタンスの作成に使用できる再利用可能なカスタム イメージに変換する方法について詳しく説明します。

このチュートリアルの例を完了するには、既存の仮想マシンが必要です。 必要に応じて、この[サンプル スクリプト](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md)で仮想マシンを作成できます。 このチュートリアルを実行するときは、リソース グループと VM の名前を適宜置き換えてください。

## <a name="create-a-custom-image"></a>カスタム イメージを作成する

仮想マシンのイメージを作成するには、ソース VM のプロビジョニングと割り当てを解除し、汎用とマークすることで VM を準備する必要があります。 VM を準備できたら、イメージを作成することができます。

### <a name="deprovision-the-vm"></a>VM のプロビジョニングを解除する 

プロビジョニングを解除すると、マシン固有の情報が削除されるため、VM が汎用化されます。 この汎用化によって、1 つのイメージから多数の VM をデプロイできるようになります。 プロビジョニングの解除の際に、ホスト名は *localhost.localdomain* にリセットされます。 SSH ホスト キー、ネームサーバー構成、ルート パスワード、およびキャッシュされた DHCP リースも削除されます。

VM のプロビジョニングを解除するには、Azure VM エージェント (waagent) を使用します。 Azure VM エージェントは VM にインストールして、プロビジョニングと Azure ファブリック コントローラーとのやり取りの管理に使用します。 詳細については、「[Azure Linux エージェント ユーザー ガイド](agent-user-guide.md)」をご覧ください。

SSH を使用して VM に接続し、VM のプロビジョニングを解除するコマンドを実行します。 `+user` 引数を使用して、前回プロビジョニングされたユーザー アカウントおよび関連付けられたデータも削除します。 サンプルの IP アドレスは、使用している VM のパブリック IP アドレスで置き換えてください。

VM に SSH 接続します。
```bash
ssh azureuser@52.174.34.95
```
VM のプロビジョニングを解除します。

```bash
sudo waagent -deprovision+user -force
```
SSH セッションを閉じます。

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>VM の割り当てを解除して VM を汎用としてマークする

イメージを作成するには、VM の割り当てを解除する必要があります。 [az vm deallocate](/cli//azure/vm#deallocate) を使用して VM の割り当てを解除します。 
   
```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

最後に、[az vm generalize](/cli//azure/vm#generalize) を使用して VM の状態を一般化済みと設定すると、Azure プラットフォームは、VM が一般化されたことを認識します。 イメージを作成できるのは、一般化された VM からのみです。
   
```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>イメージの作成

これで、[az image create](/cli//azure/image#create) を使用して VM のイメージを作成できるようになりました。 次の例では、*myVM* という名前の VM から *myImage* という名前のイメージを作成します。
   
```azurecli
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>イメージからの VM の作成

イメージが用意できたので、[az vm create](/cli/azure/vm#create) を使用して、イメージから新しい VM を 1 つ以上作成できます。 次の例では、*myImage* という名前のイメージから *myVMfromImage* という名前の VM を作成します。

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>イメージの管理 

ここでは、一般的なイメージ管理タスクと Azure CLI を使用してそれらを完了する方法の例をいくつか示します。

すべてのイメージを、名前によって表形式で一覧表示します。

```azurecli
az image list \
  --resource-group myResourceGroup
```

イメージを削除します。 この例では、*myOldImage* という名前のイメージを *myResourceGroup* から削除します。

```azurecli
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、カスタム VM イメージを作成しました。 以下の方法について学習しました。

> [!div class="checklist"]
> * VM のプロビジョニングを解除して汎用化する
> * カスタム イメージを作成する
> * カスタム イメージから VM を作成する
> * サブスクリプション内のすべてのイメージを一覧表示する
> * イメージを削除する

次のチュートリアルに進み、可用性が高い仮想マシンについて学習してください。

> [!div class="nextstepaction"]
> [高可用性 VM の作成](tutorial-availability-sets.md)


