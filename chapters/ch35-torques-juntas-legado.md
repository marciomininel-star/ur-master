# 35 — Torques de aperto por conexão: UR3/UR5/UR10 legados

**Fonte:** TQ p.1–2, artigo17259, sem revisão/data formal identificada. A tabela nomeia UR3/UR5/UR10, não e-Series; não aplicar a UR3e/5e/10e por semelhança. P.3 sem texto útil. Ambas as páginas de valores foram conferidas visualmente.

## Selecionar pelo par de componentes

Torque pertence à conexão e ao fixador especificados, não apenas ao número da junta. “Base mounting bracket/J0” da tabela não é automaticamente o parafuso que fixa o robô ao piso. “Tool/Gripper M6” não se generaliza a qualquer ferramenta/parafusoM6, material ou engajamento de rosca.

| Grupo de conexão descrito | UR3 | UR5 | UR10 | Página |
|---|---:|---:|---:|---|
| Bracket/J0, J0/J1 e J1/upper arm | 3,0Nm | 3,0Nm | 8,0Nm | 1–2 |
| Upper arm/J2 e conexão após cotovelo | 1,3Nm | 3,0Nm | 3,0Nm, com Elbow Counterpart | 1–2 |
| Trechos inferiores até wrist3/tool bracket | 1,3Nm | 1,3Nm | 1,3Nm | 1–2 |
| Tool/GripperM6 | 10Nm | 9Nm | 9Nm | 1–2 |
| Alignment screw | 0,4Nm | 0,4Nm | 0,4Nm | 1–2 |
| Blue lids | 0,4Nm | 0,4Nm | 0,4Nm | 1–2 |

Esta comparação sintetiza grupos; para aplicação, localizar a linha exata no original e conferir fixador/revisão. UR10 diferencia Elbow/Elbow Counterpart e Counterpart/Lower arm. Não omitir essa geometria ao selecionar instrução de serviço.

Cabeças/ferramentas também diferem: UR3 usa TorxT20/T10 nas conexões listadas e T8 em tampas de punho; UR5/UR10 usam medidas hexagonais específicas conforme linha. Tipo e tamanho da cabeça não determinam sozinhos torque; chave que encaixa não comprova conexão correta.

## Método de seleção de especificação

Registrar modelo/serial/revisão, par de peças, posição, quantidade e identificação do fixador. Confrontar TQ com manual de serviço aplicável. Se divergirem, investigar revisão, conexão, material, peça e condição de montagem antes de apertar. Não calcular média entre documentos.

Usar instrumento adequado/calibrado e procedimento de serviço quanto a sequência, substituição, limpeza e trava de rosca. A tabela não detalha todos esses fatores; não inventar condição seca/lubrificada, reutilização ou tolerância ausente. Inspeção pode detectar necessidade de serviço sem autorizar desmontagem.

## Relação com manutenção já incorporada

SVC4 p.15 e MAINT citam0,4Nm nas tampas de e-Series, mas a coincidência desse número não transfere o restante da tabela legada para e-Series. MAINT cita tolerância para seu item; TQ não informa tolerância geral para todas as conexões. Valores de montagem da base em U5E tratam conexão diferente das junções internas.

**Anti-padrões:** aplicar10Nm a todoM6; escolher apenas pelo modelo sem identificar par de peças; usar torque de tampa em flange; interpretar como pedido para apertar juntas do robô.

**Exercício:** pedido “qual torque do UR5?”. Resultado esperado: especificar conexão e revisão primeiro, depois citar linha/fonte; explicitar se ferramentaM6, junta interna ou fixação externa da base.


## Complemento do lote ampliado — 2026-10-28

NovosService/JointReplacementManuais têm torques por família,posição,tipo de parafuso eclamp. Coincidência numérica não transfere aplicabilidade. N09p.30 diferencia gruposURSeries. Verch39/53.
