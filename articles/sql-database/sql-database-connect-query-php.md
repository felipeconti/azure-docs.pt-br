---
title: Usar PHP para consultar o Banco de Dados SQL do Azure | Microsoft Docs
description: Este tópico mostra como usar o PHP para criar um programa que se conecta a um banco de dados SQL do Azure e consultá-lo usando instruções Transact-SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: php
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 8fe343587336ff22f82ed0d1ef700fc56c86f577
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38561087"
---
# <a name="use-php-to-query-an-azure-sql-database"></a>Usar PHP para consultar um banco de dados SQL do Azure

Este início rápido demonstra como usar o [PHP](http://php.net/manual/en/intro-whatis.php) a fim de criar um programa para se conectar a um banco de dados SQL do Azure e usar instruções Transact-SQL para consultar dados.

## <a name="prerequisites"></a>pré-requisitos

Para concluir este início rápido, tenha o seguinte:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Uma [regra de firewall no nível do servidor](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) para o endereço IP público do computador que usou para este início rápido.

- O PHP e o software relacionado para seu sistema operacional instalados:

    - **MacOS**: instale o Homebrew e o PHP, instale o driver ODBC e o SQLCMD e, em seguida, instale o Driver PHP para SQL Server. Consulte [Etapas 1.2, 1.3 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/mac/).
    - **Ubuntu**: instale o PHP e outros pacotes necessários e, em seguida, instale o Driver PHP para SQL Server. Consulte [Etapas 1.2 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **Windows**: instale a versão mais recente do PHP para IIS Express, a versão mais recente dos Drivers da Microsoft para SQL Server no IIS Express, Chocolatey, o driver ODBC e SQLCMD. Consulte [Etapas 1.2 e 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>Informações de conexão do servidor SQL

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Inserir código para consultar o banco de dados SQL

1. Em seu editor de texto favorito, crie um novo arquivo, **sqltest.php**.  

2. Substitua o conteúdo pelo código a seguir e adicione os valores apropriados para seu servidor, banco de dados, usuário e senha.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes the connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Executar o código

1. No prompt de comando, execute estes comandos:

   ```php
   php sqltest.php
   ```

2. Verifique se as 20 linhas superiores são retornadas e, em seguida, feche a janela do aplicativo.

## <a name="next-steps"></a>Próximas etapas
- [Projetar seu primeiro banco de dados SQL do Azure](sql-database-design-first-database.md)
- [Drivers PHP Microsoft para SQL Server](https://github.com/Microsoft/msphpsql/)
- [Relatar problemas ou fazer perguntas](https://github.com/Microsoft/msphpsql/issues)
- [Exemplo de lógica de repetição: Conectar com resiliência ao SQL usando PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

