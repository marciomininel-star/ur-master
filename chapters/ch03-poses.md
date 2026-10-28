# 3 — Poses, orientação e transformações

**Fontes:** PPP slides 40–62; RG p.17–22; WB p.15–20; AP p.123–153.

## Ideia central

Uma pose só é interpretável com seu referencial. Defina onde as coordenadas são expressas antes de escolher unidades, representação angular e operação geométrica.

## Estrutura e unidades

```urscript
pose_var = p[x, y, z, rx, ry, rz]
```

| Índices | Conteúdo | Unidade |
|---|---|---|
| 0, 1, 2 | x, y, z | metro |
| 3, 4, 5 | rx, ry, rz: vetor de rotação | radiano |

O vetor de rotação não contém três ângulos RPY independentes. PPP 48 explica que sua direção representa o eixo de rotação e sua magnitude representa o ângulo. Não some componentes de vetor de rotação como translações para obter uma orientação geral.

### Conversões documentadas

```urscript
angulo = d2r(45)
graus = r2d(angulo)
rot = rpy2rotvec([d2r(180), d2r(45), d2r(10)])
rpy = rotvec2rpy(rot)
```

`d2r(45)` ≈ 0,785398. RG p.21 usa entrada arredondada 0,78540 e mostra aproximadamente 45,00011 graus. A diferença é arredondamento.

## Escolha da operação

| Operação | Uso no curso | Limite |
|---|---|---|
| `get_actual_tcp_pose()` | Pose medida na Base | Não gera trajetória livre de obstáculos |
| `pose_add(a,b)` | Exercício com câmera alinhada à Base | Não é transformação geral de frame girado |
| `pose_trans(frame, local)` | Transformar pose local por frame conhecido | Ordem dos argumentos importa |
| `pose_inv(pose)` | Inversa, em exemplo simples | Não equivale em geral a negar seis números |
| `pose_sub(a,b)` | Exercício de subtração | Desenho translacional não especifica composição angular |
| `pose_dist(a,b)` | Exercício e slide 52 | Título do slide diz `point_dist`; semântica completa não confirmada |

A notação `T_base_alvo = T_base_frame × T_frame_alvo` ajuda a raciocinar sobre a composição ensinada; é notação matemática, não nova API.

## Exemplo trabalhado — câmera simulada

WB 3.1–3.2 e PPP 56–58 usam três entradas do operador: x/y em mm e rz em graus. A câmera do primeiro exercício está paralela à Base. O segundo gira seu frame e usa transformação.

Reconstrução, sem comando de movimento:

```urscript
def alvo_da_camera(frame_camera, x_mm, y_mm, yaw_graus):
  local = p[x_mm/1000.0, y_mm/1000.0, 0, 0, 0, d2r(yaw_graus)]
  return pose_trans(frame_camera, local)
end
```

Os nomes são próprios desta reconstrução. Z=0 pressupõe peça no plano da câmera do exercício. A pose final real pode exigir altura e orientação adicionais da ferramenta; essa função isolada não é um pick-and-place pronto.

**Conferência geométrica reconstruída:** frame com origem na Base `(0,5; 0,2; 0,1)` m e giro +90° em Z. Offset local `(0,1; 0; 0)` m produz posição `(0,5; 0,3; 0,1)` m na Base. Somar apenas x na Base produz `(0,6; 0,2; 0,1)`, outra direção. Esse caso explica a troca para `pose_trans` no exercício 3.2.

### Método para estudar/aplicar

1. Ensine frame da câmera e registre origem/eixos.
2. Receba x, y, rz e converta unidades uma única vez.
3. Construa a pose local.
4. Transforme para Base.
5. Calcule pré-posição no referencial pretendido.
6. Confira posição, orientação e aproximação antes de usar movimento.

WB 3.1 acrescenta pegar, depositar na esteira, recolher e inspecionar com a aplicação do ch01.

## Exemplo trabalhado — retomada

PPP 50–51 e WB 3.3 leem a pose atual e substituem Z para criar afastamento antes do ciclo:

```urscript
act_pos = get_actual_tcp_pose()
act_pos[2] = 0.4
```

No curso, um MoveL leva a essa variável. **0,4 m é exemplo do slide**, não altura segura universal. A operação preserva x/y e orientação e altera Z da Base. Só é apropriada quando esse caminho estiver desobstruído, dentro do alcance e compatível com ferramenta/peça. Não é função de segurança certificada.

WB 3.4 pede alturas de pré-posições fornecidas em mm pelo operador: converta a metros e documente se a direção é Z da Base ou normal local. WB 3.5 pede alturas por região x/y: desenhe condições mutuamente compreensíveis e trate regiões não cobertas. Os valores 200/300/400 mm pertencem aos exemplos.

## Antipadrões e sinais de falha

- Erro por fator 1000: investigar mistura mm/m.
- Orientação inesperada: investigar graus/radianos e RPY/vetor de rotação.
- Offset na direção errada: verificar frame e ordem de `pose_trans`.
- Funciona alinhado, falha ao girar câmera: investigar soma usada como transformação.
- Retomada atravessa peças: caminho a partir do estado atual não foi considerado.
- Negar ou somar todos os componentes: desenhos simples não justificam regra geral de orientação.

## Exercícios e conferência

WB 3.1: câmera paralela; 3.2: câmera girada; 3.3: retomada; 3.4: alturas por operador; 3.5: alturas por região. Pergunte para cada solução: qual frame? qual unidade? qual direção de offset? qual caminho de aproximação? Essas respostas avaliam compreensão e não substituem validação no sistema.

**Conexões:** ch04 para features; ch05 para TCP; ch07 para frame de força.

## Integração Core — representação na GUI e waypoint relativo

**Fontes:** CPP 5–6, 11–12, 44, 51; CRG p.14–15, 35. As coordenadas seguem o sistema cartesiano de mão direita; cores RGB correspondem a X/Y/Z no recurso didático. Na GUI, o Pose Editor oferece vetor de rotação ou RPY em graus/radianos e posição em mm. Isso não modifica a representação de uma pose URScript: m e vetor de rotação em radianos.

View é opção de jogging relacionada à vista gráfica, não a localização física de uma câmera calibrada. Edit Pose para salvar um waypoint e comandos para mover até ele são operações distintas na apresentação (CPP 44); confira o comando concreto antes de afirmar que uma edição apenas salva ou movimenta.

Waypoint relativo guarda deslocamento From→To e o aplica a partir da posição atual no momento da execução; não é o mesmo que waypoint fixo relativo a uma feature. Repetir o deslocamento pode acumular movimento. Veja ch10 para primeira posição relativa/variável e sua relação com Auto-Move; ch09 para unidades de velocidade na GUI.

## Integração Industrial/Interfaces — coordenadas recebidas

**Fontes:** ICWB p.3–5, 8–9; ICP p.18–20, 45–46; IFP p.37–38, 55–56. O canal de comunicação não determina unidade/frame: Modbus do exercício usa décimos de mm, socket usa tupla ASCII e RTDE monta pose de seis componentes. Converta conforme contrato, não conforme aparência do número.

Leitura ASCII de três floats retorna primeiro a quantidade; X começa no índice 1 (ch16). Alguns exemplos usam X/Y/Z e outros X/Y/Rz: validar o significado do terceiro campo. A pose RTDE do exemplo usa m/rad e não seis ângulos de juntas (ch19). Não usar o nome `get_current_tcp_pose` do IFG p.19 como API confirmada: fontes anteriores usam `get_actual_tcp_pose`; verificar a versão antes de gerar movimento.


## Complemento dos manuais — 2026-10-28

SM511 p.85–90 resolve as distâncias: point_dist ignora orientação; pose_dist a considera, sem fórmula completa no trecho. pose_add soma posições e compõe rotações R1*R2, não componentes angulares. Composição local segue pose_trans. Detalhes e limites de cinemática em ch27.


## Complemento CB3 e artigos de referência — 2026-10-28

SM341 p.35–36 confirma point_dist sem rotação e soma de posições/produto de rotações em pose_add; não define completamente pose_dist. EX inclui poses malformadas. Ver ch30 antes de transcrever.


## Complemento do lote ampliado — 2026-10-28

As novasAPIs aceitam algunsgoals comframe/nome doworldmodel, sem transformar uma string arbitrária emframe válido. wrench_trans é específico para força/torque, não pose_trans; verN10p.111/ch43.
