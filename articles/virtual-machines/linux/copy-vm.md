---
title: Copiar uma VM Linux usando a CLI do Azure | Microsoft Docs
description: Saiba como criar uma cópia da sua VM Linux do Azure usando a CLI do Azure e Managed Disks.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 09/25/2017
ms.author: cynthn
ms.openlocfilehash: 8d250f1289c3757d5ea862a1c195dde6f8efb0eb
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938257"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-and-managed-disks"></a>Criar uma cópia da sua VM Linux usando a CLI do Azure e Managed Disks


Este artigo mostra como criar uma cópia de sua VM (máquina virtual) do Azure executando o Linux usando a CLI do Azure 2.0 e o modelo de implantação do Azure Resource Manager. 

Você também pode [carregar e criar uma VM com base em um VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>pré-requisitos


-   Instalar a [CLI do Azure 2.0](/cli/azure/install-az-cli2)

-   Entre em uma conta do Azure com [az login](/cli/azure/reference-index#az_login).

-   Tenha uma VM do Azure para usar como origem para a cópia.

## <a name="step-1-stop-the-source-vm"></a>Etapa 1: interromper a VM de origem


Desaloque a VM de origem usando [az vm deallocate](/cli/azure/vm#az_vm_deallocate).
O seguinte exemplo desaloca a VM **myVM** no grupo de recursos chamado **myResourceGroup**:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="step-2-copy-the-source-vm"></a>Etapa 2: copiar a VM de origem


Para copiar uma máquina virtual, você deve criar uma cópia do disco rígido virtual subjacente. Por meio desse processo, você cria uma VHD especializada que contém a mesma configuração e definições que a VM de origem.

Para saber mais sobre Azure Managed Disks, veja [Visão geral dos Azure Managed Disks](../windows/managed-disks-overview.md). 

1.  Lista cada VM e o nome do disco do sistema operacional com [az vm list](/cli/azure/vm#az_vm_list). O exemplo a seguir lista todas as VMs no grupo de recursos denominado **myResourceGroup**:
    
    ```azurecli
    az vm list -g myResourceGroup \
         --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' \
         --output table
    ```

    A saída deverá ser semelhante ao seguinte exemplo:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Copie o disco, criando um novo disco gerenciado usando [az disk create](/cli/azure/disk#az_disk_create). O exemplo a seguir cria um disco chamado **myCopiedDisk** do disco gerenciado chamado **myDisk**:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  Verifique se os discos gerenciados agora em seu grupo de recursos usando [az disk list](/cli/azure/disk#az_disk_list). O exemplo a seguir lista os discos gerenciados no grupo de recursos denominado **myResourceGroup**:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="step-3-set-up-a-virtual-network"></a>Etapa 3: configurar uma rede virtual


As etapas opcionais a seguir criam uma nova rede virtual, uma sub-rede, um endereço IP público e uma placa de adaptador de rede virtual (NIC).

Se você estiver copiando uma VM para fins ou implantações adicionais de solução de problemas, não convém usar uma máquina virtual em uma rede virtual existente.

Se quiser criar uma infraestrutura de rede virtual para as VMs copiadas, siga as próximas etapas. Se não quiser criar uma rede virtual, vá para a [Etapa 4: criar uma VM](#step-4-create-a-vm).

1.  Crie a rede virtual usando [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo a seguir cria uma rede virtual chamada **myVnet** e uma sub-rede chamada **mySubnet**:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  Crie um IP público usando [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create). O exemplo a seguir cria um IP público chamado **myPublicIP** com o nome DNS de **mypublicdns**. (O nome DNS deve ser exclusivo; portanto, forneça um nome exclusivo.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  Crie a NIC usando [az network nic create](/cli/azure/network/nic#az_network_nic_create).
    O exemplo a seguir cria uma NIC chamada **myNic** anexada à sub-rede **mySubnet**:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>Etapa 4: criar uma VM

Não é possível criar uma VM usando [az vm create](/cli/azure/vm#az_vm_create).

Especifique o disco copiado gerenciado para usar como o disco do sistema operacional (--attach-os-disk) da seguinte maneira:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Próximas etapas

Para saber como usar a CLI do Azure para gerenciar sua nova VM, confira [Comandos da CLI do Azure para o Azure Resource Manager](../azure-cli-arm-commands.md).
