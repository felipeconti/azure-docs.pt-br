---
title: Usar Preferência de Leitura do MongoDB com API MongoDB do Azure Cosmos DB | Microsoft Docs
description: Saiba como usar Preferência de Leitura do MongoDB com API MongoDB do Azure Cosmos DB
services: cosmos-db
author: vidhoonv
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: ''
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: sclyon
ms.openlocfilehash: 90c8d73e32f4c99c6871ce9cdb7839cd1d380b9b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42139990"
---
# <a name="how-to-globally-distribute-reads-using-read-preference-with-the-azure-cosmos-db-mongodb-api"></a>Como distribuir globalmente leituras usando Preferência de Leitura com API MongoDB do Azure Cosmos DB 

Este artigo mostra como distribuir globalmente operações de leitura usando configurações de [Preferência de Leitura do MongoDB](https://docs.mongodb.com/manual/core/read-preference/) com API MongoDB do Azure Cosmos DB. 

## <a name="prerequisites"></a>Pré-requisitos 
Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Consulte esse artigo [Início Rápido](tutorial-global-distribution-mongodb.md) para obter instruções sobre como usar o Portal do Azure para configurar a conta do Azure Cosmos DB com distribuição global e, em seguida, conectar usando a API MongoDB.

## <a name="clone-the-sample-application"></a>Clonar o aplicativo de exemplo

Abra uma janela de terminal do Git, como git bash, e `cd` para um diretório de trabalho.  

Execute os comandos a seguir para clonar o repositório de exemplo. Com base na sua plataforma de interesse, use um dos repositórios de exemplo a seguir:

1. [Aplicativo de exemplo .NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [Aplicativo de exemplo NodeJS]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [Aplicativo de exemplo Mongoose](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-mongoose-geo-readpreference)
4. [Aplicativo de exemplo Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)
5. [Aplicativo de exemplo SpringBoot](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-spring)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>Executar o aplicativo

Dependendo da plataforma utilizada, instale os pacotes necessários e inicie o aplicativo. Para instalar dependências, siga o LEIAME incluído no repositório de aplicativo de exemplo. Por exemplo, no aplicativo de exemplo do NodeJS, use os comandos a seguir para instalar os pacotes necessários e iniciar o aplicativo.

```bash
cd mean
npm install
node index.js
```
O aplicativo tenta conectar-se a uma fonte do MongoDB e falha porque a cadeia de conexão é inválida. Siga as etapas no LEIAME para atualizar a cadeia de conexão`url`. Além disso, atualize `readFromRegion` para uma região de leitura na sua Conta do Azure Cosmos DB. As instruções a seguir são do exemplo NodeJS:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos DB account's values. 
```

Após seguir essas etapas, o aplicativo de exemplo executa e produzir a seguinte saída:

```
connected!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromSecondaryfunc query completed!
```

## <a name="read-using-read-preference-mode"></a>Leia usando o modo Preferência de Leitura

O MongoDB fornece os seguintes modos de Preferência de Leitura a serem utilizados pelos clientes:

1. PRIMARY
2. PRIMARY_PREFERRED
3. SECONDARY
4. SECONDARY_PREFERRED
5. NEAREST

Consulte a documentação detalhada [Comportamento da Preferência de Leitura do MongoDB](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) para obter detalhes sobre o comportamento de cada um desses modos de preferência de leitura. No Azure Cosmos DB, primário mapeia para a região de GRAVAÇÃO e o secundário mapeia para região de LEITURA.

Com base em cenários comuns, é usar as seguintes configurações a seguir:

1. Se **leituras de baixa latência** forem necessárias, utilize o modo de preferência de leitura **NEAREST**. Essa configuração direciona as operações de leitura para a região disponível mais próxima. Observe que, se a região mais próxima for a região de GRAVAÇÃO, essas operações serão direcionadas a essa região.
2. Se **alta disponibilidade e distribuição geográfica de leituras** forem necessárias (latência não é uma restrição), utilize o modo de preferência de leitura **SECONDARY PREFERRED**. Esta configuração direciona as operações de leitura para uma região de LEITURA disponível. Se nenhuma região de LEITURA estiver disponível, as solicitações serão direcionados para a região de GRAVAÇÃO.

O trecho de código a seguir do aplicativo de exemplo mostra como configurar a Preferência de Leitura NEAREST no NodeJS:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Da mesma forma, trecho de código abaixo mostra como configurar a Preferência de Leitura SECONDARY_PREFERRED no NodeJS:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

Consulte o repos de aplicativo de exemplo correspondente para outras plataformas, como [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) e [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Leitura usando marcas

Além do modo de Preferência de Leitura, o MongoDB permite o uso de marcas para direcionar as operações de leitura. No Azure Cosmos DB para API MongoDB, a marca `region` está incluída por padrão como parte da resposta `isMaster`:

```json
"tags": {
         "region": "West US"
      }
```

Portanto, o MongoClient pode usar a marca `region` juntamente com o nome da região para direcionar operações de leitura para regiões específicas. Para contas do Azure Cosmos DB, os nomes das regiões podem ser encontrados no Portal do Azure à esquerda em **Configurações->Replicar dados globalmente**. Essa configuração é útil para alcançar **isolamento de leitura** - casos em que o aplicativo cliente deseja direcionar operações de leitura apenas para uma região específica. Essa configuração é ideal para cenários de tipo análise/sem produção, que são executados em segundo plano e não são serviços críticos de produção.

O trecho de código a seguir do aplicativo de exemplo mostra como configurar a Preferência de Leitura com marcas no NodeJS:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

Consulte o repos de aplicativo de exemplo correspondente para outras plataformas, como [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) e [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

Neste artigo, você aprendeu a distribuir globalmente operações de leitura usando a Preferência de Leitura com API MongoDB do Azure Cosmos DB.

## <a name="clean-up-resources"></a>Limpar recursos

Se você não continuar a usar esse aplicativo, exclua todos os recursos criados por este artigo no Portal do Azure com as seguintes etapas:

1. No menu à esquerda no Portal do Azure, clique em **Grupos de recursos** e depois clique no nome do recurso criado. 
2. Em sua página de grupo de recursos, clique em **Excluir**, digite o nome do recurso para excluir na caixa de texto e depois clique em **Excluir**.

## <a name="next-steps"></a>Próximas etapas

* [Importar dados do MongoDB no Azure Cosmos DB](mongodb-migrate.md)
* [Configurar uma conta do Azure Cosmos DB replicada globalmente e utilizá-la com API MongoDB](tutorial-global-distribution-mongodb.md)
* [Desenvolver localmente com o emulador](local-emulator.md)
