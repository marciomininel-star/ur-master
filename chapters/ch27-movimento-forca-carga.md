# 27 — URScript SW5.11: movimento, força e carga

**Fontes:** SM511 p.38–69, 85–90, 132–149; U5E p.206–208, 226–227; DEP p.17–24. Complementa ch03/05/07/09.

## Movimento pelo caminho necessário

movej: a/v articulares em rad/s² e rad/s; movel: m/s² e m/s. t especificado tem precedência sobre a/v; r de blend é em metros. Tempo desejado não garante viabilidade. movec usa início atual, via e destino; mode=1 mantém orientação relativa à tangente, não fixa no mundo.

O cabeçalho servoj do manual mostra t=0.008; não substituir por 0.002 só porque o ciclo interno é 500 Hz. Avaliar período, taxa de setpoints, lookahead, ganho e ruído juntos. Alto ganho/pouco lookahead podem causar instabilidade.

SM511 p.46 e U5E p.206 avisam conflitos entre Path Offset, Force e Conveyor Tracking: não combinar no escopo documentado. Parâmetros de offset podem persistir; projetar entrada, filtragem, saída e parada.

## Geometria e cinemática

point_dist ignora orientação; pose_dist considera orientação, mas a seção não dá fórmula completa para reprodução. pose_add soma posições e compõe rotações R1*R2, não componentes angulares. Para offset local, usar pose_trans com ordem dos frames e pose_inv quando necessário.

get_actual_tcp_pose usa encoders e Base; flange e TCP diferem. get_inverse_kin usa qnear para solução próxima. get_inverse_kin_has_solution testa existência cinemática, não colisão ou segurança certificada. is_within_safety_limits está obsoleta em p.65 devido a diferença de algoritmo de IK; a recomendação de IK_has_solution não a transforma em função de segurança.

## Força: medição ≠ comando

get_tcp_force retorna seis componentes [Fx,Fy,Fz,Tx,Ty,Tz], referidas aos eixos da Base e com compensação de payload descrita no manual. Apesar da notação p[], não são uma pose em m/rad. force() é norma das três forças e perde direção. Não aplicar pose_trans ao wrench como se fosse posição.

force_mode(task_frame,selection_vector,wrench,type,limits): seis seletores de compliance, wrench em N/Nm; type 1 Point, 2 Frame, 3 Motion. Simple é opção GUI, não quarto inteiro da API. Em eixos complacentes, limits limita velocidade (m/s ou rad/s); nos demais, desvio (m ou rad). Não é vetor de limites de força.

Força real pode ficar abaixo da solicitada por limites de juntas. Evitar aceleração/desaceleração intensa na transição e durante o modo; prever end_force_mode e retomada. U5E p.207 define Simple em Z da feature, mas Y para feature linha. Ganho acima de 1 pode ser instável. O exemplo de SM511 p.39 tem pose malformada: não copiar literalmente.

## Carga por estado

Desde 5.10, SM511 recomenda set_target_payload(m,cog,inertia=[0,0,0,0,0,0]) em lugar dos setters antigos. Massa kg; CoG em m desde a flange; inércia kg*m² na ordem [Ixx,Iyy,Izz,Ixy,Ixz,Iyz], origem no CoG, eixos alinhados à flange. Padrão zero da API não equivale à estimativa de esfera da GUI U5E p.227.

set_payload antigo pode vincular CoG ao TCP se omitido; trocar TCP pode então alterar CoG. Setters antigos têm efeitos sobre inércia/tara (p.66–69); não migrar apenas o nome.

Para centros no **mesmo frame da flange**:

CoG_total = soma(m_i * CoG_i) / soma(m_i).

DEP p.17–20: garra 0,8 kg a 0,052 m + peça 1,35 kg a 0,180 m → 2,15 kg e Cz≈0,13237 m, arredondado no slide para 0,132 m. Não somar offsets com origens diferentes. Após pegar/soltar, ativar a carga correspondente; configuração não confirma peça presente.

## Remote TCP / toolpath

SM511 p.132 exige execução no PolyScope e controlador Remote TCP & Toolpath URCap ativo. mc_* depende desse contexto; PCS é frame da peça. Inicialização define TCP regular/remoto e filas retornam IDs, não confirmação física. O manual alerta sobre mc_add_circular: sexto argumento mode pode faltar na lista da GUI. Consultar seção completa antes de montar processo.

**Exercício:** explicar por que point_dist pode ser zero entre poses com orientações diferentes e por que isso não autoriza tratar o alvo como igual.


## Complemento do lote ampliado — 2026-10-28

N10/N26 documentamservoj t=0.002,OptiMovepercentual,MotionVersion2,transition_time em payload e descrição nova deis_within_safety_limits. Esses pontos superam cabeçalhos antigos somente no contexto novo. Verch42/43.
