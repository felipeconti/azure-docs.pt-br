---
title: 'Portal do Azure: consultar o Banco de Dados SQL do Azure usando o Editor de consultas | Microsoft Docs'
description: Saiba como se conectar ao Banco de dados SQL no portal do Azure usando o Editor de consultas SQL. Em seguida, execute instruções T-SQL (Transact-SQL) para consultar e editar dados.
keywords: conectar-se ao banco de dados sql,portal do azure,portal,editor de consultas
services: sql-database
author: ayoolubeko
manager: craigg
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.topic: quickstart
ms.date: 01/10/2018
ms.author: ayolubek
ms.openlocfilehash: 6e9dbeb5915f98ec4d08d8656b6b338ea78117da
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792623"
---
# <a name="azure-portal-use-the-sql-query-editor-to-connect-and-query-data"></a>Portal do Azure: usar o editor Consulta SQL para se conectar e consultar dados

O editor Consulta SQL é uma ferramenta de consulta no navegador que fornece uma maneira eficiente e leve para executar consultas SQL em seu Banco de dados SQL do Azure ou SQL Data Warehouse do Azure sem sair do portal do Azure. Este guia rápido demonstra como usar o editor de consultas para se conectar a um banco de dados SQL e depois usar instruções do Transact-SQL para consultar, inserir, atualizar e excluir dados no banco de dados.

## <a name="prerequisites"></a>pré-requisitos

Este início rápido usa como ponto de partida os recursos criados em um destes inícios rápidos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

> [!NOTE]
> Verifique se a opção “Permitir acesso aos serviços do Azure” está definida como “Ativada” nas configurações de firewall do seu SQL Server. Essa opção dá ao editor Consulta SQL o acesso aos seus bancos de dados e data warehouses.

## <a name="log-in-to-the-azure-portal"></a>Faça logon no Portal do Azure

Faça logon no [Portal do Azure](https://portal.azure.com/).


## <a name="connect-using-sql-authentication"></a>Conectar-se usando Autenticação SQL

1. Clique em **bancos de dados SQL** no menu esquerdo e clique no banco de dados que deseja consultar.

2. Na página do Banco de dados SQL para o seu banco de dados, localize e clique em **Editor de consulta (Versão Prévia)** no menu à esquerda.

    ![localizar editor de consultas](./media/sql-database-connect-query-portal/find-query-editor.PNG)

3. Clique em **Login** e, quando receber solicitação, selecione **Autenticação do SQL Server** e forneça o logon de administrador do servidor e a senha fornecida ao criar o banco de dados.

    ![logon](./media/sql-database-connect-query-portal/login-menu.png)

4. Clique em **OK para fazer logon**.


## <a name="connect-using-azure-ad"></a>Conecte-se usando o Azure Active Directory

Configurar um administrador do Active Directory permite que você use uma única identidade de logon para o portal do Azure e seu banco de dados SQL. Siga as etapas abaixo para configurar um administrador do Active Directory para o SQL Server que você criou.

> [!NOTE]
> Contas de email (por exemplo, outlook.com, hotmail.com, live.com, gmail.com, yahoo.com) ainda não contam com suporte como administradoras de Active Directory. Verifique se escolheu um usuário que foi originalmente criado ou federado no Azure Active Directory.

1. Selecione **SQL Servers** no menu à esquerda e selecione seu SQL Server na lista de servidores.

2. Selecione a configuração **Administrador do Active Directory** no menu de configurações do SQL Server.

3. Na folha do Administrador do Active Directory, clique no comando **Definir administrador** e selecione o usuário ou grupo que será o administrador do Active Directory.

    ![selecionar active directory](./media/sql-database-connect-query-portal/select-active-directory.png)

4. Na parte superior da folha do Administrador do Active Directory, clique no comando **Salvar** para definir o administrador do Active Directory.

Navegue até o banco de dados SQL o qual deseja consultar e clique em **Data Explorer (versão prévia)** no menu à esquerda. A página do Data Explorer é aberta e o conecta automaticamente ao banco de dados.


## <a name="run-query-using-query-editor"></a>Execute a consulta usando o Editor de consultas

Depois de autenticado, digite a consulta a seguir no painel do Editor de consulta para consultar os 20 principais produtos por categoria.

```sql
 SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
 FROM SalesLT.ProductCategory pc
 JOIN SalesLT.Product p
 ON pc.productcategoryid = p.productcategoryid;
```

Clique em **Executar** e reveja os resultados da consulta no painel **Resultados**.

![resultados do editor de consultas](./media/sql-database-connect-query-portal/query-editor-results.png)

## <a name="insert-data-using-query-editor"></a>Inserir dados usando o Editor de consulta

Use o código a seguir para inserir um novo produto na tabela SalesLT.Product usando a instrução [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) do Transact-SQL.

1. Na janela de consulta, substitua a consulta anterior pela seguinte consulta:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Na barra de ferramentas, clique em **Executar** para inserir uma nova linha na tabela do Produto.

## <a name="update-data-using-query-editor"></a>Atualizar dados usando o Editor de consulta

Use o código a seguir para atualizar o novo produto que você adicionou anteriormente usando a instrução [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) do Transact-SQL.

1. Na janela de consulta, substitua a consulta anterior pela seguinte consulta:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Na barra de ferramentas, clique em **Executar** para atualizar a linha especificada na tabela do Produto.

## <a name="delete-data-using-query-editor"></a>Excluir dados usando o Editor de consulta

Use o código a seguir para excluir o novo produto que você adicionou anteriormente usando a instrução [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) do Transact-SQL.

1. Na janela de consulta, substitua a consulta anterior pela seguinte consulta:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Na barra de ferramentas, clique em **Executar** para excluir a linha especificada na tabela do Produto.


## <a name="query-editor-considerations"></a>Considerações sobre o Editor de consultas

Há algumas coisas que se deve saber ao trabalhar com o Editor de consultas:

1. Verifique se a opção "Permitir acesso aos serviços do Azure" nas configurações do firewall do Azure SQL Server foi definida como "Ativada". Essa opção dá ao Editor de consultas SQL o acesso aos seus bancos de dados e data warehouses SQL.

2. Se o SQL Server estiver em uma Rede Virtual, o Editor de consultas não pode ser usado para consultar os bancos de dados no servidor.

3. Pressionar a tecla F5 atualiza a página do Editor de consultas e faz perder a consulta que está sendo trabalhada. Use o botão Executar na barra de ferramentas para executar consultas.

4. O Editor de consulta não oferece suporte para conexão ao banco de dados mestre

5. Há um tempo limite de 5 minutos para a execução da consulta.

6. O logon no Administrador do Azure Active Directory não funciona com contas que têm a autenticação de dois fatores habilitada.

7. Contas de email (por exemplo, outlook.com, hotmail.com, live.com, gmail.com, yahoo.com) ainda não contam com suporte como administradoras de Active Directory. Verifique se escolheu um usuário que foi originalmente criado ou federado no Azure Active Directory

8. O Editor de consultas só oferece suporte a projeção cilíndrica para tipos de dados geográficos.

9. Não há suporte para IntelliSense para tabelas e exibições de banco de dados. No entanto, o editor oferece suporte de preenchimento automático nos nomes que já foram digitados.


## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre o Transact-SQL com suporte em bancos de dados SQL do Azure, confira [Diferenças do Transact-SQL no banco de dados SQL](sql-database-transact-sql-information.md).
