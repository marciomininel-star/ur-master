# 42 — Motion Version, OptiMove e servo por versão

**Fontes:** N10/N26 p.38–39,51–57,62; N12 p.90–92,190–191; N27 contém a edição5.25 do handbook. Não confundir modo de planejar movimento com modo de segurança.

## Motion Version não é velocidade percentual

N10/N26 p.38 diz que a configuração afeta movej/movel. Versão1 reproduz comportamento atéPolyScope 5.21; versão2 altera rampas de aceleração, clamp de limites de hardware, blends e redução dinâmica de raios para evitar sobreposição/skipping. Caminhos de blend e tempos podem mudar, exigindo revisão da aplicação. A melhoria não comprova afastamento de obstáculos.

API motion_version_get/set existe desde5.22; p.39 lista1/2 parae-Series/UR20/UR30 em5.22+, apenas2 emPolyScope X10.9+ eUR15 e posteriores conforme a tabela. Configuração não disponível é ignorada com aviso no log. Portanto, escrever1 não garante comportamento1 em toda plataforma.

Preferir intenção no arquivo de instalação quando esse é o controle do programa; PolyScope aplica no preâmbulo. Mudança local exige preservar/restaurar estado e conhecer persistência até restart/novo programaGUI. Não trocar globalmente para corrigir um único trecho sem avaliar movimentos subsequentes.

## OptiMove tem grandezas diferentes

optimovej/optimovel não são afetadas pela seleçãoMotion Version. N10 p.55–57 descreve a/v como frações(0,1] da capacidade ao longo do caminho, não rad/s² oum/s². r continua emmetros. Não copiar a=1.2 de movel paraoptimovel: excede o domínio e tem significado diferente.

A velocidade absoluta depende da configuração e do caminho.100% não é velocidade cartesiana fixa e não garante peça presa por sucção. Motion Limits/tool_wrench_limit pode limitar aceleração/carga conforme a aplicação, sem substituir validação da garra.

Goals podem ser juntas, poseBase, struct com pose/frame ou nome de objeto doworldmodel conforme a assinatura. Não usar string de frame inexistente nem presumir módulo externo configurado; confirmar contexto de referência. De/para movej/l eoptimovej/l há condições de blend; outras combinações podem exigir repouso inicial.

## Servo e taxa real

N10/N26 p.62 documenta servoj t=0.002, diferentemente do cabeçalhoSW5.11 antigo eSM341t=0.008. Essa é evidência da versão nova, não permissão para substituir período em código legado. get_steptime p.77 retorna o passo do controlador; taxa de envioPC/RTDE, jitter, setpoint e controle devem ser compatíveis.

Ganho alto e lookahead curto podem gerar vibração, especialmente com alvos ruidosos/atualização lenta. Não escolher ganhos por tentativa durante produção. Uma migração deve registrar perfil anterior, versão, caminho, ciclo, blends e resposta física validada.

**Exercício:** comparar movej a/v físicos comoptimovej percentuais e explicar por que30% não permite deduzir um tempo sem geometria e limites do modelo.
