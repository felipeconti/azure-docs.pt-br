---
title: Implantar e gerenciar backups para VMs implantadas com o Gerenciador de Recursos usando o PowerShell
description: Use o PowerShell para implantar e gerenciar backups no Azure para VMs implantadas com o Gerenciador de Recursos
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/06/2018
ms.author: markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d8e2d3e6a303009f5718a86772cdc3db8ed332a
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523845"
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a>Usar os cmdlets AzureRM.RecoveryServices.Backup para fazer backup de máquinas virtuais

Este artigo mostra como usar os cmdlets do Azure PowerShell para fazer backup e recuperar uma VM (máquina virtual) do Azure de um cofre dos Serviços de Recuperação. Um cofre dos Serviços de Recuperação é um recurso do Azure Resource Manager usado para proteger dados e ativos nos serviços de Backup do Azure e do Azure Site Recovery. Você pode usar um cofre dos Serviços de Recuperação para proteger as VMs implantadas no Azure Service Manager, bem como as VMs implantadas no Azure Resource Manager.

> [!NOTE]
> O Azure tem dois modelos de implantação para criar e trabalhar com recursos: [Gerenciador de Recursos e Clássico](../azure-resource-manager/resource-manager-deployment-model.md). Este artigo destina-se a VMs criadas usando o modelo do Resource Manager.
>
>

Ele guiará você sobre como usar o PowerShell para proteger uma VM e como restaurar dados de um ponto de recuperação.

## <a name="concepts"></a>Conceitos
Se você não estiver familiarizado com o serviço de Backup do Azure, para ter uma visão geral do serviço, confira [O que é o Backup do Azure?](backup-introduction-to-azure-backup.md) Antes de começar, é importante ter noções básicas sobre os pré-requisitos necessários para trabalhar com o Backup do Azure e as limitações da atual solução de backup de VM.

Para usar efetivamente o PowerShell, é necessário compreender a hierarquia de objetos e de onde começar.

![Hierarquia de objetos dos Serviços de Recuperação](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Para exibir a referência do cmdlet AzureRm.RecoveryServices.Backup do PowerShell, veja [Backup do Azure – cmdlets dos Serviços de Recuperação](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) na biblioteca do Azure.

## <a name="setup-and-registration"></a>Configuração e registro
Para começar:

1. [Baixe a versão mais recente do PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (a versão mínima necessária é: 1.4.0)

2. Localize os cmdlets do PowerShell do Backup do Azure disponíveis digitando o seguinte comando:
    ```PS
    PS C:\> Get-Command *azurermrecoveryservices*
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
    Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
    Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
    Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
    Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
    Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
    Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
    Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
    ```
3. Entre em sua conta do Azure usando **Connect-AzureRmAccount**. Esse cmdlet abre uma página da Web que solicita suas credenciais de conta: 
    - Como alternativa, é possível incluir as credenciais de conta como um parâmetro no cmdlet **Connect-AzureRmAccount**, usando o parâmetro **-Credential**.
    - Se você é um parceiro CSP trabalhando em nome de um locatário, especifique o cliente como um locatário usando sua tenantID ou o nome de domínio primário do locatário. Por exemplo: **Connect-AzureRmAccount -Tenant "fabrikam.com"**
4. Associe a assinatura que deseja usar com a conta, uma vez que uma conta pode ter várias assinaturas:

    ```PS
    PS C:\> Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Se você estiver usando um Backup do Azure pela primeira vez, deverá usar o cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** para registrar o provedor do Serviço de Recuperação do Azure com sua assinatura.

    ```PS
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Você pode verificar se os provedores foram registrados com êxito usando os seguintes comandos:
    ```PS
    PS C:\> Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ``` 
Na saída do comando, o **RegistrationState** deve ser definido como **Registrado**. Caso não esteja, basta executar novamente o cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** mostrado acima.

As seguintes tarefas podem ser automatizadas com o PowerShell:

* [Criar um cofre dos Serviços de Recuperação](backup-azure-vms-automation.md#create-a-recovery-services-vault)
* [Fazer backup de VMs do Azure](backup-azure-vms-automation.md#back-up-azure-vms)
* [Disparar um trabalho de backup](backup-azure-vms-automation.md#trigger-a-backup-job)
* [Monitorar um trabalho de backup](backup-azure-vms-automation.md#monitoring-a-backup-job)
* [Restaurar uma VM do Azure](backup-azure-vms-automation.md#restore-an-azure-vm)

## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação

As etapas a seguir orientarão você durante a criação de um cofre dos Serviços de Recuperação. Um cofre dos Serviços de Recuperação é diferente de um cofre de Backup.

1. O cofre dos Serviços de Recuperação é um recurso do Resource Manager e, portanto, você precisará colocá-lo em um grupo de recursos. Você pode usar um grupo de recursos existente ou criar um grupo de recursos com o cmdlet **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**. Ao criar um grupo de recursos, especifique o nome e o local para o grupo de recursos.  

    ```PS
    PS C:\> New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Use o cmdlet **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** para criar um cofre dos Serviços de Recuperação. Lembre-se de especificar o mesmo local para o cofre usado para o grupo de recursos.

    ```PS
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Especifique o tipo de redundância de armazenamento a usar. Você pode usar o [Armazenamento com Redundância Local (LRS)](../storage/common/storage-redundancy-lrs.md) ou o [Armazenamento com Redundância Geográfica (GRS)](../storage/common/storage-redundancy-grs.md). O exemplo a seguir mostra que a opção BackupStorageRedundancy para o testvault está definida como GeoRedundant.

    ```PS
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Muitos cmdlets do Backup do Azure exigem o objeto do cofre dos Serviços de Recuperação como entrada. Por esse motivo, pode ser útil armazenar o objeto do cofre dos Serviços de Recuperação de backup em uma variável.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Exibir os cofres em uma assinatura
Use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** para exibir a lista de todos os cofres da assinatura atual. Você pode usar esse comando para verificar se um novo cofre foi criado ou para ver quais cofres estão disponíveis na assinatura.

Execute o comando Get-AzureRmRecoveryServicesVault para exibir todos os cofres na assinatura. O exemplo a seguir mostra as informações exibidas para cada cofre.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Fazer backup de VMs do Azure
Use um cofre dos Serviços de Recuperação para proteger as máquinas virtuais. Antes de aplicar a proteção, defina o contexto de cofre (o tipo de dados protegidos no cofre) e verifique a política de proteção. A política de proteção é a agenda de quando o trabalho de backup é executado e de quanto tempo cada instantâneo de backup é mantido.

### <a name="set-vault-context"></a>Definir o contexto de cofre
Antes de habilitar a proteção em uma VM, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** para definir o contexto de cofre. Depois que o contexto de cofre é definido, ele se aplica a todos os cmdlets subsequentes. O exemplo a seguir define o contexto de cofre para o cofre, *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Crie uma política de proteção
Quando você cria um cofre dos Serviços de Recuperação, ele vem com proteção e políticas de retenção padrão. A política de proteção padrão dispara um trabalho de backup diariamente em um horário especificado. A política de retenção padrão retém o ponto de recuperação diário por 30 dias. Você pode usar a política padrão para proteger rapidamente sua VM e editá-la posteriormente com detalhes diferentes.

Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** para exibir as políticas de proteção disponíveis no cofre. Você pode usar este cmdlet para obter uma política específica ou para exibir as políticas associadas a um tipo de carga de trabalho. O exemplo a seguir obtém as políticas para o tipo de carga de trabalho, AzureVM.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> O fuso horário do campo BackupTime no PowerShell é UTC. No entanto, quando o tempo de backup é mostrado no Portal do Azure, o horário é ajustado para seu fuso horário local.
>
>

Uma política de proteção de backup está associada a pelo menos uma política de retenção. A política de retenção define por quanto tempo um ponto de recuperação é mantido até ser excluído. Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** para exibir a política de retenção padrão.  Da mesma forma, você pode usar **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** para obter a política de agendamento padrão. O cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cria um objeto do PowerShell que mantém as informações da política de backup. Os objetos de política de retenção e agendamento são usados como entradas para o cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**. O exemplo a seguir armazena a política de agendamento e a política de retenção em variáveis. O exemplo usa essas variáveis para definir os parâmetros ao criar uma política de proteção, *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Habilitar proteção
Depois de definir a política de proteção de backup, você deve habilitar a política para um item. Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** para habilitar a proteção. Habilitar a proteção envolve dois objetos – o item e a política. Depois de a política ter sido associada ao cofre, o fluxo de trabalho de backup será disparado no momento definido no agendamento da política.

O exemplo a seguir habilita a proteção para o item, V2VM, usando a política, NewPolicy. Para habilitar a proteção em VMs do Resource Manager não criptografadas

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Para habilitar a proteção em VMs criptografadas (criptografadas usando BEK e KEK), você precisa conceder permissão para o serviço de Backup do Azure ler as chaves e os segredos do cofre de chaves.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Para habilitar a proteção em VMs criptografadas (criptografadas usando apenas BEK), você precisa dar permissão de serviço de Backup do Azure para ler segredos do cofre de chaves.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Se você estiver usando a nuvem do Azure Governamental, use o valor ff281ffe-705c-4f53-9f37-a40e6f2c68f3 para o parâmetro **-ServicePrincipalName** no cmdlet [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).
>
>

Para VMs clássicas

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Modificar uma política de proteção
Para modificar a política de proteção, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) para modificar os objetos SchedulePolicy ou RetentionPolicy.

O exemplo a seguir altera retenção de ponto de recuperação para 365 dias.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Disparar um backup
Você pode usar **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** para disparar um trabalho de backup. Se esse é o backup inicial, ele é um backup completo. Os backups posteriores fazem uma cópia incremental. Você deve usar **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** para definir o contexto de cofre antes de disparar o trabalho de backup. O exemplo a seguir pressupõe que o contexto de cofre foi definido.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> O fuso horário dos campos StartTime e EndTime mostrado no PowerShell é UTC. No entanto, quando a hora é exibida no Portal do Azure, ela é ajustada para seu fuso horário local.
>
>

## <a name="monitoring-a-backup-job"></a>Monitoramento de um trabalho de backup
Você pode monitorar operações de longa duração, como trabalhos de backup, sem usar o Portal do Azure. Para obter o status de um trabalho em andamento, use o cmdlet **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**. Esse cmdlet obtém os trabalhos de backup para um cofre específico, o qual está especificado no contexto de cofre. O exemplo a seguir obtém o status de um trabalho em andamento como uma matriz e armazena o status na variável $joblist.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Em vez de sondar esses trabalhos para conclusão (o que é um código adicional desnecessário), é mais simples usar o cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**. Esse cmdlet pausa a execução até que o trabalho seja concluído ou o valor de tempo limite especificado seja atingido.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Restaurar uma VM do Azure
Há uma diferença importante entre a restauração de uma máquina virtual usando o portal do Azure e a restauração de uma máquina virtual usando o PowerShell. Com o PowerShell, a operação de restauração é concluída quando são criadas as informações dos discos e de configuração do ponto de recuperação. Se quiser restaurar ou recuperar alguns arquivos de um backup de VM do Azure, consulte a [seção de recuperação de arquivos](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)

> [!NOTE]
> A operação de restauração não cria uma máquina virtual.
>
>

Para criar uma máquina virtual com base no disco, consulte a seção [Criar a VM com base em discos restaurados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). As etapas básicas para restaurar uma VM do Azure são:

* Selecione a VM
* Escolha um ponto de recuperação
* Restaure os discos
* Crie a VM de discos armazenados

O gráfico a seguir mostra a hierarquia de objetos do RecoveryServicesVault até o BackupRecoveryPoint.

![Hierarquia de objetos dos Serviços de Recuperação mostrando BackupContainer](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Para restaurar dados de backup, identifique o item de backup e o ponto de recuperação que mantém os dados pontuais. Use o cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** para restaurar dados do cofre para a conta do cliente.

### <a name="select-the-vm"></a>Selecione a VM
Para obter o objeto do PowerShell que identifica o item correto de backup, comece do contêiner no cofre e desça progressivamente na hierarquia de objetos. Para selecionar o contêiner que representa a VM, use o cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** e o redirecione para o cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Escolha um ponto de recuperação
Use o cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** para listar todos os pontos de recuperação para o item de backup. Em seguida, escolha o ponto de recuperação a ser restaurado. Se você não tiver certeza de qual ponto de recuperação será usado, é uma boa prática escolher o mais recente ponto RecoveryPointType = AppConsistent na lista.

No script a seguir, a variável **$rp** é uma matriz de pontos de recuperação dos últimos sete dias para o item de backup selecionado. A matriz é classificada em ordem inversa de tempo com o último ponto de recuperação no índice 0. Use a indexação de matriz padrão do PowerShell para selecionar o ponto de recuperação. No exemplo, $rp[0] seleciona o último ponto de recuperação.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-the-disks"></a>Restaure os discos
Use o cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** para restaurar os dados de backup e a configuração de um item de backup para um ponto de recuperação. Depois de ter identificado um ponto de recuperação, use-o como o valor do parâmetro **-RecoveryPoint**. No código de exemplo anterior, **$rp [0]** foi o ponto de recuperação a ser usado. No código de exemplo abaixo, **$rp [0]** é o ponto de recuperação a ser usado para a restauração do disco.

Para restaurar as informações de discos e de configuração:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Use o cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** para aguardar a conclusão do trabalho de Restauração.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Após a conclusão do trabalho de Restauração, use o cmdlet **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** para obter os detalhes da operação de restauração. A propriedade JobDetails tem as informações necessárias para recompilar a VM.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Depois de restaurar os discos, vá para a próxima seção para criar a VM.

## <a name="create-a-vm-from-restored-disks"></a>Criar uma máquina virtual de discos restaurados
Depois de ter restaurado os discos, use estas etapas para criar e configurar uma máquina virtual do disco.

> [!NOTE]
> Para criar VMs criptografadas de discos restaurados, a função do Azure deverá ter permissão para executar a ação **Microsoft.KeyVault/vaults/deploy/action**. Se sua função não tem essa permissão, crie uma função personalizada com esta ação. Para obter mais informações, veja [Funções personalizadas no RBAC do Azure](../role-based-access-control/custom-roles.md).
>
>

1. Consulte as propriedades do disco restaurado para obter os detalhes do trabalho.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $configBlobName = $properties["Config Blob Name"]
  ```

2. Defina o contexto do armazenamento do Azure e restaure o arquivo de configuração JSON.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. Use o arquivo de configuração JSON para criar a configuração da VM.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. Anexe o disco do sistema operacional e os discos de dados. Dependendo da configuração das suas VMs, confira a seção relevante para ver os respectivos cmdlets:

    #### <a name="non-managed-non-encrypted-vms"></a>VMs não criptografadas não gerenciadas

    Use a amostra a seguir para VMs não criptografadas não gerenciadas.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>VMs criptografadas e não gerenciadas (apenas BEK)

    Para VMs criptografadas e não gerenciadas (criptografadas usando apenas BEK), você precisa restaurar o segredo para o cofre de chaves para poder anexar discos. Para obter mais informações, consulte o artigo [Restaurar uma máquina virtual criptografada de um ponto de recuperação do Backup do Azure](backup-azure-restore-key-secret.md). O exemplo a seguir mostra como anexar discos de dados e de SO a VMs criptografadas.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    ```
    
 Durante a configuração do disco do sistema operacional, verifique se o tipo de sistema operacional relevante é mencionado   
    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```
    
A criptografia de discos de dados deve ser habilitada manualmente usando o comando a seguir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -VolumeType Data
    ```
    
   #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>VMs criptografadas e não gerenciadas (BEK e KEK)

   Para VMs criptografadas e não gerenciadas (criptografadas usando BEK e KEK), você precisa restaurar a chave e o segredo para o cofre de chaves para poder anexar discos. Para obter mais informações, consulte o artigo [Restaurar uma máquina virtual criptografada de um ponto de recuperação do Backup do Azure](backup-azure-restore-key-secret.md). O exemplo a seguir mostra como anexar discos de dados e de SO a VMs criptografadas.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

A criptografia de discos de dados deve ser habilitada manualmente usando o comando a seguir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
   #### <a name="managed-non-encrypted-vms"></a>VMs não criptografadas gerenciadas

   Para VMs não criptografadas gerenciadas, você precisará criar discos gerenciados no armazenamento de blobs e, em seguida, anexar os discos. Para obter informações detalhadas, consulte o artigo [Anexar um disco de dados a uma VM do Windows usando o PowerShell](../virtual-machines/windows/attach-disk-ps.md). O código de exemplo a seguir mostra como anexar os discos de dados a VMs não criptografadas gerenciadas.

    ```
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

   #### <a name="managed-encrypted-vms-bek-only"></a>VMs criptografadas e gerenciadas (apenas BEK)

   Para VMs criptografadas e gerenciadas (criptografadas usando somente BEK), você precisará criar Managed Disks do armazenamento de blobs e então anexar os discos. Para obter informações detalhadas, consulte o artigo [Anexar um disco de dados a uma VM do Windows usando o PowerShell](../virtual-machines/windows/attach-disk-ps.md). O código de exemplo a seguir mostra como anexar os discos de dados a VMs criptografadas gerenciadas.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

A criptografia de discos de dados deve ser habilitada manualmente usando o comando a seguir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
   #### <a name="managed-encrypted-vms-bek-and-kek"></a>VMs criptografadas e gerenciadas (BEK e KEK)

   Para VMs criptografadas e gerenciadas (criptografadas usando BEK e KEK), você precisará criar discos gerenciados do armazenamento de blobs e, em seguida, anexar os discos. Para obter informações detalhadas, consulte o artigo [Anexar um disco de dados a uma VM do Windows usando o PowerShell](../virtual-machines/windows/attach-disk-ps.md). O código de exemplo a seguir mostra como anexar os discos de dados a VMs criptografadas gerenciadas.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```
A criptografia de discos de dados deve ser habilitada manualmente usando o comando a seguir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
5. Defina as configurações de Rede.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $virtualNetwork = New-AzureRmVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    PS C:\> $virtualNetwork | Set-AzureRmVirtualNetwork
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $subnetindex=0
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Crie a máquina virtual.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Restaurar arquivos de um backup de VM do Azure

Além de restauração de discos, você também pode restaurar arquivos individuais de um backup de VM do Azure. A funcionalidade de restauração de arquivos fornece acesso a todos os arquivos em um ponto de recuperação e você pode gerenciá-los por meio do explorador de arquivos como faria para os arquivos normais.

As etapas básicas para restaurar um arquivo do backup de VM do Azure são:

* Selecione a VM
* Escolha um ponto de recuperação
* Monte os discos do ponto de recuperação
* Copie os arquivos necessários
* Desmonte o disco


### <a name="select-the-vm"></a>Selecione a VM
Para obter o objeto do PowerShell que identifica o item correto de backup, comece do contêiner no cofre e desça progressivamente na hierarquia de objetos. Para selecionar o contêiner que representa a VM, use o cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** e o redirecione para o cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Escolha um ponto de recuperação
Use o cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** para listar todos os pontos de recuperação para o item de backup. Em seguida, escolha o ponto de recuperação a ser restaurado. Se você não tiver certeza de qual ponto de recuperação será usado, é uma boa prática escolher o mais recente ponto RecoveryPointType = AppConsistent na lista.

No script a seguir, a variável **$rp** é uma matriz de pontos de recuperação dos últimos sete dias para o item de backup selecionado. A matriz é classificada em ordem inversa de tempo com o último ponto de recuperação no índice 0. Use a indexação de matriz padrão do PowerShell para selecionar o ponto de recuperação. No exemplo, $rp[0] seleciona o último ponto de recuperação.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>Monte os discos do ponto de recuperação

Use o cmdlet **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)** para obter o script para montar todos os discos do ponto de recuperação.

> [!NOTE]
> Os discos são montados como discos conectados do iSCSI no computador em que o script é executado. Portanto, isso é quase instantâneo e NÃO incorrerá nenhuma cobrança
>
>

```
PS C:\> Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]

OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```
Execute o script no computador em que deseja recuperar os arquivos. Você precisa inserir a senha mostrada acima para executar o script. Depois que os discos forem anexados, use o explorador de arquivos do Windows para procurar a novos volumes e arquivos. Para obter mais informações, consulte a [documentação de recuperação de arquivos](backup-azure-restore-files-from-vm.md)

### <a name="unmount-the-disks"></a>Desmonte os discos
Depois que os arquivos necessários tiverem sido copiados, desmonte os discos usando o cmdlet **[Disable-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)**. Isso é altamente recomendável porque garante que o acesso seja removido dos arquivos de ponto de recuperação

```
PS C:\> Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```


## <a name="next-steps"></a>Próximas etapas
Se você preferir usar o PowerShell para interagir com os recursos do Azure, confira o artigo do PowerShell, [Implantar e gerenciar Backup do Windows Server](backup-client-automation.md). Se você gerencia backups do DPM, consulte o artigo [Implantar e gerenciar o backup do DPM](backup-dpm-automation.md). Esses dois artigos têm uma versão para implantações do Resource Manager, bem como para implantações do modelo Clássico.  
