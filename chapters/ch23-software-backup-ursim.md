# 23 — Software, backup, imagem e URSim

**Fontes:** IMG p.4–10; SVC4 p.61–69; VM p.6–24. Guias históricos; não definem caminhos de atualização de versões posteriores.

## Escolher a operação

| Objetivo | Operação | Evidência necessária |
|---|---|---|
| Guardar programa/instalação | Backup de arquivos | Arquivos associados e variáveis |
| Recuperar máquina | Backup de sistema/calibração | Serial, versão e restauração definida |
| Investigar ocorrência | Support File/logs | Evento e horário; não equivale a imagem |
| Mudar PolyScope | Atualização compatível | Hardware, versões inicial/destino |
| Recuperar mídia | Nova imagem | Imagem correta e destino inequívoco |
| Estudar no computador | URSim | Modelo/versão e limites da simulação |

## Recuperação preserva identidade

SVC4 p.69 descreve programas e associados (.urp, .txt, .script, .installation, .variables e históricos), ur-serial, log_history, flightreports e .urcontrol com calibração. Preservar estrutura, versão e dados originais. Calibração de outro braço não substitui a do ativo. Conferir nomes reais: IMG tem grafias inconsistentes.

Support File auxilia diagnóstico; não garante restauração integral. SVC4 descreve esse recurso a partir de 5.8 e backups restauráveis em outro fluxo. Magic Files de backup e upload são ações diferentes; alguns alteram os arquivos do robô.

SVC4 p.61–65 descreve progressão por versões menores antes de 5.5 e outro caminho para versões da época posteriores a 5.5. Isso não autoriza salto para qualquer versão atual. Diferencia downgrade por pacote de restauração de backup e adverte sobre software anterior ao de fabricação.

IMG associa CF a CB2.0/CB3.0, USB a CB2.1/CB3.1 e SD a e-Series no escopo do guia. Gravar imagem apaga o destino. A proposta deve identificar mídia, imagem compatível, inventário de backup e testes posteriores. Se execução for solicitada, confirmar alvo destrutivo concreto antes de gravar; não selecionar disco apenas por letra. O exemplo com Rufus não autoriza instalar ou executar a ferramenta.

## Bancada URSim

VM v03 é de abril de 2020, baseado em VirtualBox 6.1.6 e PolyScope 5.8. Fluxo: VDI apropriada, VM Linux, disco/configuração, modelo e pastas Programs URx. Versões citadas são contexto, não recomendação atual.

VM p.22 diferencia Host Only e bridge segundo necessidade. Host e convidado têm IPs distintos; definir direção da conexão. Ping não valida protocolo ou atualidade dos dados. Simulação permite examinar lógica e sintaxe, mas não valida carga, força real, colisão física ou circuito de segurança.

**Anti-padrões:** reformatar sem preservar calibração; chamar Support File de backup completo; restaurar outro serial; conectar VM à rede externa sem necessidade; supor ausência de efeitos de partida.

**Exercício:** preparar recuperação fictícia sem executar ferramentas: identidade/versão, tipos de backup, destino e verificação de restauração.


## Complemento CB3 e artigos de referência — 2026-10-28

MAG (04/03/2013) descreve Magic Files no contexto antigo de controllerbox2.0. É registro histórico, não exclusão universal de versões novas. Distingue urmagic_log_file.sh, urmagic_backup_programs.sh, urmagic_configuration_files.sh, urmagic_screenshot.zip e urmagic_upload_programs.sh.

No procedimento descrito, inserção doUSB pode iniciar automaticamente arquivos na raiz, em sequência. Indicador vermelho !USB! significa processamento; verde sinaliza conclusão de cada arquivo. Não remover antes do último. Não inserir mídia com scripts desconhecidos para testar backup.

MAG p.1 separa log_history.txt, backup de programas e cópia de configuração. Pastas de log/programas usam serial. P.2 coloca screenshots PNG em screenshots, numerados0–9999, e upload em /programs com subpastas. Upload copia .urp/.txt/.script/.installation e menciona .vars como planejado para1.8; não transformar previsão histórica em nome universal atual. O texto do upload reutiliza a descrição “screenshot”, uma inconsistência editorial.

A proposta deve definir direção, arquivos, origem confiável, compatibilidade, destino e evidência de conclusão. Backup não é upload, cópia de programas não é imagem completa e screenshot não preserva calibração. Nenhum script foi baixado, preparado para autoexecução ou executado nesta incorporação. Referência: MAG p.1–2; para e-Series usar contexto SVC4 e procedimento aplicável.


## Complemento do lote ampliado — 2026-10-28

N12p.248 informa incompatibilidade debackups anteriores5.25 com5.26 e procedimento intermediário5.25;N12p.249 dizMagicFiles desativados por padrão na edição. Export dePolyScopeX não é imagem restaurável5. Conflito de caminhos de atualização emch54.
