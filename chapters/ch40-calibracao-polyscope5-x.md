# 40 — Calibração moderna: PolyScope 5/X e mudanças do kit

**Fontes:** N11Calibration GuidePolyScope 5/X61p; N23Kinematic CalibrationPolyScope 539p. Relacionado: CAL/ch33CB3, que permanece histórico e não é substituído por analogia.

## Separar três identidades

Registrar família/modelo/serial de cada braço, software5/X de cada controlador e kit/ferramental/revisão. Uma tabela de preparação que lista modelos não comprova qualquer par misto. Confirmar combinação e montagem do kit antes da operação. N11 possui preparações e aplicação separadas paraPolyScope 5 eX; não misturar telas e estados.

N11 p.14/23 usa52,4graus de tilt no dispositivo e rotação270graus paraUR3e,90graus para os demais modelos listados. São valores da bancada de calibração, não recomendação de montagem da célula. N11 listaUR8Long/15/20/30 além de modelos e-Series; não inferirUR18/UR7e/UR12e automaticamente quando a tabela não os nomeia.

## Rede e sessão corretas

N11 p.18 documenta Master10.17.17.18 eSlave10.17.17.19 em ligação direta por cabo. Isso resolve os cinco grupos numéricos inválidos do CAL antigo, mas apenas para esse procedimento. A fonte alerta sobre conflito comLAN e quebra de comunicação. PolyScope X usa Leader/Follower e configuração de sessão própria; não importar redeCB3 por palpite.

Conexão de rede não comprova fixação do Calibration Connector. O início de coleta desencadeia movimento coordenado; a ordem mecânica e as confirmações da tela pertencem ao procedimento de serviço. N11 p.31 indica que Auto step não pode ser desabilitado com3PE no fluxo descrito; não presumir operação manual sem movimento automático.

## Estatística e validação física

N11 p.33 distingue Calibration Results de validação, com média, desvio padrão e máximo emmm/mrad. Cor verde e números de exemplo não são tolerância universal de processo. Não transportar os critérios numéricosCAL2016 sem verificar guia/ferramental atual.

Mudança concreta: CAL/ch33 usavaGo2 mm/NoGo3 mm. N11 p.34 e N23 p.30 usam **Go 1,5 mm/NoGo 3,5 mm**, com distância nominal2,5 mm. N11 p.34 ilustra2,5±1 mm. Não combinar ferramenta antiga e critério novo. Antes do ensaio, flanges devem estar livres de parafusos/pinos/conector conforme a etapa. Resultado cinemático não substitui essa verificação física e a verificação deHome.

N11 p.34 afirma persistência após validação bem-sucedida. Não confundir com o fluxo antigo que tinha outras opções deSave calibration/Correct home. Registrar exatamente quando dados passam a ser persistentes.

## Avaliação e retorno

Se falhar, investigar identidade, montagem limpa/repetível, fixações, orientação, configuração e evidências do ensaio; não ampliar presets para forçar aprovação. A fonte contém telas de ajuste de segurança para a bancada, não instrução geral de afrouxar limites da célula. Preservar calibração original do serial e rastrear versões/arquivos.

**Exercício:** comparar relatórios de dois kits comGo/NoGo diferentes. Resultado: escolher guia/kit compatíveis e declarar critério da edição, sem concluir que qualquer vão próximo2,5 mm serve para todo procedimento.
