# 9 — Operação básica, movimentos e otimização

**Fontes Core:** CRG p.5–19 e 29–33, 55; CPP slides 10–16 e 37–47; CWB p.2 e 6.

## Ideia central

O waypoint define o destino; o tipo de movimento define como o TCP chega até ele. Otimize o ciclo medindo resultados, preservando os pontos que exigem contato/precisão e os limites definidos para a aplicação.

## Operação e coordenadas

CRG p.5 descreve estados Power Off → Idle → Normal: energizar juntas e liberar freios são etapas distintas. Os rótulos dos botões variam entre as notas e o guia. CPP 10 menciona verificar carga antes da inicialização e que a liberação pode envolver pequeno movimento de junta.

| Ferramenta | Papel |
|---|---|
| Move Joint | Ajustar uma junta e observar ângulo/limites |
| Move Tool | Deslocar TCP segundo eixos da feature selecionada |
| Freedrive | Ensino manual de posições no estado apropriado |
| Pose Editor | Ajustar coordenadas/orientação e posições de junta |
| I/O Tab | Monitorar sinais e, quando permitido, comandar saídas |

Backdrive (CRG p.7) é descrito como liberação seletiva de freio em estado anterior à inicialização completa; não é simplesmente outro nome para Freedrive normal.

CPP 11 inclui Base, Tool, View e features próprias no jogging. View depende da orientação da visualização; não confunda esse auxílio de jog com frame físico fixo da peça.

CPP 12 mostra mm na posição cartesiana da GUI e opções de representação angular: vetor de rotação/RPY em graus/radianos. Isso não muda o formato em metros/radianos de uma pose URScript (ch03). CPP 44 explica que Edit Pose permite salvar um ponto sem mover fisicamente até ele; outras ações da interface podem comandar movimento. Diferencie editar, confirmar e mover na versão usada.

## Escolher movimento pelo requisito

| Tipo | Caminho/controlado | Uso do curso | Unidade na GUI descrita |
|---|---|---|---|
| MoveJ | Movimento em espaço de juntas; TCP não linear | Transferência em espaço livre quando caminho do TCP não é requisito | °/s e °/s² |
| MoveL | TCP em linha entre pontos | Aproximação/recuo linear e caminho restrito | mm/s e mm/s² |
| MoveP | Processo com velocidade de TCP constante e blends | Cola/dispensação contínua | Parâmetros de ferramenta/processo |
| Circle Move | Arco a partir de ponto inicial, via e final | Movimento circular na estrutura MoveP do curso | Conforme nó de processo |

Não afirme que MoveJ é sempre o menor tempo para qualquer geometria. O guia o apresenta como opção rápida em espaço livre. A viabilidade da trajetória depende da configuração e do alcance.

CRG p.55: o ponto anterior é o início do arco; os dois pontos do Circle Move são via e final. As notas CPP 42 diferenciam modos de orientação Fixed/Unconstrained no exemplo. Não invente círculo completo a partir de dois pontos sem início definido nem extrapole a organização MoveC/MoveP para qualquer versão.

## Blending: suavidade versus passagem exata

CRG p.31 / CPP 39–40: blend permite contornar o waypoint sem parar nele. Raio maior suaviza a curva e pode desviar mais do ponto nominal. Não aplique por rotina a um ponto de pega, encaixe ou operação que requer passagem/parada exata.

O curso alerta para blends sobrepostos; além de raio, velocidade inicial/final, tipo de movimento e tempo influenciam a transição. A apresentação diz que a GUI limita raio, mas isso não comprova por si só a validade de toda combinação. Confira os avisos e a trajetória da versão.

CPP 42 informa que espera por operador ou I/O durante o trecho de MoveP a velocidade constante pode interromper o movimento ou causar parada. Posicione as esperas de sincronização onde a sequência pode parar, fora do trecho contínuo pretendido.

## Parâmetros compartilhados e pontos vinculados

- Velocidade/aceleração no Move aplicam-se aos pontos que usam seus parâmetros compartilhados.
- Um waypoint pode ter ajustes próprios ou tempo especificado; tempo e velocidade não devem ser interpretados como promessas de execução independente dos limites.
- Linked waypoints compartilham posição: alterar um muda os vinculados. Blend, velocidade e aceleração permanecem parâmetros locais no exemplo CPP 44.
- Aceleração muito alta pode produzir movimento abrupto sem ganho proporcional no tempo (CPP 43).

## Exemplo trabalhado — reduzir tempo de pick-and-place

CWB p.6 pede medir ciclo inicial, revisar tipos, blends e velocidade/aceleração, medir novamente e calcular melhora. Reconstrução de ensaio:

```text
Ciclo inicial: 12 s
Ciclo revisado: 10 s
Redução absoluta: 2 s
Redução percentual: (12 - 10) / 12 × 100 ≈ 16,7%
Taxa teórica sem perdas: 60/12 = 5 → 60/10 = 6 ciclos/min
```

Os tempos são exemplo reconstruído, não resultado do curso. Compare a mesma tarefa e estado de carga; registre esperas, aproximações e mudanças individuais. A taxa teórica não considera abastecimento, falhas, manutenção ou qualidade. Não remova limites de segurança para obter o resultado.

Na revisão, primeiro pergunte quais pontos exigem trajetória/posição exata, quais transferências têm espaço livre e quais esperas podem ser ajustadas com informação efetiva de garra/sensor. Só então escolha o parâmetro a alterar.

## Antipadrões e conferência

- Substituir aproximação linear por caminho não controlado apenas para ganhar tempo.
- Aumentar todos os parâmetros simultaneamente e perder a relação causa/efeito.
- Aplicar blend no ponto que precisa ser alcançado exatamente.
- Esperar confirmação humana no meio de processo contínuo.
- Editar waypoint vinculado sem revisar suas outras ocorrências.
- Ler mm/° da GUI como unidades de URScript.

**Exercícios:** CWB módulo 1 para operação e módulo 3 para otimização. **Conexões:** ch03 unidades; ch05 TCP/carga; ch08 restrições; ch12 seleção de processos.


## Complemento dos manuais — 2026-10-28

SM511 p.43–50 documenta unidades e precedência de t sobre a/v; movec mode=1 fixa orientação relativamente à tangente, não ao mundo. Blend/acelerações sugeridos em IG não são limites universais. Servoj não herda t=2 ms automaticamente do ciclo de controle. Ver ch21 e ch27.


## Complemento do lote ampliado — 2026-10-28

N10/N26p.38–39 introduzemMotionVersion2 e disponibilidade por plataforma;OptiMovea/v são frações, não unidades físicas deMoveJ/L. Regras antigas deblend/skipping devem ser qualificadas por versão. Verch42.
