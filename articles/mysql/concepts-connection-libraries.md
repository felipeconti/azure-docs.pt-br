---
title: Bibliotecas de conexão para o Banco de Dados do Azure para MySQL
description: Este artigo lista cada biblioteca ou driver que os programas de cliente podem usar ao se conectarem ao Banco de Dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 3f49065d4f66f55ed728626764d9cac2aa5c3c69
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/10/2018
ms.locfileid: "40037850"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>Bibliotecas de conexão para o Banco de Dados do Azure para MySQL
Este artigo lista cada biblioteca ou driver que os programas de cliente podem usar ao se conectarem ao Banco de Dados do Azure para MySQL.

## <a name="client-interfaces"></a>Interfaces de cliente
O MySQL oferece conectividade de driver de banco de dados padrão para usar o MySQL com aplicativos e ferramentas compatíveis com ODBC e JDBC padrões do setor. Qualquer sistema que funciona com ODBC ou JDBC pode usar o MySQL.

| **Linguagem** | **Plataforma** | **Recursos adicionais** | **Baixar** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [Driver nativo do MySQL para PHP - mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [Baixar](http://php.net/downloads.php) |
| ODBCODBC | Plataformas Windows, Linux, Mac OS X e Unix | [Guia do desenvolvedor do MySQL Connector/ODBC](https://dev.mysql.com/doc/connector-odbc/en/) | [Baixar](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [Guia do desenvolvedor do MySQL Connector/Net](https://dev.mysql.com/doc/connector-net/en/) | [Baixar](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Independente de plataforma | [Guia do desenvolvedor do MySQL Connector/J 5.1](https://dev.mysql.com/doc/connector-j/5.1/en/) | [Baixar](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [Baixar](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [Guia do desenvolvedor do MySQL Connector/Python](https://dev.mysql.com/doc/connector-python/en/) | [Baixar](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [Guia do desenvolvedor do MySQL Connector/C++](https://dev.mysql.com/doc/connector-cpp/en/) | [Baixar](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [Guia do desenvolvedor do MySQL Connector/C](https://dev.mysql.com/doc/connector-c/en/) | [Baixar](https://dev.mysql.com/downloads/connector/c/)
| Perl | Plataformas Windows, Linux, Mac OS X e Unix | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [Baixar](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Próximas etapas
Leia estes guias de início rápido para saber como se conectar e consultar o Banco de Dados do Azure para MySQL usando a linguagem de sua escolha:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [Go](./connect-go.md)

