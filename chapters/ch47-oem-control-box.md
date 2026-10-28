# 47 — OEM Control Box: variantes, alimentação e integração

**Fonte:** N16Installation GuideOEM50p, incluindoAC/DC eACFlexTail. Referências físicas p.8,16–35,39. Complementa desenhoOEMDWG antigo; não presume mesmas dimensões/revisão.

## Identificar variante antes da ligação

DistinguirAC,DC eACFlexTail, geração da caixa e modelo do braço. A ediçãoN16p.35 informaOEM451x150x168 mm eFlexTail475x150x168 mm; o desenho2019/ch22 pode ter outro envelope. Não escolher pelo nomeOEM sem revisão/partnumber.

AC requer terminais de linha/neutro/PE correspondentes;DC exige polaridade e fonte capaz de suprir partida. P.39 alerta dano permanente por inversãoDC. Não inferir capacidade da fonte pela potência média do ciclo; corrente de partida, fusível, condutor, queda de tensão e aterramento precisam da tabela/projeto aplicável. Não conectar polos sem marcação nem usar diagramaAC paraDC.

P.35 informa fonteI/O24V até2A no contexto da caixa; não equivale à capacidade da alimentação principal ou da ferramenta. Conservar separação entrePE, retornoGND e blindagem.

## Térmica e montagem

N16 p.27–28 pede proteção contra poeira/água/solventes/químicos, ar de entrada0–50°C, frame de alumínio0–65°C e recomenda pelo menos50 mm livres em entradas/saídas de ar. Evitar recirculação quente e considerar dissipação no invólucro final. Instalar em gabinete não transfere automaticamenteIP ou certificação do conjunto.

Superfície condutiva de montagem também precisa da ligação de proteção descrita. Dimensões/furação precisam de vista da variante exata; texto linear não substitui desenho cotado. Cabos exigem espaço e alívio de tensão.

## Periféricos e operação sem pendant

N16 p.29–34 diferenciaEthernet,USB,SD,MiniDP eTeachPendant. MiniDP suportaDP ou adaptadores ativosDVI/HDMI; não presumir adaptador passivo. Não retirarSD em operação ou conectar/desconectar pendant energizado.

O guia descreve habilitador3PE externo/pendant eEmergencyStop externo quando pendant não conectado. UR20/UR30 não suportam o pendant padrão no fluxo descrito. Remover interface local não remove exigências de habilitação e emergência. Local/Remote é seleção de origem de comando, não circuito de segurança.

## Entregável

Diagrama por variante, origem/fonte e orçamento elétrico,PE/EMC, dissipação/folgas, fixação, dispositivos externos de segurança e critérios de ensaio. A integração completa e conformidade da célula permanecem além da simples instalação da caixa. Não energizar nem alterar hardware ao consultar a skill.

**Exercício:** explicar por que uma fonteDC que sustenta consumo médio pode falhar na partida e por que a versãoFlexTail muda o desenho sem mudar necessariamente a função.
