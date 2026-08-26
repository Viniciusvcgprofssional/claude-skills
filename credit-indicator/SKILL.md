---
name: credit-indicator
description: >
  Monitora e estima o consumo de créditos/limite de uso do Claude Pro ao longo de uma conversa,
    exibindo ao final de certas respostas um indicador percentual do quanto foi consumido da janela de 5 horas.
      Use esta skill em TODA conversa com o usuário, adicionando o indicador ao final das respostas
        sempre que o consumo estimado for relevante de mencionar (a cada ~3-5 mensagens, ou quando
          houver salto perceptível no consumo). Nunca omita o indicador em sessões longas ou com respostas pesadas.
          ---

          # Credit Indicator

          Esta skill adiciona um indicador de consumo estimado ao final de certas respostas, informando o usuário
          sobre o quanto do limite de 5 horas do plano Pro já foi utilizado na sessão atual.

          ## Como funciona o limite do Claude Pro

          - Janela de 5 horas: reset contínuo a partir da primeira mensagem da sessão
          - ~45 mensagens curtas por janela (referência base)
          - O consumo real varia conforme: tamanho das mensagens (entrada + saída); modelo usado (Opus consome ~3-5x mais que Sonnet; Haiku ~30% de Sonnet); uso de ferramentas (web search, geração de arquivos, execução de código); tamanho do contexto acumulado (histórico longo pesa mais)

          ## Modelo de estimativa de consumo

          Use esta tabela como referência para estimar o "peso" de cada resposta em pontos percentuais (%) da janela de 5h:

          | Tipo de resposta | Consumo estimado (% da janela) |
          |---|---|
          | Resposta curta/conversa | 0.5% - 1.5% |
          | Resposta média (explicação) | 1.5% - 3% |
          | Resposta longa (análise) | 3% - 6% |
          | Resposta com ferramenta | +1% - 3% adicional |
          | Geração de arquivo | +2% - 5% adicional |
          | Web search (por busca) | +1% - 2% adicional |
          | Contexto acumulado longo | multiplicador de 1.2x - 1.8x |

          ## Quando exibir o indicador

          Exiba o indicador ao final da resposta: a cada 3 a 5 mensagens na conversa; quando houver uma resposta notavelmente pesada (arquivos, ferramentas, resposta muito longa); quando o consumo estimado acumulado ultrapassar 25%, 50%, 75%; quando o usuário perguntar explicitamente sobre o consumo.

          Não exiba em respostas muito curtas e triviais (1-2 linhas), a menos que seja o momento de um checkpoint periódico.

          ## Formato do indicador

          Adicione ao final da resposta, separado por uma linha horizontal (---), neste formato:

          Consumo estimado da sessão: ~XX% da janela de 5h
          [barra de progresso] XX%
          Aproximadamente YY mensagens médias restantes.

          Use a barra de progresso com blocos cheios e vazios, em escala de 15 caracteres: 0-10%: 1-2 blocos cheios; 25%: ~4 blocos; 50%: ~8 blocos; 75%: ~11 blocos; 90%+: 14-15 blocos (adicionar aviso).

          Se o consumo estimado ultrapassar 85%, adicione um aviso: "Próximo do limite da janela. Considere iniciar uma nova conversa em breve."

          ## Controle de estado

          Como Claude não tem memória entre respostas dentro de uma skill, mantenha o consumo acumulado
          estimado com base em todo o histórico visível da conversa. A cada resposta, some o custo
          estimado de TODAS as trocas anteriores visíveis mais a resposta atual.

          Dica prática: conversas mais longas com muitas ferramentas ativadas e respostas extensas
          devem ter o multiplicador de contexto aplicado a partir da 10ª mensagem em diante.

          ## Tom do indicador

          Mantenha o indicador informativo e leve, nunca alarmista — exceto quando realmente próximo do limite.
          É uma ferramenta de consciência, não de ansiedade.
          
