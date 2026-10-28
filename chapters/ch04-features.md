# 4 — Features e reutilização de trajetórias

**Fontes:** PPP slides 64–85; RG p.23–24; WB p.21–23; AP p.155–179.

## Ideia central

Ensine a forma da trajetória no frame da peça e altere a localização desse frame para repetir o processo. A geometria do processo não precisa ser ensinada em cada posição do palete.

## Seleção de feature

Uma feature é um sistema com origem e eixos X/Y/Z, representado no curso como pose. Isso não torna todos os modos de waypoint equivalentes.

| Feature | Construção descrita | Aplicação |
|---|---|---|
| Base | Referência na base do robô | Poses globais e leitura do TCP |
| Tool/TCP | Referências associadas à ferramenta | Direções de processo |
| Point | Um ponto e orientação | Referência local; centro no Force Point |
| Line | Dois pontos, Y de ponto 1 a ponto 2 no curso | Direção de esteira |
| Plane | Três pontos | Superfície e visão |

Os slides distinguem origem no flange e origem no TCP. Verifique TCP ativo e significado da tela antes de aplicar a nomenclatura antiga Tool/TCP.

### Divergência no plano

PPP 73 e AP p.167 dizem “Point_2 → Y-Axis”. As notas do mesmo slide e RG p.23 descrevem ponto 1 como origem, ponto 2 como X e ponto 3 como Y. O RG menciona software 3.6.

1. Identifique versão e sequência do assistente visível.
2. Ensine os pontos conforme essa versão.
3. Verifique origem e sinais dos eixos.
4. Confira coordenadas no Move Tab antes de atribuir offsets.

Não escolha uma convenção contraditória como universal.

## Programar relativo à variável

1. Crie a feature na instalação.
2. Habilite Variable quando presente na versão.
3. Insira MoveL/MoveP e selecione a variável, como `Plane_1_var`.
4. Ensine os waypoints dentro desse movimento.
5. Altere a variável entre execuções para mover a trajetória.

PPP 75 também relaciona MoveC. RG p.24 destaca MoveL/MoveP e afirma que MoveJ não pode ser associado à feature nessa interface. Não conclua daí que toda API/versão impossibilita qualquer uso cartesiano envolvendo MoveJ.

## Decisão sobre offsets

| Intenção | Método do curso | Direção da translação |
|---|---|---|
| Ajustar x/y/z de pose na Base | Indexação | Base |
| Somar translação como nos exemplos | `pose_add(feature, offset)` | Base no caso demonstrado |
| Deslocar nos eixos locais | `pose_trans(feature, offset_local)` | Feature local |
| Reusar em outro dispositivo | Atribuir outra pose à feature variável | Novo frame completo |

Mantenha o frame estável durante a passagem do processo. Essa recomendação deriva dos exercícios organizados por ciclo; fontes não ensinam a troca em meio a contato como transição controlada.

## Exemplo trabalhado — dois logotipos

WB 4.1 pede ensinar `UR_1`/`UR_2`, criar `UR_1_var`, programar o logo uma vez relativo à variável e alternar:

```text
Ciclo 1: UR_1_var recebe referência original UR_1 → executar logo
Ciclo 2: UR_1_var recebe UR_2 → executar o mesmo logo
Ciclo 3: restaurar referência original UR_1 → executar logo
```

Preserve a referência original separada da variável sobrescrita. Use os nomes efetivos de constantes/variáveis da versão; não presuma um sufixo automático.

## Exemplo trabalhado — palete e camadas

WB 4.2 desloca a feature com `pose_trans` para três peças e depois restaura. Reconstrução algorítmica:

```text
Preservar origem
Para cada posição da fileira:
  calcular offset local para essa posição
  compor origem e offset
  atribuir resultado ao frame do processo
  executar trajetória ensinada
Restaurar origem ao terminar
```

É pseudocódigo, não sintaxe URScript de laço `for`; o curso apresenta `while`. Calcular a partir da origem é uma alternativa reconstruída para conferir índices e evitar que o lote seguinte herde o último deslocamento. O WB usa deslocamentos sucessivos e reset.

WB 4.3 combina duas fileiras e três peças: seis posições. WB 4.4 acrescenta três níveis. Separe posição na fileira, fileira e nível. Espaçamentos não foram recuperados como medidas completas no texto; obtenha-os do dispositivo ou desenho antes de gerar coordenadas.

## Por que funciona e como falha

O movimento relativo preserva a forma local quando o frame muda. Reduz ensinamento repetido e concentra a localização da peça em uma referência. Falha se a peça muda de forma, se eixos são trocados no ensino ou se transições entre posições não têm retirada definida.

### Antipadrões

- Ensinar manualmente cada cópia da mesma trajetória.
- Aplicar offset na Base esperando direção de plano girado.
- Sobrescrever a única referência original.
- Esquecer reset após último item/camada.
- Transitar com ferramenta em contato sem retirada.
- Usar rascunho como definição inequívoca do assistente atual.

## Exercícios e conferência

1. WB 4.1: mesma forma em dois frames, com orientação consistente.
2. WB 4.2: três posições e reset; girar feature para conferir eixo do avanço.
3. WB 4.3: transição de fileira e contagem de seis peças.
4. WB 4.4: início/fim de três níveis e retorno à primeira posição.

O curso pede aplicação de cola em palete/caixa; não fornece vazão, cura ou parâmetros de adesivo.

**Conexões:** ch03 para composição; ch05 para dois bicos; ch06 para feature Line.

## Integração Core — plano e propriedades E-series

**Fontes:** CRG p.57–58; CPP 104–107; CWB p.17. O Core explicita ponto 1=origem, ponto 2=+X e ponto 3=+Y no assistente descrito, reforçando a sequência textual do RG Advanced. A figura do guia é esquemática e os slides Advanced ainda contêm outra descrição; confirme a interface/versão em vez de deduzir eixo só do desenho.

CPP 105–106 informa que, na E-series apresentada, Show axes, Joggable e Variable ficam automaticamente habilitados, enquanto antes eram opções. Assim, a ausência de checkbox Variable não prova que a feature não possa ser usada como variável. O sufixo/nome disponível também precisa ser conferido na instalação efetiva.

CWB p.17 propõe ensinar o logo relativo a um plano, mover a peça e reensinar o plano, preservando o processo. Reavalie aproximação, obstáculos e cópias de referências de segurança: CPP 27–28 informa que cópias para Safety podem ficar fora de sincronia. Pallet Template para colocar/recolher peças está no ch11; ele é distinto do palete de trajetórias do Advanced.
