# 7 — Controle de força e contato

**Fontes:** PPP slides 117–132; RG p.32–35; WB p.28–30; AP p.214–230.

## Ideia central

Escolha quais graus de liberdade devem ceder e em qual frame a força será interpretada. Uma aplicação de contato precisa de direção, magnitude, limites e condição de saída; posição programada sozinha pode não acomodar a variação da superfície.

## Seleção de tipo

| Tipo | Frame/direção | Use no contexto do curso |
|---|---|---|
| Simple | Um eixo; normalmente Z da feature; Y para Line segundo RG p.32 | Pressão unidirecional |
| Frame | Eixos da feature selecionada, até seis graus de liberdade | Compliance escolhida ou contorno por trechos |
| Point | Y aponta do TCP à origem da feature; frame varia com posição | Pressão radial e deslocamento ao redor de peça redonda |
| Motion | X é projeção da direção do movimento no plano XY da feature | Força transversal ao avanço em trajetória |

Os nomes Simple/Frame/Point/Motion são modos da GUI do curso. Não equivalem a uma tabela completa de números de tipo para uma função URScript. As fontes não fornecem aqui um programa `force_mode(...)` completo; não invente assinatura ou mapeamento numérico.

## Simple

1. Insira o nó Force.
2. Selecione Simple e a feature.
3. Defina sinal e magnitude da força.
4. Estabeleça a duração/condição que mantém o bloco ativo e sua saída.
5. Confira eixo indicado pelo assistente.

O sinal positivo/negativo escolhe o sentido do eixo. PPP usa Base com -30 N e Tool com +30 N como exemplos, não recomendações universais. RG p.32 registra a exceção **Line → Y**; reduzir Simple a “sempre Z” perde essa informação.

## Frame

RG p.33 permite selecionar compliance e força/torque nos seis graus de liberdade. Para cada eixo selecionado, registre força e limite de velocidade. Os eixos de translação usam força; os rotacionais, torque.

**Exemplo trabalhado — morsa hidráulica, PPP 125–126:** uma peça segurada ligeiramente fora do centro pode deslocar o robô quando a morsa fecha. O exemplo torna Y complacente com referência de força 0 N, permitindo acomodação lateral. Isso não significa rigidez em Y nem garantia de força residual exatamente zero; as notas mencionam tolerâncias. A adequação ao processo e a escolha de eixo dependem da montagem.

### Rebarbação retangular por trechos

WB 7.3 pede:

1. Home e pré-posição.
2. Quatro nós Force Frame, um para cada aresta, com eixos/sinais apropriados.
3. Uma thread para atualizar a posição medida em variável.
4. Condições de término em X ou Y em laços dentro de cada trecho.
5. Retorno ao Home após conclusão.

O material não oferece um limiar universal para cada aresta, forças ou código de thread completo. Use a geometria real para definir os limites e evite transformar a sugestão em laço infinito sem progresso.

## Point

RG p.34: Y aponta do TCP à origem da feature selecionada, e a distância TCP–origem deve ser **pelo menos 10 mm**. X/Z dependem da orientação original da feature; o frame muda em execução conforme a posição do TCP.

Para o caso circular, coloque a Point no centro da peça. O eixo Y controla a componente radial; uma componente em X pode promover o deslocamento ao redor, conforme o exemplo. Confirme sentido e orientação antes de interpretar o sinal como horário/anti-horário.

**Exemplo trabalhado — WB 7.2:** peça alinhada com X/Y da Base, Start/Home e pré-posição; aguardar sinal digital; usar Point para contornar por **20 s**, com velocidade máxima **50 mm/s**; retirar para uma posição de afastamento, exemplificada por **+50 mm em Z da Base**, e voltar ao Home. São parâmetros do exercício, sem qualificação para qualquer ferramenta/peça.

Não use o centro como destino coincidente com o TCP para definir a direção radial: a distância mínima é parte explícita da descrição.

## Motion

RG p.35 define X como projeção da direção de movimento do TCP no plano XY da feature e Y perpendicular ao movimento nesse plano. X não é selecionável no assistente demonstrado. PPP 127 mostra pressão transversal mantendo o significado relativo ao avanço entre arestas.

Particularidade documentada: ao entrar parado, não há eixos complacentes até a velocidade do TCP ficar maior que zero. Se o robô voltar a parar já em force mode, o task frame mantém a orientação do último movimento. Não suponha que o frame Motion em repouso inicial esteja definido da mesma forma que durante o avanço.

## Exemplo trabalhado — localizar centro por quatro contatos

WB 7.4 pressupõe peça retangular alinhada a X/Y da Base, com posição variável e **sem necessidade de estimar rotação**:

1. Toque as quatro faces e identifique contato por um limiar de força definido para o ensaio.
2. Registre quatro posições de contato.
3. Calcule o centro.
4. Use o centro para a tarefa seguinte somente após conferir geometria e aproximação.

Reconstrução matemática para contatos opostos e geometria de ponta simétrica:

```text
xc = (x_esquerda + x_direita) / 2
yc = (y_frente + y_tras) / 2
```

Exemplo didático reconstruído: x=0,20 e 0,30 m; y=0,10 e 0,18 m → centro `(0,25; 0,14)` m. A simetria permite cancelar o offset da ponta em contatos opostos; assimetria, flexão e limiares distintos exigem avaliação. O WB pede calcular o centro, mas não fornece a fórmula, compensações completas ou um valor de força de detecção.

O procedimento não identifica orientação de uma peça girada; essa hipótese é explicitamente excluída pelo enunciado. Não extrapole o resultado de posição para localização completa em seis graus de liberdade.

## Antipadrões

- Confundir eixo selecionado a 0 N com eixo bloqueado.
- Escolher sinal sem conhecer a feature ativa.
- Copiar 30 N ou 50 mm/s como valor recomendado para qualquer aplicação.
- Omitir a exceção de Line no Simple.
- Usar Point abaixo da distância mínima descrita.
- Esperar que Motion parado na entrada já tenha o mesmo comportamento de frame móvel.
- Eliminar protective stops alterando limites sem investigar causa.
- Confundir ausência de parada com validação de segurança ou qualidade do processo.

## Exercícios e conferência

WB 7.1: explorar Simple/Frame/Point pelo Test e Freedrive, conforme ambiente de treinamento. WB 7.2: circular com Point. WB 7.3: retangular com quatro Frame. WB 7.4: centro por contato.

Confira frame, eixos, sinais, condição de saída, limites e estado de carga. O critério do curso de executar sem protective stop é um resultado didático, não certificação da célula. Não há demonstrações com pessoas ou execução de movimentos automatizadas por esta skill.

**Conexões:** ch03 frames e poses; ch04 Point/Line/Plane; ch05 TCP e carga.

## Integração Core — medição, thread e Event

**Fontes:** CRG p.45–48; CPP 83–86; CWB p.13. `force()` retorna, no guia, a norma das componentes de força externa X/Y/Z no TCP, em N. É escalar: não identifica sozinho sentido, eixo ou torque. Medir força não é o mesmo que comandar força com o wizard.

O exercício inicializa `f=0`, atualiza `f=force()` em thread e insere Wait de 0,01 s. CPP 84 recomenda uma espera ou `sync()` para ceder tempo de execução. Isso evita um laço que consome tempo sem ceder; não termina o laço nem garante amostragem total exata de 10 ms em qualquer implementação.

Thread executa em paralelo; Event executa segundo condição. CPP 83 descreve avaliação a cada ciclo de controle (2 ms no rascunho) e apenas um Event executando por vez; não tratar como especificação atual universal ou canal de segurança. A ação precisa ser projetada para interromper o processo de forma coerente, sem usar a leitura de software como função de segurança certificada.

**Exemplo trabalhado do Core:** CWB p.13 pede Simple no Tool com 10 N por 10 s; opcionalmente, Event interrompe a aplicação quando `force()` excede 9 N. CPP 86 usa demonstração de Halt acima de 10 N. Preserve os dois contextos: 9 N é limiar do opcional do workbook, 10 N é outro exemplo, não uma única regra contraditória a ser corrigida silenciosamente. Tempos/forças são didáticos. A demonstração manual e a balança do curso não foram executadas por esta skill.

Para estudar, explique o que o escalar informa, como uma espera afeta o monitoramento e o que acontece com o estado do programa quando o Event dispara. Para programa de contato real, são necessárias versão, condição de saída e geometria; o corpus não fornece um script completo e validado para isso.


## Complemento dos manuais — 2026-10-28

SM511 p.38–40,57,64 define força e wrench; get_tcp_force usa eixos da Base e não é pose geométrica. type=1 Point,2 Frame,3 Motion; Simple pertence à GUI. limits significa velocidade nos eixos complacentes e desvio nos demais. U5E p.207 mantém Simple em Y para feature linha. Não combinar Force com Conveyor/Path Offset (p.206). Ver ch27.


## Complemento do lote ampliado — 2026-10-28

N10p.111 oferecewrench_trans e p.113 atualiza sensorF/Texterno porRTDE; efeitos sobreforce/screwdriving/freedrive e tara/transição estão emch43. Alto torque de aparafusamento é tema próprioch51.
