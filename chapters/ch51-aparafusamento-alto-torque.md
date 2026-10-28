# 51 — Aparafusamento de alto torque e reação mecânica

**Fonte:** N13High Torque Screwdriving32p, especialmente9–16,18–28. Complementa N10/N26screw_driving e o treinamentobásico. Torque de aperto do produto não é torque de montagem da junta do robô.

## Avaliar reação, não sóNm da ferramenta

N13 p.9 relaciona capacidade a montagem da ferramenta, pose, carga, programação, limites e estratégia da parafusadeira. As faixas<30,30–60,60–100 e>100 Nm descrevem complexidade de aplicaçõesUR10/UR10e naquele guia; não são capacidade garantida de qualquerUR ou permissão para ajustar limites até atingir torque.

O torque disponível de cada junta já é parcialmente usado para sustentar braço/carga. Braço esticado,CoG distante ou reação concentrada num eixo podem consumir a margem. Evitar eixo de aperto paralelo/próximo do eixo de uma única junta quando isso concentra desvio. Confirme também força axial necessária para acompanhar parafuso.

## Braço de alavanca no plano correto

N13 p.10–12 usaM=F*d, comd perpendicular ao eixo de aperto. A distância ao longo desse eixo não aumenta alavanca útil. Exemplo:30 Nm/0,3329m≈90,12N. A conta é física elementar, não prova que a pose/carga sustenta esse processo.

Aumentard reduz força de reação transferida, mas pode prejudicar a força axial disponível e aumentarCoG/inércia. Ferramenta mais longa não é ganho sem custo. A tabela de combinações força/torque serve para relação entre grandezas; o guia afirma que não garante capacidade do robô.

## Steady versusFast-Steady

N13 p.14: steady ocorre após500 ms parado com velocidade alvozero;is_steady consulta estado, não o ativa. O braço deve permanecer sem comando de movimento durante aperto nesse modo. Para seguir avanço do parafuso, pode ser necessária compensação mecânica.

P.15–16: funçãoScrewdriving come-Series≥5.4, indisponível emCB3 segundo o guia. Fast-Steady é associado à opçãoFollowtheScrewusingForce ouscrew_driving, detectando reação e sustentando posição. Não atribuir esse benefício a qualquer seleçãoGUI apenas por nomeScrewdriving. TCP+Z deve apontar para o furo conforme a instalação.

Programar aproximação → confirmação de contato/processo → aperto com estratégia definida → respostaOK/NOK/timeout → saída do modo → retirada. Uma resposta da parafusadeira precisa corresponder à peça/ciclo, não ficarlatched de aperto anterior. end_screw_driving é saída de processo; não função de emergência.

## Limites e alternativas

N13 p.18 vincula limites permitidos à avaliação de risco; não recomendarpreset menos restritivo como ajuste automático. ToolPosition deve representar ponto de contato real. Não confundir valores máximos de catálogo de junta com torque externamente aplicável em qualquer pose.

P.23–27 discute ferramenta de pulsos, estratégia do controlador, braço de reação e dispositivo que apoia na peça. Considerar precisão, vibração, acesso e flexibilidade. Exemplos de marcas/ensaios são históricos, não recomendações atuais de compra.

## Evidência de processo

Registrar configuração, pose/carga, torque/ângulo da ferramenta, respostaOK/NOK, alvos/medidas do robô e momentos da sequência. N13 p.28 recomendaRTDE para coleta; identificar janela de aperto e reação, distinguindo torque de junta, corrente e torque do parafuso. Desvio recorrente/parada protetiva não é normal para alcançar meta.

**Exercício:** comparar30 Nm com alavancas0,1m e0,4m:300N versus75N, depois listar limitações da conta antes de selecionar ferramenta ou limite.
