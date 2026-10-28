# 43 — Dinâmica, torque direto, carga e wrench nas versões5.25/5.26

**Fontes:** N10/N26 p.41,70,73–78,82,86–87,111–114. Capacidades avançadas não são equivalentes a funções de segurança.

## direct_torque exige contrato por edição

N26 p.41: direct_torque(torques,friction_comp=True). N10 p.41: direct_torque(torque,viscous_scale=...,coulomb_scale=...).5.26 introduz escalas por junta e mantémfriction_comp como legado, ignorado quando novas escalas são fornecidas. Não passarFalse como segundo argumento posicional e assumir o mesmo efeito entre versões.

Ambas descrevem vetor6 emNm, comando a500 Hz por passo e compensação de gravidade interna: não somar gravidade novamente ao torque pedido. Sem chamada no passo, volta ao controle de posição; isso não significa parada física automática. O programa deve definir transição/saída. É controle baixo nível que contorna recursos de compliance; não substituir force_mode ou demonstrar com torquezero em hardware sem projeto.

## Medidas não são intercambiáveis

get_joint_torques descreve torque corrigido pelas parcelas necessárias para mover o próprio robô; não equivale a corrente bruta ou torquímetro do parafuso. get_mass_matrix, get_coriolis_and_centrifugal_torques, Jacobian e derivada usam parâmetros/frame próprios. Em torque direto, target acceleration não é aceleração comandada útil; a documentação recomenda distinguir medição de alvo.

get_steptime fornece período real. Mesmo uma medição a cada passo não elimina atraso de transporte externo. Modelos de dinâmica e log servem para análise, não para inferir limite seguro de contato humano.

## Atualização de payload e tara

N10 p.86–87 acrescenta transition_time a set_target_payload. Transição nãozero evita salto abrupto, mas programa continua durante o ajuste; o sensorF/T recebe reset final no fim da transição. Aceleração nesse instante pode contaminar medição. Planejar transição/estabilização e tara antes do uso de força, conforme procedimento; não zerar sensor com força de contato que se deseja medir.

estimate_payload p.70 exige pelo menos4poses, orientações diversas e wrench correspondente a cada pose, no ponto/orientação especificados. Retorna struct massa/CoG; não mede tensor completo por inferência. O pseudocódigo da fonte usa setters legados apesar da recomendação de API nova: não copiá-lo sem revisar.

## Frames e sensor externo

wrench_trans p.111 muda o ponto de vista do wrench, incluindo efeito da translação no torque; não usar pose_trans em forças como se fossem posição. Declarar ponto de aplicação, frame, ordemFx/Fy/Fz/Mx/My/Mz eN/Nm.

ft_rtde_input_enable p.113 substitui enable_external_ft_sensor e afeta force_mode, screw_driving efreedrive. Usaexternal_force_torqueVECTOR6D e pode configurar watchdog padrão10 Hz se nenhum outro estiver definido. Isso é limite de supervisão, não taxa recomendada do controle. Incluir massa/CoG/offset do sensor na instalação e tratar falha/atraso.

## Atualização de is_within_safety_limits

N10 p.82 documenta posição/juntas e qNear, verificando limites ativos de junta, planos, orientação e alcance. Isso difere da nota de obsolescência emSM511. Preservar ambas com versão: get_inverse_kin_has_solution testa alcançabilidade, sem substituir esse teste nem certificar trajetória/célula. Uma pose válida não valida o caminho inteiro.

**Exercício:** revisar contrato de migração5.25→5.26 sem executar movimento, identificando assinatura de torque, compensação, período, saída e evidência de validação.
