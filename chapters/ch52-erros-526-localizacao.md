# 52 — Error Codes Directory5.26: localizar antes de diagnosticar

**Fonte:** N22ErrorCodes174p, edição5.26AllRobots. Não substituir a mensagem real do controlador por título de família. O índice da skill localiza páginas, não executa soluções do manual.

## Método: código completo → contexto → evidência → hipótese

ColeteCxAy, texto/substituições numéricas, horário, modelo/serial, software, origem(junta/controlador/ferramenta), programa/linha e estado físico. Códigos que parecem similares podem refletir famílias e ações diferentes. Uma página pode continuar descrição da anterior; ler página e vizinhas antes de concluir.

O arquivo [error-code-locator.json](../references/error-code-locator.json) contém apenas código→páginas físicas de ocorrência, derivado deN22. Ocorrência no índice não é o mesmo que início da explicação; preferir ocorrência no corpo. Não ébase completa de procedimentos de reparo. Documento original é identificado peloSHA256 no manifesto.

## Exemplos com evidência

| Código/família | Interpretação dirigida | Fonte |
|---|---|---|
| C153A0–A5 | Desvio do caminho detectado na junta indicada; verificar contato, carga/CoG/aceleração | N22 p.34–35 |
| C154 | Posição em singularidade | N22 p.35 |
| C155/C156 | Sustentação/carga/montagem ou força externa noFreedrive | N22 p.35 |
| C157A* | Colisão detectada pela junta; contexto necessário | N22 p.35 em diante |
| C343A48/50/51 | Compatibilidade dehardware/firmware/calibração requer revisão | N22 p.139 |
| C347 | NetworkMap: localizar subcódigo | N22 p.139–140 |
| C746 | CableExtender: temperatura/reconhecimento por subcódigo | N22 p.170–171 |

C153 não prova junta defeituosa; indicação da junta que detecta não identifica sozinha a causa raiz. C343A50 menciona firmware antigo para dados de calibração: atualizar por procedimento compatível difere de apagar calibração para suprimir erro.

## Correlacionar com manual do acessório

RangeExtenderN15p.22 fornece contexto deC347A5/C262A17/C746, útil quando o produto realmente está instalado. Semextender, não trocar cabo baseado apenas no títuloC746 lido em outra máquina. FalhaSafe BrakeUR Series tem comportamento operacional específico(ch38), inclusive respostaDashboard alterada.

## O que não fazer com sugestões do diretório

Muitas entradas listamrestart/update/contactarserviço como alternativas. Não transformá-las em sequência automática, ticket enviado ou autorização para alterarsoftware. Preservar logs e testar hipótese; os riscos do estado físico e a compatibilidade do ativo continuam relevantes.

Não usar loopunlock/play, desligamento reiterado ou alteração de limite para ocultar parada. Após causa corrigida, definir retomada e evidência de liberação. Se não houver dados suficientes, entregar hipóteses e próximo teste apropriado, sem diagnóstico definitivo.

**Exercício:** compararC153 comC343A50. Resultado: primeiro investiga trajetória/carga/contato; segundo compatibilidade/calibração/firmware, sempre com código completo, logs e versão.
