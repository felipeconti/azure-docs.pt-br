---
title: Como configurar a Identidade de Serviço Gerenciada em uma VM do Azure usando um modelo
description: Instruções passo a passo para configurar uma Identidade de Serviço Gerenciada em uma VM do Azure usando um modelo do Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 4b25c82de4d2d3f4300fbb688c75be74ce63fe40
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42140593"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Configurar a Identidade de Serviço Gerenciado de um VM usando um modelo

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

A Identidade de Serviço Gerenciado fornece aos serviços do Azure uma identidade gerenciada automaticamente no Active Directory do Azure. Você pode usar essa identidade para autenticar em qualquer serviço que dá suporte à autenticação do Azure AD, incluindo o Key Vault, sem ter as credenciais no seu código. 

Neste artigo, você aprenderá a executar as seguintes operações de Identidade de Serviço Gerenciada em uma VM do Azure usando o modelo de implantação do Azure Resource Manager:

## <a name="prerequisites"></a>Pré-requisitos

- Se você não estiver familiarizado com a Identidade de Serviço Gerenciada, consulte a [seção de visão geral](overview.md). **Verifique se examinou a [diferença entre uma identidade atribuída pelo sistema e uma atribuída pelo usuário](overview.md#how-does-it-work)**.
- Se você ainda não tiver uma conta do Azure, [inscreva-se em uma conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para realizar as operações de gerenciamento deste artigo, a conta precisará das seguintes atribuições de função:
    - [Colaborador da Máquina Virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para criar uma VM e habilitar e remover a identidade gerenciada atribuída ao usuário e/ou sistema de uma VM do Azure.
    - Função de [Colaborador de Identidade Gerenciada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para criar uma identidade atribuída ao usuário.
    - Função de [Operador de Identidade Gerenciada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para atribuir e remover uma identidade atribuída ao usuário de e para uma VM.

## <a name="azure-resource-manager-templates"></a>Modelos do Gerenciador de Recursos do Azure

Assim como com o Portal do Azure e o script, os modelos do [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) permitem implantar recursos novos ou modificados definidos por um grupo de recursos do Azure. Há várias opções disponíveis para a edição e a implantação do modelo, tanto locais quanto baseadas em portal, incluindo:

   - Use um [modelo personalizado do Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite a criação de um modelo do zero ou use como base um modelo comum existente ou de [Início Rápido](https://azure.microsoft.com/documentation/templates/).
   - Derivar de um grupo de recursos existente, exportando um modelo da [implantação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) ou do [estado atual da implantação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Usar um [editor JSON local (por exemplo, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), depois carregar e implantar usando o PowerShell ou a CLI.
   - Usar o [projeto do Grupo de Recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) do Visual Studio para criar e implantar um modelo.  

Independentemente da opção escolhido, a sintaxe do modelo será a mesma durante a implantação inicial e a reimplantação. Habilitar uma identidade atribuída pelo sistema ou pelo usuário em uma VM nova ou existente é realizado da mesma maneira. Além disso, por padrão, o Azure Resource Manager faz uma [atualização incremental](../../azure-resource-manager/deployment-modes.md) para implantações.

## <a name="system-assigned-identity"></a>Identidade atribuída pelo sistema

Nesta seção, você habilitará e desabilitará uma identidade atribuída pelo sistema usando um modelo do Azure Resource Manager.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Habilitar a identidade atribuída pelo sistema durante a criação de uma VM do Azure ou de uma VM existente

1. Se você entrar no Azure localmente ou por meio do portal do Azure, use uma conta que esteja associada com a assinatura do Azure que contenha a máquina virtual.

2. Para habilitar a identidade atribuída ao sistema, carregue o modelo em um editor, localize o recurso `Microsoft.Compute/virtualMachines` de interesse dentro da seção `resources` e adicione a propriedade `"identity"` no mesmo nível que a propriedade `"type": "Microsoft.Compute/virtualMachines"`. Use a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   },
   ```

3. (Opcional) Adicione a extensão de Identidade de Serviço Gerenciada da VM como um elemento `resources`. Essa etapa é opcional, pois você pode usar o ponto de extremidade de identidade do Serviço de Metadados da Instância do Azure (IMDS) para recuperar também os tokens.  Use a seguinte sintaxe:

   >[!NOTE] 
   > O exemplo a seguir pressupõe que uma extensão de VM do Windows (`ManagedIdentityExtensionForWindows`) está sendo implantada. Você também pode configurar para Linux usando `ManagedIdentityExtensionForLinux` em vez disso, para os elementos `"name"` e `"type"`.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2018-06-01",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

4. Quando tiver concluído, as seções a seguir deverão ser adicionadas à seção `resource` do modelo e serem semelhantes ao seguinte:

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                },
            },
            {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
        }
    ]
   ```

### <a name="assign-a-role-the-vms-system-assigned-identity"></a>Atribuir uma função à identidade atribuída pelo sistema da VM

Depois que você habilitar a identidade atribuída pelo sistema na VM, recomendamos conceder uma função a ela, como o acesso de **Leitor** ao grupo de recursos no qual ele foi criado.

1. Se você entrar no Azure localmente ou por meio do portal do Azure, use uma conta que esteja associada com a assinatura do Azure que contenha a máquina virtual.
 
2. Carregue o modelo em um [editor](#azure-resource-manager-templates) e adicione as informações a seguir para conceder o acesso de **Leitor** na VM ao grupo de recursos no qual ela foi criada.  A estrutura do modelo pode variar conforme o editor e o modelo de implantação escolhido.
   
   Na seção `parameters`, adicione o seguinte:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    Na seção `variables`, adicione o seguinte:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    Na seção `resources`, adicione o seguinte:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Desabilitar uma identidade atribuída pelo sistema de um VM do Azure

Se você tiver uma VM que não precise mais de uma máquina virtual:

1. Se você entrar no Azure localmente ou por meio do portal do Azure, use uma conta que esteja associada com a assinatura do Azure que contenha a máquina virtual.

2. Carregue o modelo em um [editor](#azure-resource-manager-templates) e localize o recurso `Microsoft.Compute/virtualMachines` de interesse na seção `resources`. Caso tenha uma VM com apenas a identidade atribuída ao sistema, desabilite-a alterando o tipo de identidade para `None`.  
   
   **Microsoft.Compute/virtualMachines API versão 2018-06-01**

   Se a VM tiver identidades atribuídas ao usuário e sistema, remova `SystemAssigned` do tipo de identidade e mantenha `UserAssigned` junto com os valores de dicionário `userAssignedIdentities`.

   **Microsoft.Compute/virtualMachines API versão 2018-06-01 e anterior**
   
   Se `apiVersion` for `2017-12-01` e a VM tiver ambas as identidades atribuídas ao usuário e sistema, remova `SystemAssigned` do tipo de identidade e mantenha `UserAssigned` junto com a matriz `identityIds` das identidades atribuídas ao usuário.  
   
O exemplo a seguir mostra como remover uma identidade atribuída pelo sistema de uma VM sem nenhuma identidade atribuída pelo usuário:

```JSON
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[parameters('vmName')]",
    "location": "[resourceGroup().location]",
    "identity": { 
        "type": "None"
}
```

## <a name="user-assigned-identity"></a>Identidade atribuída pelo usuário

Nesta seção você atribui uma identidade atribuída pelo usuário a uma VM do Azure usando um modelo do Azure Resource Manager.

> [!Note]
> Para criar uma identidade atribuída pelo usuário usando um Modelo do Azure Resource Manager, consulte [Criar uma identidade atribuída pelo usuário](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Atribuir uma identidade atribuída pelo usuário a uma VM do Azure

1. No elemento `resources`, adicione a seguinte entrada para atribuir uma identidade atribuída pelo usuário à VM.  Certifique-se de substituir `<USERASSIGNEDIDENTITY>` pelo nome da identidade atribuída pelo usuário que você criou.

   **Microsoft.Compute/virtualMachines API versão 2018-06-01**

   Se `apiVersion` for `2018-06-01`, as identidades atribuídas ao usuário serão armazenadas no formato de dicionário `userAssignedIdentities` e o valor `<USERASSIGNEDIDENTITYNAME>` deverá ser armazenado em uma variável definida na seção `variables` do modelo.

   ```json
   {
       "apiVersion": "2018-06-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachines API versão 2017-12-01 e anterior**
    
   Se `apiVersion` for `2017-12-01`, as identidades atribuídas ao usuário serão armazenadas na matriz `identityIds` e o valor `<USERASSIGNEDIDENTITYNAME>` deverá ser armazenado em uma variável definida na seção `variables` do modelo.
    
   ```json
   {
       "apiVersion": "2017-12-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
           ]
       }
   }
   ```
       

2. (Opcional) Em seguida, no elemento `resources`, adicione a seguinte entrada para atribuir a extensão de identidade gerenciada para sua VM. Essa etapa é opcional, pois você pode usar o ponto de extremidade de identidade do Serviço de Metadados da Instância do Azure (IMDS) para recuperar também os tokens. Use a seguinte sintaxe:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
3. Quando tiver concluído, as seções a seguir deverão ser adicionadas à seção `resource` do modelo e serem semelhantes ao seguinte:
   
   **Microsoft.Compute/virtualMachines API versão 2018-06-01**    

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "userAssignedIdentities": {
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
            "apiVersion": "2018-06-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```
   **Microsoft.Compute/virtualMachines API versão 2017-12-01 e anterior**
   
   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "identityIds": [
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```
    

### <a name="remove-user-assigned-identity-from-an-azure-vm"></a>Remover identidade atribuída ao usuário de uma VM do Azure

Se você tiver uma VM que não precise mais de uma máquina virtual:

1. Se você entrar no Azure localmente ou por meio do portal do Azure, use uma conta que esteja associada com a assinatura do Azure que contenha a máquina virtual.

2. Carregue o modelo em um [editor](#azure-resource-manager-templates) e localize o recurso `Microsoft.Compute/virtualMachines` de interesse na seção `resources`. Caso tenha uma VM que tem apenas a identidade atribuída ao usuário, desabilite-a alterando o tipo de identidade para `None`.
 
   O exemplo a seguir mostra como remover todas as identidades atribuídas pelo usuário de uma VM sem identidades atribuídas pelo sistema:
   
   ```json
    {
      "apiVersion": "2018-06-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```
   
   **Microsoft.Compute/virtualMachines API versão 2018-06-01 e anterior**
    
   Para remover uma identidade atribuída a um único usuário de uma VM, remova-a do dicionário `useraAssignedIdentities`.

   Se você tiver uma identidade atribuída ao sistema, mantenha-a no valor `type` no valor `identity`.
 
   **Microsoft.Compute/virtualMachines API versão 2017-12-01**

   Para remover uma única identidade de usuário atribuída de uma VM, remova-a da matriz `identityIds`.

   Se você tiver uma identidade atribuída ao sistema, mantenha-a no valor `type` no valor `identity`.
   
## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma perspectiva mais ampla sobre Identidade de Serviço Gerenciada, leia a [visão geral de Identidade de Serviço Gerenciada](overview.md).

