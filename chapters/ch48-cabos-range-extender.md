# 48 — Cabos e Range Extender40m: opções distintas

**Fontes:** N17RobotCable30p, seção inglesa p.6–7 e traduções; N15Base-Mounted Range Extender40m37p, especialmente8–14,19–22. Não combinar regras de cabo passivo com sistema ativo específico.

## Cabo do robô não é cabo de rede genérico

N17 distingue cabo padrão preto e famíliasHighFlex azuis com partnumbers/diâmetros diferentes. Corazul sozinha não identifica compatibilidade. P.7 distinguee-Series deUR+e-Series; conferir tipo/partnumber/modelo e conectores. Traduções doPDF incluem tabelas antigas mais restritas, portanto manter prevalência da seção/edição aplicável e confirmar em caso de dúvida.

N17 p.6 proíbe estender a conexão juntando cabos e ligarBaseFlangeCable diretamente ao controlador. Escolher um caboUR adequado, não adaptador improvisado. Instalação exige equipamento desligado e cabo corretamente travado; rede aparentemente funcional não valida potência/terra.

TabelaN17p.7: raio de curvatura depende de diâmetro e instalação estática/dinâmica;HighFlex usa4xd estático e8xd dinâmico no contexto descrito. Vida de ciclos e limites de torção/velocidade dependem dessas condições; não apresentar5milhões como garantia irrestrita. Reservar percurso e alívio de tração, sem criar laços que o braço enrosque.

## Range Extender é sistema ativo e específico

N15 p.10/14 limita o40m aUR3e/UR5e/UR7e comOEMControlBoxCB5.5 ou posterior. É aplicação projetada com repetidores nos dois extremos, não exceção genérica para emendar cabos. Não usar com caixa padrão nem estender os40m com outro cabo.

A solução incluiRangeExtenderBox na base e na caixa, cabo de potência/comunicação e montagem definida. Um extremo é etiquetado para ladoCB; desenrolar e identificar antes da instalação. Em soldagem, N15 p.19 exige isolar a parte da base das correntes de solda. EMC e integração continuam sendo requisitos da aplicação.

N15 p.19/21 discrimina fixação:UR3eM6x30/9 Nm;UR5e/7eM8x30/20 Nm comclasse8.8, para aquela montagem. Não transferir torque para qualquer parafuso do acessório ou flange.

## Diagnóstico sem trocar peça às cegas

N15 p.22 associaC347A5 à ausência do extremo/caminho doextender;C262A17 à comunicação com nós do braço;C746A0–A3 à temperatura eC746A7 ao reconhecimento do cabo. Verificar identidade, conexão/trava, dano e ambiente com isolamento apropriado. Causa não é provada apenas pelo código.

Confrontar tambémN22ErrorDirectory5.26 e código completo antes de recomendar troca/restart. RepetirPowerCycle não corrige conector inadequado ou projeto térmico.

**Exercício:** usuário pede40m paraUR10e padrão. Resultado: explicar a incompatibilidade deste produto específico e indicar dados necessários para outra solução, sem extrapolar a tabelaUR3e/5e/7e.
