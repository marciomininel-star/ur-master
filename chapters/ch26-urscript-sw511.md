# 26 — URScript e-Series SW5.11: dados, escopo e execução

**Fonte:** SM511 p.13–33. Não generalizar capacidades para qualquer versão CB3 ou rascunho do treinamento.

## Tipos e funções

SW5.11 documenta none, bool, números, pose e string. Strings são bytes: índice/tamanho não representam necessariamente caracteres Unicode. Poses usam m/rad e vetor de rotação.

SM511 p.16–17 documenta matrizes numéricas e índice [linha,coluna] a partir de zero, ampliando a simplificação dos slides antigos sobre ausência de matrizes. Operadores numéricos, dimensões e semântica de listas precisam ser conferidos; não traduzir list*scalar de Python como operação matemática URScript.

Parâmetros, inclusive arrays, são passados por valor (p.19–20). Alterar lista recebida não altera automaticamente a do chamador; retornar e atribuir explicitamente. Sem retorno explícito, função retorna None.

## Escopo e threads

Variável livre tenta primeiro ligação global existente; local explícita pode sombreá-la. O primeiro nível de indentação do programa principal tem tratamento global mesmo com qualificador local (p.22–23). Declarar intenção e evitar dependência de nomes implícitos.

Threads não recebem argumentos e retorno é descartado. run fornece handle; join espera filha; kill também alcança filhas. critical não é trava absoluta através de comandos que consomem tempo físico: sleep, sync, movimento e socketRead permitem concorrência (p.24–26). Manter seção curta e não bloqueante.

O escalonador descrito tem quadros de 2 ms/500 Hz. Loops de cálculo precisam ceder execução adequadamente; ausência de movimento não evita sobrecarga. Frequência interna não determina taxa de interface de rede.

## Escolher mecanismo de execução

| Mecanismo | Uso | Limite SW5.11 |
|---|---|---|
| def normal | Fluxo estruturado | Escopo e tempo físico normais |
| sec secundário | Pequena operação concorrente de I/O | Um passo de controle; sem sleep ou movimento |
| Interpreter Mode | Inserir instruções durante execução | Entrada no thread principal; socket 30020; sem novos globais |

SM511 p.14 descreve script por 30002, linhas com newline e bloco completo def/sec: início/final no nível externo, corpo indentado. Aceitação de texto não comprova execução ou resultado físico.

Interpreter (p.29–33) responde ack com ID ou discard por erro/ausência de programa. ack não significa conclusão de movimento. interpreter_mode bloqueia o chamador, tem opções de limpeza de fila/objetos e exige sincronização de prontidão da sessão correta. Somente detectar “ativo” não basta.

clear_interpreter remove objetos/threads criados no modo; end_interpreter não desfaz estado físico nem todos os valores modificados. abort está limitado a movej/movel na edição, com comportamento condicionado pelo blend; skipbuffer descarta instruções recebidas ainda não executadas. Não são funções de parada de segurança. statelastexecuted indica instrução que **começou** a executar, apesar do nome.

**Revisão de exemplo:** verificar versão, tipo, escopo, unidade, aridade, indentação, operações bloqueantes e saída. Erros documentais em [version-matrix.md](../references/version-matrix.md). Ensaiar cálculo puro antes de integrar movimento.


## Complemento CB3 e artigos de referência — 2026-10-28

SM341 p.9–10 descreve125Hz/8ms no contexto legado, diferente de500Hz/2ms aqui. Não transpor reservas de registradores e assinaturas SW5.11 para3.4.1. Comparação e revisão de EX em ch30.


## Complemento do lote ampliado — 2026-10-28

Listas/structs/métodosN10/N26 emch41 ampliam as capacidades5.11. Frequência interna não garante taxaRTDE/Modbus. Notas antigas de obsolescência não são universais para5.26.
