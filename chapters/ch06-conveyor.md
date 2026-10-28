# 6 — Conveyor tracking

**Fontes:** PPP slides 102–115; RG p.29–31; WB p.25–27; AP p.199–212.

## Ideia central

O tracking adapta o movimento programado ao deslocamento medido da esteira. A posição ensinada deve ter relação definida com o sensor que inicia o acompanhamento; velocidade correta não compensa uma origem de detecção incoerente.

## Configuração e decisões

1. Habilite Conveyor Tracking na instalação.
2. Escolha encoder incremental ou absoluto.
3. Para incremental, escolha modo de contagem e entradas compatíveis.
4. Selecione tipo de esteira: linear ou circular.
5. Para linear, configure ticks por metro.
6. Crie uma feature Line alinhada ao avanço e selecione-a para direção.
7. Confira o sinal; o material apresenta Reverse direction.
8. Salve e confira configuração antes de integrar ao ciclo.

| Encoder/modo | Informação do curso |
|---|---|
| Incremental | Ligação a entradas digitais |
| Absoluto | Ligação por Modbus; procedimento completo não fornecido |
| Rise | Conta bordas de subida |
| Fall | Conta bordas de descida |
| Rise and fall | Conta ambas |
| Quadrature | Dois canais/entradas |

Não confunda o número de pulsos nominal do encoder com a escala efetiva após decodificação e transmissão mecânica. A necessidade de conferir essa cadeia deriva dos modos distintos do curso; o material não fornece todas as relações de transmissão para calcular a escala real.

### Divergência sobre entradas

PPP slide 108 e AP p.205 afirmam “digital_in[0]–[1]”. As notas do slide falam nas primeiras quatro entradas e decodificação a 40 kHz. Não forneça pinagem definitiva ou frequência garantida com base nessa contradição. Peça modelo de controlador/robô e versão do software para consultar o manual correto antes de orientar ligação. A apresentação apenas indica o conceito.

## Referência espacial e gatilho

RG p.31 e PPP 104 usam:

- WAIT_POS acima da região de detecção.
- PICK_POS ensinado com a peça na posição em que **acaba de acionar** a barreira óptica.
- Espera pelo sinal da barreira antes do bloco de tracking.
- MoveL e operações de pegar/depositar dentro do bloco.

Durante o tracking, os waypoints internos acompanham a direção e a velocidade da esteira. O waypoint ensinado não é uma referência arbitrária no meio do percurso: ele corresponde ao evento do sensor.

## Exemplo trabalhado — pegar da esteira

Reconstrução do fluxo de PPP 104 / WB 6.1:

```text
Mover para WAIT_POS
Aguardar evento da barreira associado à peça
Entrar em Conveyor Tracking
  MoveL para PICK_POS relativo à referência ensinada
  Acionar garra conforme mapeamento da instalação
  Concluir pega e retirar da região de contato
Sair do acompanhamento conforme fluxo do programa
Depositar no palete
Encerrar quando palete estiver completo
```

A apresentação usa DI0, DO7 e espera de 0,5 s; RG cita afastamento de 200 mm como exemplo. Esses números não são um mapeamento universal nem tempo de pega garantido. Interfaces da garra, atrasos e espaço disponível pertencem à aplicação.

### Conferência de escala reconstruída

Como checagem dimensional, se uma medição de avanço de 0,5 m acumular 1000 ticks, a escala efetiva é `1000/0,5 = 2000 ticks/m`. É exemplo reconstruído de calibração dimensional, não valor fornecido para o encoder do curso. Com a direção correta, deslocamento medido e deslocamento acompanhado devem ser coerentes. Se houver erro proporcional à distância, investigue escala; se o erro inicial for quase constante, investigue referência de ensino/gatilho. Essas são heurísticas de diagnóstico, não prova isolada de causa.

## Seleção de modo: paletizar ou despaletizar

WB p.25 define High = despaletizar para esteira; Low = esteira para palete. A origem do sinal deve ser nomeada.

| Exercício | Quando ler o seletor | Comportamento |
|---|---|---|
| 6.2 | BeforeStart, guardar em variável | Mudanças durante execução não alteram o modo |
| 6.3 | Na fronteira entre ciclos | Terminar peça atual e aplicar novo modo no próximo ciclo |

Capture uma decisão de modo por ciclo. Não interrompa uma peça no meio da transferência porque o seletor mudou. Preserve os contadores/posições do palete coerentes ao alternar; o exercício 6.3 exige voltar ao local correto, sem fornecer solução única pronta.

## Antipadrões e diagnóstico

- Ensinar PICK_POS distante da referência de acionamento do sensor.
- Trocar escala sem conferir modo de contagem.
- Inverter direção em software e também fisicamente sem verificar resultado.
- Reutilizar o mesmo nível alto do sensor como se toda leitura fosse nova peça.
- Executar movimentação que depende da esteira fora do bloco de tracking.
- Alterar modo durante transferência sem limite de ciclo.
- Declarar ligação/frequência garantida com base no slide contraditório.

## Exercícios e conferência

WB 6.1: pegar da esteira e encher palete, parar ao final. WB 6.2: adicionar fluxo inverso e seleção em BeforeStart. WB 6.3: aceitar mudança de modo após finalizar ciclo atual.

Confira encoder, escala, direção, origem no sensor, momento de captura do modo e posição no palete. O corpus não cobre fila de muitas peças, recuperação completa após perda de encoder ou implementação detalhada de tracking circular/Modbus.

**Conexões:** ch01 estrutura de estados; ch02 I/Os e variáveis; ch04 Line; ch05 carga no pegar/soltar.

## Integração Core — canais rápidos E-series

**Fontes:** CPP 14 e notas técnicas dos CPP 114–115. O Core menciona processador e terminais DI[8]–DI[11] dedicados à quadratura na E-series do rascunho, além de quatro entradas rápidas nos quadros do controlador. Isso amplia, mas não elimina, a divergência do Advanced que cita DI0–1/primeiras quatro.

Não misture numeração de famílias/controladores. A frequência “40 kHz?” aparece com interrogação nas notas do CPP 14 e não deve ser promovida a garantia. Para ligação real, identificar controlador, versão e esquema elétrico correto. Mantenha o procedimento de tracking do Advanced e a necessidade de escala/direção/gatilho coerentes. O buffer de paletização do Core (ch11) resolve ocupação de posições; não é acompanhamento de esteira.


## Complemento dos manuais — 2026-10-28

U5E p.238 e SM511 p.35 documentam DI8–11 e decodificação até 40 kHz no escopo e-Series. conveyor_pulse_decode legada é obsoleta; não misturar índices com encoder_enable_pulse_decode. O exemplo desta última tem aridade inconsistente no manual; ver ch28. Force/Path Offset/Conveyor não devem ser combinados no escopo U5E p.206 e SM511 p.46.


## Complemento do lote ampliado — 2026-10-28

Continuar identificando encoder/API e frame da captura;MotionVersion/OptiMove não tornamtracking automaticamente compatível com outrosmodos. ConsultarScriptDirectoryda versão emch42/44.
