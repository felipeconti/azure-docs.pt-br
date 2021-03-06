---
title: Como criar um modelo acústico com o Serviço de Fala - Serviços Cognitivos da Microsoft
description: Aprenda como criar um modelo acústico com o Serviço de Fala nos Serviços Cognitivos da Microsoft.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 7f7e008e8fb999ce28cf515fe9af549c309316d4
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285169"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Criar um modelo acústico personalizado

Criar um modelo acústico personalizado é útil se seu aplicativo foi projetado para uso em um ambiente específico, como um carro, com condições ou dispositivos de gravação específicos ou pela população de determinado usuário. Os exemplos envolvem fala e ruídos de fundo específicos ou o uso de um microfone específico para gravação.

Nessa página, você aprenderá a:
> [!div class="checklist"]
> * Preparar os dados
> * Importar o conjunto de dados acústico
> * Criar o modelo acústico personalizado

Se você não tiver uma conta dos Serviços Cognitivos, crie uma [conta gratuita](https://azure.microsoft.com/try/cognitive-services) antes de começar.

## <a name="prerequisites"></a>pré-requisitos

Verifique se sua conta de Serviços Cognitivos está conectada a uma assinatura abrindo a página [Assinaturas de Serviços Cognitivos](https://customspeech.ai/Subscriptions).

Você pode se conectar a uma assinatura do Serviço de Fala criada no portal do Azure clicando no botão **Conectar-se a uma assinatura existente**.

Para obter informações sobre como criar uma assinatura do Serviço de Fala no portal do Azure, consulte a página de [introdução](get-started.md).

## <a name="prepare-the-data"></a>Preparar os dados

Para personalizar um modelo acústico para um domínio específico, é necessário ter uma coleção de dados de fala. Isso pode variar de dois enunciados a várias centenas de horas de fala. Essa coleção consiste em um conjunto de arquivos de áudio com dados de fala e um arquivo de texto com transcrições de cada arquivo de áudio. Os dados de áudio devem ser representativos do cenário no qual você gostaria de usar o reconhecedor.

Por exemplo: 

*   Se você deseja reconhecer melhor a fala em um ambiente de fábrica com ruídos, os arquivos de áudio devem conter pessoas que falem em uma fábrica com ruídos.
*   Se você estiver interessado em otimizar o desempenho de um único palestrante, por exemplo, será melhor transcrever os discursos de um palestrante e os arquivos de áudio devem consistir em muitos exemplos apenas dessa pessoa.

Um conjunto de dados acústico para personalizar o modelo acústico consiste em duas partes: (1) um conjunto de arquivos de áudio que contém dados de fala e (2) um arquivo que contém as transcrições de todos os arquivos de áudio.

### <a name="audio-data-recommendations"></a>Recomendações para os dados de áudio

*   Todos os arquivos de áudio no conjunto de dados devem ser armazenados no formato de áudio WAV (RIFF).
*   O áudio deve ter uma taxa de amostragem de 8 kHz ou 16 kHz e os valores de exemplo devem ser armazenados como inteiros de 16-bit PCM com sinal (curtos).
*   Há suporte apenas para arquivos de áudio de canal único (mono).
*   Os arquivos de áudio devem ter entre 100 ms e 1 minuto de duração. Cada arquivo de áudio deve, idealmente, começar e terminar com pelo menos 100 ms de silêncio; é comum algo entre 500 ms e 1 segundo.
*   Se você tiver ruído de fundo em seus dados, é recomendável também ter alguns exemplos com mais segmentos de silêncio, por exemplo, alguns segundos antes e/ou depois do conteúdo de fala em seus dados.
*   Cada arquivo de áudio deve consistir em uma única expressão, por exemplo, uma única frase para ditado, uma única consulta ou uma única rodada do sistema de diálogo.
*   Cada arquivo de áudio no conjunto de dados deve ter um nome de arquivo exclusivo e a extensão "wav".
*   O conjunto de arquivos de áudio deve ser colocado em uma única pasta sem subdiretórios, e todo o conjunto de arquivos de áudio deve ser empacotado como um único arquivo ZIP.

> [!NOTE]
> As importações de dados pelo portal da Web atualmente estão limitadas a 2 GB e, portanto, esse é o tamanho máximo de um conjunto de dados acústico. Isso corresponde a cerca de 17 horas de áudio gravado a 16 kHz ou a 34 horas de áudio gravado a 8 kHz. Os principais requisitos para os dados de áudio estão resumidos na tabela a seguir.
>

| Propriedade | Valor |
|---------- |----------|
| Formato de arquivo | RIFF (WAV) |
| Taxa de amostragem | 8000 ou 16.000 Hz |
| Canais | 1 (mono) |
| Formato de exemplo | PCM, inteiros de 16 bits |
| Duração do arquivo | 0,1 segundo < duração < 60 segundos |
| Anel de silêncio | > 0,1 segundo |
| Formato de arquivo | Zip |
| Tamanho máximo de arquivo | 2 GB |

## <a name="language-support"></a>Suporte ao idioma

Os idiomas a seguir têm suporte para modelos de linguagem de **Conversão de fala em texto** personalizados.

Clique para obter uma lista completa de [linguagens com suporte](supported-languages.md)

### <a name="transcriptions-for-the-audio-dataset"></a>Transcrições do conjunto de dados de áudio

As transcrições para todos os arquivos WAV devem estar contidas em um único arquivo de texto sem formatação. Cada linha do arquivo de transcrição deve ter o nome de um dos arquivos de áudio, seguidos da transcrição correspondente. O nome do arquivo e transcrição devem ser separados por uma tabulação (\t).

  Por exemplo: 
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> A transcrição deve ser codificada como UTF-8 Bom

As transcrições serão normalizadas para texto para processamento pelo sistema. No entanto, há algumas normalizações importantes que devem ser executadas pelo usuário _antes_ de carregar os dados para o Serviço de Fala Personalizado. Consulte a seção sobre [diretrizes de transcrição](prepare-transcription.md) no idioma apropriado na hora de preparar sua transcrições.

As etapas a seguir são feitas usando o [Portal dos Serviços de Fala](https://customspeech.ai).

## <a name="import-the-acoustic-data-set"></a>Importar o conjunto de dados acústico

Depois que os arquivos de áudio e as transcrições foram preparadas, eles estarão prontos para importação no portal da Web do serviço.

Para fazer isso, primeiro verifique se você está conectado ao [Portal dos Serviços de Fala](https://customspeech.ai). Depois, clique no menu suspenso de “Fala Personalizada” na faixa de opções e selecione “Dados de Adaptação”. Se esta for a primeira vez fazendo o carregamento de dados para o Serviço de Fala Personalizado, você verá uma tabela vazia chamada “Conjuntos de dados”. 

Clique no botão “Importar” na linha “Conjuntos de dados acústicos”; o site exibirá uma página para carregar um novo conjunto de dados.

![experimentar](media/stt/speech-acoustic-datasets-import.png)

Insira um _Nome_ e uma _Descrição_ nas caixas de texto apropriadas. É útil usar descrições amigáveis para manter o controle de vários conjuntos de dados carregados. Em seguida, clique em "Escolher Arquivo" em "Arquivo de Transcrição" e "Arquivos WAV" e selecione o arquivo de transcrição de texto sem formatação e o de arquivos WAV compactado, respectivamente. Quando a preparação for concluída, clique em "Importar" para carregar os dados. Seus dados serão carregados em seguida. Para grandes conjuntos de dados, isso pode levar vários minutos.

Quando o upload for concluído, você retornará para a tabela "Conjuntos de dados acústicos" e verá uma entrada que corresponde ao seu conjunto de dados acústico. Observe que um GUID (ID) exclusivo foi atribuído a ela. Os dados também terão um status que reflete o estado atual. O status será: “NotStarted”, enquanto estiver na fila para processamento; “Running”, durante a validação; e “Complete”, quando os dados estiverem prontos para uso.

A validação de dados inclui uma série de verificações nos arquivos de áudio para confirmar o formato do arquivo, o comprimento e a taxa de amostragem e, na transcrição arquivos, para confirmar o formato do arquivo e executar normalização do texto.

Quando o status aparece como "Êxito", você pode clicar em "Detalhes" para ver o relatório de verificação dos dados acústicos. O número de enunciados que passaram ou falharam na verificação será exibido, juntamente com detalhes sobre os enunciados com falha. No exemplo abaixo, dois arquivos WAV tiveram falha na verificação devido ao formato de áudio inadequado (nesse conjunto de dados, um tinha uma taxa de amostragem incorreta e o outro tinha um formato de arquivo incorreto).

![experimentar](media/stt/speech-acoustic-datasets-report.png)

Em algum momento, se você quiser alterar o nome ou a descrição do conjunto de dados, poderá clicar no link "Editar" e alterar essas entradas. Você não pode modificar os arquivos de áudio ou as transcrições.

## <a name="create-a-custom-acoustic-model"></a>Criar um modelo acústico personalizado

Depois que o status do conjunto de dados acústico aparece como "Concluído", ele pode ser usado para criar um modelo acústico personalizado. Para fazer isso, clique em "Modelos Acústicos" no menu suspenso "Fala Personalizada". Você verá uma tabela chamada "Seus modelos" que lista todos os modelos acústicos personalizados. Esta tabela estará vazia se esse for seu primeiro uso. A localidade atual é refletida no título da tabela. Atualmente,os modelos acústicos podem ser criados apenas para inglês dos EUA.

Para criar um novo modelo, clique em "Criar Novo" sob o título da tabela. Como anteriormente, insira um nome e uma descrição para ajudar a identificar esse modelo. Por exemplo, o campo "Descrição" pode ser usado para registrar qual conjunto de dados acústicos e modelo inicial foram usados para criar o modelo. Em seguida, selecione um “Modelo Acústico Base” no menu suspenso. O modelo de base é o modelo ponto de partida para sua personalização. Há dois modelos acústicos base para escolher. O _MA de pesquisa e ditado da Microsoft_ é apropriado para fala dirigida a um aplicativo, como comandos, consultas de pesquisa ou ditados. O _modelo de conversa da Microsoft_ é apropriado para reconhecer a fala falada em um estilo conversacional. Esse tipo de fala normalmente é direcionado a outra pessoa e ocorre em call centers ou em reuniões. A latência de resultados parciais em modelos de conversa é maior do que em modelos de pesquisa e ditado.

> [!NOTE]
> No momento, estamos desenvolvendo nosso novo modelo Universal que pretende resolver todos os cenários. Os modelos mencionados anteriormente também permanecerão disponíveis publicamente.

Em seguida, selecione os dados acústicos que você deseja usar para realizar a personalização usando o menu suspenso.

![experimentar](media/stt/speech-acoustic-models-create2.png)

Você pode optar por executar o teste de precisão do novo modelo quando o processamento for concluído. Essa ação executará uma avaliação de fala em texto em um conjunto de dados acústico especificado usando o modelo acústico personalizado e o relatório de resultados. Para executar esse teste, marque a caixa de seleção "Teste de Precisão". Em seguida, selecione um modelo acústico no menu suspenso. Se você não tiver criado modelos de linguagem personalizados, apenas os modelos de linguagem base estarão na lista suspensa. Consulte a [descrição](how-to-customize-language-model.md) dos modelos de linguagem base no guia e selecione o mais apropriado.

Por fim, selecione o conjunto de dados acústicos que você deseja usar para avaliar o modelo personalizado. Se você executar o teste de precisão, é importante selecionar dados acústicos diferentes daqueles usados para a criação do modelo a fim de ter uma noção realista do desempenho do modelo. Testar a precisão com os dados de treinamento não permitirá que você avalie como o modelo adaptado se sai sob condições reais. O resultado será muito otimista. Observe também que o teste de precisão é limitado a 1000 enunciados. Se o conjunto de dados acústicos para teste for maior, somente os primeiros 1000 enunciados serão avaliados.

Quando você estiver pronto para começar a executar o processo de personalização, pressione "Criar".

Agora você verá uma nova entrada na tabela de modelos acústicos correspondente a esse novo modelo. O status do processo é refletido na tabela. O status de estados são "Waiting", "Processing" e "Complete".

![experimentar](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Próximas etapas

- [Obtenha sua assinatura de avaliação de fala](https://azure.microsoft.com/try/cognitive-services/)
- [Como reconhecer fala no C#](quickstart-csharp-dotnet-windows.md)
- [Dados de exemplo do Git](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
