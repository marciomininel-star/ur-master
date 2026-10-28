# 12 — Fluxo gráfico, seleção do operador e manutenção por contagem

**Fontes Core:** CRG p.39–43 e 51–56; CPP slides 57–69 e 94–100; CWB p.10 e 15–16. **Advanced relacionado:** ch01–02.

## Ideia central

Uma condição escolhe o fluxo; um contador registra um evento definido. Diferencie seleção de processo, contagem de peças concluídas e interrupção contínua. Essas escolhas determinam se uma troca pode ocorrer no meio da operação ou somente entre ciclos.

## Escolha do comando

| Necessidade | Recurso do curso |
|---|---|
| Condição booleana de sensor/variável | If / ElseIf / Else |
| Selecionar uma entre opções inteiras | Switch / Case / Default |
| Repetir sempre, N vezes ou enquanto condição | Loop |
| Reutilizar sequência gráfica de operações | SubProg e Call |
| Apenas agrupar e nomear | Folder |
| Cálculo com argumentos e retorno | Função URScript (ch02) |
| Receber decisão do operador | Assignment com Source = Operator |

CPP 67 descreve retorno ao comando seguinte após Call e uma restrição da versão: subprogramas não chamam outros subprogramas. Não extrapole isso para funções URScript ou para todo software posterior. Pasta não realiza chamada e não substitui SubProg.

CRG p.39: crie/use variáveis no programa antes de configurar seus valores em Init Variables. CPP 63–64 descreve tipos definidos pela atribuição inicial e limitações antigas de listas; ch02 mantém essas limitações vinculadas à época do curso.

## If e avaliação contínua

If examina condição; Else é opcional e executa quando nenhum ramo anterior se aplica. Vários ElseIf são possíveis, mas só um Else.

CPP 59 e CRG p.51 explicam **Check expression continuously**: em If/ElseIf ou Loop, a expressão pode ser reavaliada durante a execução e interromper o corpo quando deixa de ser verdadeira. Isso é diferente de avaliar só ao entrar ou entre iterações.

**Decisão:** se a tarefa precisa concluir uma transferência inteira antes de mudar de modo, não adote interrupção contínua por hábito. Se uma condição pode interromper, defina o que acontece com movimento, garra, peça e contadores naquele ponto. O curso não fornece recuperação universal para todos os casos.

## Exemplo trabalhado — cada quinta peça para inspeção

CWB p.10 pede contador iniciado em 0, incremento por peça/ciclo e chamada de subprograma para separar cada quinta peça. Reconstrução sem comando de movimento:

```text
Inicializar contador = 0
Após pegar/concluir o evento escolhido para contagem:
  contador = contador + 1
  se contador == 5:
    chamar Inspecao
    contador = 0
  senão:
    executar colocação normal
```

O ponto exato do incremento deve ser coerente com “peça contada”: não incremente por tentativa inválida nem duas vezes pela mesma peça. A reconstrução de reset a cada cinco é uma solução ao requisito, não código transcrito.

| Peça | Contador antes da decisão | Destino |
|---:|---:|---|
| 1–4 | 1–4 | Normal |
| 5 | 5, depois reset | Inspeção |
| 6–9 | 1–4 | Normal |
| 10 | 5, depois reset | Inspeção |

O opcional permite inspeção de todas as peças por seletor e contagem `var_quality`; outro modo devolve peças inspecionadas ao fluxo e decrementa a quantidade. Separe peças processadas, peças aguardando inspeção e peças já devolvidas; os contadores representam estados diferentes.

## Exemplo trabalhado — três formatos e limpeza após sete peças

CWB p.15–16:

1. Calibrar TCP/orientação/CoG do aplicador (ch05).
2. Receber número inteiro de peça por Assignment Operator.
3. Switch com Cases 1, 2 e 3 para geometrias diferentes.
4. Default mostra erro quando entrada não é 1/2/3.
5. Após sete peças processadas, ir à posição de serviço e mostrar “Please clean nozzle”.
6. Opcional: receber do operador a quantidade de peças entre limpezas.

Reconstrução da lógica:

```text
Enquanto há cota de peças válidas antes da limpeza:
  ler seleção
  seleção 1/2/3 → executar processo correspondente e contar conclusão
  outra seleção → informar erro, sem dispensar e sem contar conclusão
Ao cumprir cota:
  ir à posição de serviço definida
  solicitar limpeza e aguardar a resposta prevista
  reiniciar cota segundo a política da aplicação
```

O WB não detalha validação da cota configurável: zero, negativos e valores fora da faixa operacional precisam de uma política. Não deixe entrada inválida consumir a contagem de sete como se uma peça tivesse sido colada.

A escolha do operador e a limpeza ocorrem nas fronteiras do processo. Para continuidade de cola/velocidade em MoveP, não espere operador no meio do traçado (ch09). A geometria dos três formatos depende de handout; ele não foi fornecido, então não gere coordenadas exatas a partir deste enunciado.

## Contagem de Loop versus contador de processo

CPP 94 descreve índice de Loop X vezes de 0 a X−1. Isso não transforma o índice em número de peças efetivamente concluídas se há Default, falha ou interrupção. A opção Program Loops Forever também pode repetir um bloco que se supunha executar uma vez. Confira as duas camadas de repetição.

## Antipadrões e conferência

- Criar variável somente no editor de inicialização.
- Confundir Folder com chamada de subprograma.
- Omitir Default e deixar entrada inválida sem tratamento.
- Contar tentativas como peças concluídas.
- Marcar avaliação contínua sem projetar interrupção.
- Confundir índices 0…N−1 com contagem de N peças válidas.
- Resetar estoque de inspeção sem retirar/devolver peças reais.

**Exercícios:** CWB p.10, 15–16. **Conexões:** ch02 funções e tipos; ch09 processos contínuos; ch10 inicialização; ch11 buffer físico; ch07 medição/eventos para força.
