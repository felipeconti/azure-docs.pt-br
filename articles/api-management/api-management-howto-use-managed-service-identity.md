---
title: Usar o Azure Managed Service Identity no Gerenciamento de API do Azure | Microsoft Docs
description: Saiba como usar a Identidade de Serviço Gerenciado do Azure no Gerenciamento de API
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 98aa70935a3efbbe2edb07aade85fa3ea17ce786
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150423"
---
# <a name="use-azure-managed-service-identity-in-azure-api-management"></a>Usar o Azure Managed Service Identity no Gerenciamento de API do Azure

Este artigo mostra como criar uma identidade de serviço gerenciado para uma instância do serviço de Gerenciamento de API e como acessar outros recursos. Uma identidade do serviço gerenciada gerada pelo Azure AD (Azure Active Directory) permite que a instância de Gerenciamento de API acesse de maneira fácil e segura outros recursos protegidos pelo Azure AD, como o Azure Key Vault. A identidade de serviço gerenciado é gerenciada pelo Azure e não exige provisionamento ou giro de nenhum segredo. Para obter mais informações sobre o Azure Managed Service Identity, consulte [Managed Service Identity para recursos do Azure](../active-directory/msi-overview.md).

## <a name="create-a-managed-service-identity-for-an-api-management-instance"></a>Criar uma identidade de serviço gerenciado para uma instância de Gerenciamento de API

### <a name="using-the-azure-portal"></a>Usando o portal do Azure

Para configurar uma identidade de serviço gerenciado no portal, primeiro, crie uma instância de Gerenciamento de API como normal e, em seguida, habilite o recurso.

1. Crie uma instância de Gerenciamento de API no portal, como você faria normalmente. Navegue até ele no portal.
2. Selecione **Managed Service Identity**.
3. Defina Registrar com o Azure Active Directory como Ativado. Clique em Salvar.

![Habilitar MSI](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Usando o modelo do Azure Resource Manager

Crie uma instância de Gerenciamento de API com uma identidade incluindo a seguinte propriedade na definição de recurso: 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Isso faz com que o Azure crie e gerencie a identidade para sua instância do Gerenciamento de API. 

Por exemplo, um modelo completo do Azure Resource Manager pode ter a seguinte aparência:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0"
    },
    "resources": [
        {
            "apiVersion": "2017-03-01",
            "name": "contoso",
            "type": "Microsoft.ApiManagement/service",
            "location": "[resourceGroup().location]",
            "tags": {},
            "sku": {
                "name": "Developer",
                "capacity": "1"
            },
            "properties": {
                "publisherEmail": "admin@contoso.com",
                "publisherName": "Contoso"
            },
            "identity": { 
                "type": "systemAssigned" 
            }
        }
    ]
}
```
## <a name="use-the-managed-service-identity-to-access-other-resources"></a>Usar a identidade de serviço gerenciado para acessar outros recursos

> [!NOTE]
> Atualmente, a identidade de serviço gerenciado pode ser usada para obter certificados de Azure Key Vault para nomes de domínio personalizado do Gerenciamento de API. Em breve, mais cenários receberão suporte.
> 
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Obter um certificado do Azure Key Vault

#### <a name="prerequisites"></a>pré-requisitos
1. O Key Vault que contém o certificado pfx deve estar na mesma assinatura do Azure e no mesmo Grupo de recursos que o serviço de Gerenciamento de API. Este é um requisito do modelo do Azure Resource Manager. 
2. O Tipo de conteúdo do segredo deve ser *application/x-pkcs12*. É possível usar o seguinte script para carregar o certificado:

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Se a versão do objeto do certificado não for fornecida, o Gerenciamento de API obterá automaticamente a versão mais recente do certificado depois de carregada no Key Vault. 

O exemplo a seguir mostra um modelo do Azure Resource Manager que contém as seguintes etapas:

1. Criar uma instância de Gerenciamento de API com uma identidade de serviço gerenciado
2. Atualizar as políticas de acesso de uma instância do Azure Key Vault e permitir que a instância de Gerenciamento de API obtenha os segredos dele.
3. Atualizar a instância de Gerenciamento de API configurando um nome de domínio personalizado por meio de um certificado da instância do Key Vault.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
            
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",        
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    { 
      "apiVersion": "2017-05-10", 
      "name": "apimWithKeyVault", 
      "type": "Microsoft.Resources/deployments",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": {
            "publisherEmail": { "value": "[parameters('publisherEmail')]"},
            "publisherName": { "value": "[parameters('publisherName')]"},
            "sku": { "value": "[parameters('sku')]"},
            "skuCount": { "value": "[parameters('skuCount')]"},
            "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
            "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
        }
      } 
    }]
}
```

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o Azure Managed Service Identity:

* [Managed Service Identity para recursos do Azure](../active-directory/msi-overview.md)
* [Modelos do Gerenciador de Recursos do Azure](https://github.com/Azure/azure-quickstart-templates)

