# 1 — Estrutura de programas e configuração inicial

**Fontes:** PPP slides 4–17; RG p.2–9; WB p.4–10; AP p.4–28.

## Ideia central

Planeje o processo como uma sequência de estados e decisões antes de ensinar pontos. A estrutura deve permitir localizar uma falha pelo nome da operação e reconhecer as condições necessárias para continuar.

## Métodos e critérios

### Program Flow Chart

Use quando o ciclo contém decisões, equipamentos externos ou retrabalho. Registre início/término, operações, subprogramas, sinais, perguntas de decisão e destinos de cada ramo.

| Símbolo do curso | Papel |
|---|---|
| Retângulo arredondado | Início/término |
| Retângulo | Operação ou movimento |
| Retângulo com barras laterais | Subprograma |
| Losango | Decisão, If/Else ou Switch/Case |
| Paralelogramo | Entrada/saída |
| Seta | Ordem de execução |

O fluxograma expõe uma espera ou ramificação esquecida antes que fique escondida em uma longa árvore de movimentos. Também comunica a intenção a quem fará manutenção.

### Estrutura por função do processo

Agrupe pegar, carregar máquina, descarregar, transportar, inspecionar e destinar em pastas nomeadas. Use nomes que revelem objeto e papel: `Pegar_peca`, `Aproximar_CNC`, `Aguardar_maquina`, `Qualidade_OK`. São exemplos reconstruídos, não nomes obrigatórios.

Não aninhe movimentos dentro de movimentos sem necessidade. Não duplique uma sequência apenas porque recebe outro parâmetro: considere uma função (ch02). PPP slide 8 recomenda somente as threads necessárias e decisões organizadas por grupos. O exemplo de marcas/modelos ensina uma hierarquia de decisão; não demonstra formalmente busca binária ou uma complexidade algorítmica.

### Preparação da instalação

Antes do exercício WB 1.2:

1. Identifique ferramenta e configuração física.
2. Configure TCP em relação ao flange e sua orientação quando aplicável.
3. Configure payload e CoG da montagem real.
4. Nomeie sensores, entradas e saídas em I/O Setup.
5. Salve a instalação associada ao programa.
6. Implemente a sequência que já foi descrita no fluxograma.

RG p.2–5 usa mm nos campos de geometria/CoG e kg em payload. Isso não muda as unidades em metros dos comandos URScript do ch05.

## Exemplo trabalhado — CNC, esteira e inspeção

WB p.5 pede carregar uma CNC, levar a peça processada à esteira para secagem, recolhê-la na outra ponta e apresentar à câmera. Se não houver sinal alto de qualidade em até **3 segundos**, rejeite a peça. Se houver, encaminhe ao palete.

Reconstrução compacta da lógica:

```text
Iniciar com instalação e ferramenta corretas
  → pegar peça e carregar CNC
  → aguardar conclusão conforme interface da célula
  → descarregar e depositar na esteira
  → recolher na saída e apresentar à câmera
  → avaliar sinal de qualidade na janela de 3 s
      sinal recebido → paletizar
      sem sinal até o limite → descartar
  → encerrar/repetir conforme especificação
```

O enunciado não define todos os handshakes, endereços de I/O ou tempos de processo. Ao aplicar, obtenha os sinais e comportamentos de falha ausentes; não atribua DI0 ou DO7 apenas por aparecerem em outro exercício. A janela de 3 s é requisito desse exercício.

PPP slide 11 apresenta outro exemplo: abrir proteção de laser, carregar, fechar, iniciar, aguardar fim, retirar, inspecionar e separar. Ele ensina a explicitar a interação com a máquina; não fornece um projeto de intertravamento de segurança.

## Referência dos kits de treinamento

WB p.4 fornece valores para as montagens de bancada do curso, não fichas técnicas universais de produtos com esses nomes.

| Garra identificada | Massa (kg) | CX, CY (mm) | CZ (mm) |
|---|---:|---|---:|
| ROBOTIQ 2-Finger | 0,950 | 0, 0 | 45 |
| WEISS ROBOTICS grande | 1,100 | 0, 0 | 47 |
| WEISS ROBOTICS pequena | 0,450 | 0, 0 | 35 |
| ZIMMER GEH6060IL | 1,800 | 0, 0 | 32 |
| AFAG EU-20 | 0,425 | 0, 0 | 45 |
| Schunk Co-act EGP-C | 0,800 | 0, 0 | 40 |

Confira adaptadores e montagem antes de usar os valores. Pegar uma peça pode mudar massa e CoG.

## Antipadrões e diagnóstico

- **Waypoints sem nomes:** obrigam a descobrir fisicamente qual etapa representam.
- **I/Os só por número:** escondem a função e facilitam troca de sinais.
- **Muitas threads para detalhes simples:** aumentam concorrência sem melhorar a sequência principal.
- **Pular o fluxograma:** deixa esperas e decisões implícitas.
- **Ausência de qualidade tratada como aprovação:** contraria WB p.5.
- **Parâmetros de bancada copiados para outra ferramenta:** confundem exemplo com montagem efetiva.

## Exercícios e conferência

- **WB 1.1, p.7:** desenhar fluxograma.
- **WB 1.2, p.10:** implementar com instalação e nomes claros; arquivo proposto `Ex_1.2.urp`.
- **Conferência:** um colega localiza pela árvore a colocação na CNC? As duas saídas da inspeção estão definidas? A janela de 3 s está explícita? TCP, carga e CoG pertencem à instalação usada?

## Conexões

ch02: funções e parâmetros. ch03: aproximação calculada e retomada. ch05: carga dinâmica. ch06: esteira em movimento.

## Integração Core — primeiro pick-and-place

**Fontes:** CWB p.2; CRG p.5–19; CPP 10–16. Comece conferindo TCP/carga/CoG, sensores nomeados e seus estados no I/O Tab. O Core manda comparar dados da garra com medições e conferir TCP por rotação em torno da referência. Waypoint define destino; tipo de Move define percurso (ch09).

Use Wait para tempo, sinal ou condição; Set para comando de saída/estado de carga no ponto apropriado da sequência. Dar comando de garra não é a mesma coisa que confirmar peça agarrada: a confirmação depende da interface disponível. Os opcionais de Testbox são exercícios de sinais, não mapeamento universal de instalação. Para limites e interação humana, leia ch08; para BeforeStart, ch10; para inspeção por contagem, ch12.

CPP 15 acrescenta opções de controle manual de saídas e estados vinculados à execução. Portanto, além de nomear um sinal, confira sua função configurada e quem pode comandá-lo. Não presuma que uma saída permanece no mesmo estado após parada apenas por ter sido setada durante o programa.
