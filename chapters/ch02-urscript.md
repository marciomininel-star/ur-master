# 2 — URScript, funções e dados

**Fontes:** PPP slides 19–44; RG p.10–16; WB p.11–14; AP p.30–128.

## Ideia central

Use URScript para tornar tarefas reutilizáveis e parametrizadas. Uma função recebe entradas, executa uma tarefa definida e pode retornar um valor; esse valor também pode ser uma lista.

## Integração com o programa

| Mecanismo | Quando usar | Observação do curso |
|---|---|---|
| Assignment / Expression Editor | Calcular e guardar retorno | Acompanhar valor em Variables |
| Script Code — Line | Executar uma linha/chamar função | Não presume pontos visíveis em Graphics |
| Script Code — File | Definições e múltiplas linhas | Abrir/editar `.script` |
| BeforeStart | Definir funções antes das chamadas | Inicializar dados do exercício |

PPP 22 explica que `.urp` é convertido em URScript para execução e que a instalação contribui com dados ao script. Inspecionar o script gerado ajuda a estudar uma solução da GUI; não torna todo trecho independente da instalação.

## Sintaxe essencial

Reconstrução do PPP 29, sem movimento:

```urscript
def add(a, b):
  result = a + b
  return result
end
```

`add(3, 7)` retorna `10`. Defina antes de chamar. `if`, `elif`, `else`, `while` e `def` usam `:` e os blocos terminam com `end`. URScript tem sintaxe própria e não é Python.

| Operador | Teste |
|---|---|
| `==`, `!=` | Igualdade/diferença |
| `<`, `>` | Menor/maior |
| `<=`, `>=` | Menor/maior ou igual |

RG p.13 apresenta `while` como laço do material. Atualize sua condição; um laço sem progresso pode nunca terminar. O curso não é uma especificação completa de agendamento de threads.

## Tipos, índices e escopo

PPP 41 relaciona boolean, integer, floating point, string, pose e list. Poses e listas são indexadas a partir de **zero**. RG p.16 descreve listas homogêneas, com exceção de inteiros/floats, e limitações antigas de listas de strings. Não extrapole essas restrições para outras versões.

| Variável | Acesso/persistência descritos |
|---|---|
| De instalação | Programas que usam a instalação; persistente |
| De programa | Pertence ao programa; não tratada como armazenamento persistente |
| Criada em função | Escopo da função no exemplo |
| Marcada `global` | Escopo ampliado no programa no exemplo PPP 43 |

Prefira argumentos para dependências de uma função. Os slides não cobrem todas as regras de resolução de escopo ou persistência de cada versão.

### Exemplo trabalhado — indexação

Resolução do WB p.12:

```text
lista inicial: [11,54,9,13,44,84]
lista[5] = lista[0]*6 → 66
valor = 18+4 → 22; lista[1] = 22
valor = valor+valor → 44; lista[3] = 44
resultado = lista[5]-lista[3] → 22
lista[2] = lista[5]/2 → 33
lista[1] = resultado → 22
lista final: [11,22,33,44,44,66]
```

`[5]` acessa o sexto elemento.

## Exemplo trabalhado — preço de bebida

WB 2.1 pede passar preço unitário e quantidade. Reconstrução:

```urscript
def calcular_total(quantidade, preco_unitario):
  total = quantidade * preco_unitario
  return total
end
```

1. Em BeforeStart, configure `water = 0.5` e defina a função.
2. Um Assignment com Source = Operator recebe a quantidade.
3. Outro Assignment chama `calcular_total(quantidade, water)`.
4. Confira `3 → 1.5` e `5 → 2.5` em Variables.

WB 2.2 amplia para água 0,5; limonada 1,5; cola 2,5; cerveja 3,5 e seleção numérica 1–4. A função recebe preços, escolha e quantidade, seleciona por `if/elif` e retorna total. Cola × 3 retorna 7,5. São valores didáticos do curso.

O enunciado não define política para escolha inválida ou quantidade negativa. Defina esses casos ao adaptar, sem presumir sucesso.

WB 2.3 pede outra função, sem retorno, para exibir preço em popup bloqueante. RG p.14 distingue `blocking=True` e `blocking=False`; contém o erro `Flase` em um trecho. Não reproduza esse erro. Parte da assinatura está em imagem; confirme-a antes de gerar uma chamada completa com parâmetros posicionais.

WB 2.4 simula moedas: DI0=0,1; DI1=0,2; DI2=0,5; DI3=1; DI4=2 euros. A função desconta o saldo e retorna contagens por moeda. Detecção de borda/debounce não é resolvida pelo enunciado: um nível alto mantido não equivale automaticamente a várias moedas. Essa é uma decisão de implementação, não solução pronta das fontes.

## I/Os do curso

RG p.15 apresenta:

| Grupo | Ler entrada | Escrever saída | Índices descritos |
|---|---|---|---|
| Standard | `get_standard_digital_in(n)` | `set_standard_digital_out(n, b)` | 0–7 |
| Tool | `get_tool_digital_in(n)` | `set_tool_digital_out(n, b)` | 0–1 |
| Configurable | `get_configurable_digital_in(n)` | `set_configurable_digital_out(n, b)` | 0–7 |

`b` é booleano. Também são relacionados `get_standard_digital_out`, `get_tool_digital_out` e `get_configurable_digital_out`. Atribua endereços pela instalação efetiva.

## Antipadrões

- Repetir corpo inteiro do script em cada ocorrência quando definição/chamada bastam (PPP 32).
- Omitir `end` ou `:` por tratar como Python.
- Esperar vários retornos independentes sem empacotá-los em lista.
- Confundir índices iniciados em 0 com contagem iniciada em 1.
- Tratar restrições antigas de tipos ou GUI como universais.
- Presumir que todo popup interrompe a sequência.

## Exercícios e conferência

WB 2.1–2.4: aritmética → escolha → popup → pagamento e contagens. Confira assinatura/chamada e resultados esperados para os argumentos. Os resultados aritméticos foram conferidos offline; o código não foi executado em controlador ou URSim.

**Conexões:** ch01 para estrutura; ch03 para poses; ch06 para capturar modo em BeforeStart.

## Integração Core — lógica gráfica e reutilização

**Fontes:** CRG p.39–43, 51–56; CPP 57–67, 94–98; CWB p.10, 15–16. O Core ensina Init Variables, Assignment, If/ElseIf/Else, SubProg, Loop e Switch/Case na árvore gráfica. Folder agrupa; Call executa subprograma; função URScript pode receber argumentos/retornar cálculo. Não confunda essas três formas de organização.

CRG p.39 permite inicializar variáveis já utilizadas, não criá-las naquele editor. CPP 67 registra restrição antiga de subprograma chamar outro subprograma; isso não é restrição universal de funções URScript. CPP 59 e CRG p.51 descrevem avaliação contínua de condições, capaz de interromper o corpo do bloco. Detalhes e exemplos de inspeção a cada quinta peça, seleção com Default e limpeza a cada sete peças estão no ch12. Startup e reset de contadores: ch10. Threads, Events e `force()`: complemento no ch07.

## Integração Industrial/Interfaces — linguagem e comunicação

**Fontes:** IFG p.3–13; IFP p.3–29; ICP p.20, 60, 75. Python do PC e URScript do controlador são linguagens diferentes. Os novos guias misturam `raw_input`, imports de Python 2, sintaxe de Python 3 e `end` de URScript. Não copiar o exemplo do PDF sem identificar runtime; ch20 registra correções e um exemplo Python 3.

`read_port_register` do exemplo Modbus não é sinônimo de `read_input_integer_register` de PLC/RTDE. O mesmo número pode identificar bancos diferentes. Para APIs e direção: ch14 e ch17. Para dados da câmera: ch16; para enviar URScript ao controlador: ch18. Nomes com capitalização ou espaços em títulos não são assinaturas validadas.


## Complemento dos manuais — 2026-10-28

SM511 p.16–17 documenta matrizes numéricas em SW5.11; a restrição simplificada do curso não é universal. Arrays são passados por valor (p.19–20); escopo/threads em ch26. popup tem blocking=False por padrão (p.65–66). Para código SW5.11, priorizar ch26 e ch28 sobre omissões do rascunho.


## Complemento CB3 e artigos de referência — 2026-10-28

SM341 documenta URScript3.4.1 (03/04/2017), separado de SM511. EX é coleção de exemplos com erros de aridade, nome e escala; não copiar chamadas sem revisão. Ver ch30.


## Complemento do lote ampliado — 2026-10-28

Listas com capacidade fixa reservada, métodos e structs documentados emN10/N26. Não projetar essas capacidades noURScript3.4.1 ou5.11. Ler ch41 e matriz de versões antes de gerar código.
