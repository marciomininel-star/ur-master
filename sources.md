# Fontes, rastreabilidade e limites

## Identificação do corpus

| Sigla | Arquivo original | Contagem | Papel |
|---|---|---:|---|
| PPP | Advanced_PPP_skr_draft_0.4.pptx | 132 slides | Apresentação com notas do instrutor |
| RG | Advanced_Reference-Guide_skr_draft_0.2.docx.pdf | 35 páginas | Procedimentos e referência |
| WB | Advanced_Workbook_skr_draft_0.3.pdf | 30 páginas | Exercícios, hipóteses e critérios |
| AP | Apresentacao.pdf | 230 páginas | Conteúdo de apresentação, incluindo notas fragmentadas |

Instituição: Universal Robots. PPP slide 2 identifica Sven Krüger como apresentador. Isso não estabelece autoria individual de todos os documentos. Todos os caminhos originais e hashes SHA-256 estão em [source_manifest.json](source_manifest.json).

A numeração usada é física, iniciada em 1. No WB, o índice físico inclui capa e páginas preliminares; o rótulo impresso “1.13” corresponde, por exemplo, à página física 16. Não some 295 páginas e 132 slides como se fossem 427 unidades independentes de conteúdo: há repetição substancial entre fontes.

## Método de extração

- Executado o conversor book-to-skill em modo technical. Docling ausente: fallback explícito para pypdf nos PDFs.
- PPTX não é entrada nativa do conversor: texto de shapes, tabelas e notas foi extraído com python-pptx e incorporado como Markdown intermediário.
- Resultado textual consolidado: 27.108 palavras e aproximadamente 36.144 tokens segundo a heurística do extrator. Esses números não são uso faturado da sessão.
- Manifesto original usa hashes dos quatro arquivos, incluindo o PPTX, não somente do Markdown intermediário.
- Conteúdo foi organizado pelos sete módulos reais; os três “capítulos” detectados automaticamente eram artefatos de headings e linhas de equações, não a estrutura do curso.
- AP contém notas fragmentadas em páginas curtas e conteúdo repetido do PPP. Não foi tratado como um curso adicional independente.
- Não houve execução em controlador, URSim, upload ou conexão com robô.

## Conferência visual realizada

Renderização de nove páginas selecionadas, com pypdfium2, e inspeção do conjunto: RG p.19, 23, 32, 34, 35; WB p.4, 16; AP p.167, 205. Conferidos composição de frames, assistente de plano, modos de força, tabela dos kits, exercício gráfico e divergência de entradas.

Isso é amostragem dirigida, não revisão visual de todos os slides e páginas. Não foram interpretados integralmente vídeos, animações, fotos, screenshots nem objetos incorporados do PPTX. O indicador agregado `images_dropped=0` do extrator não comprova leitura visual: os metadados individuais não contabilizam essas perdas.

## Divergências e omissões preservadas

| Tema | Evidência | Tratamento na skill |
|---|---|---|
| Plane: segundo ponto | PPP 73/AP 167 dizem Y; notas/RG 23 descrevem X | Conferir versão e assistente; não impor uma convenção universal |
| Entradas de encoder | PPP 108/AP 205 dizem DI0–1; notas citam primeiras quatro e 40 kHz | Não fornecer pinagem/frequência definitiva sem manual da versão |
| Simple | Slides simplificam para Z; RG 32 excepciona Line para Y | Preservar exceção |
| Distância | PPP 52/AP 139 misturam título `point_dist()` e chamada `pose_dist(...)` | Não atribuir semântica exata sem verificação |
| Nome de função TCP | RG 17 escreve `get_actual_tcp-pose` no título; PPP 50–51 usa underscore | Adotar `get_actual_tcp_pose()` e registrar erro de título |
| Popup | RG 14 contém `Flase` | Usar `False`; assinatura completa requer referência/visualização adequada |
| Conversão RPY | RG 22 contém erros de grafia e de frase graus/radianos | Seguir chamada `rpy2rotvec([roll,pitch,yaw])` e instrução explícita de radianos |
| Tipos/listas | Restrições antigas em RG 16/PPP 41 | Limitar afirmação à versão do curso |
| MoveJ relativo | RG 24/PPP 77 descrevem restrição da GUI | Não extrapolar para toda versão/API |
| Payload API | Comandos e defaults legados em PPP/RG | Descrever como fonte histórica, sem afirmar recomendação atual |
| Performance review | Aparece na agenda, sem módulo completo entregue | Não criar oitavo capítulo fictício |

## Conteúdo reconstruído versus fonte

Reconstituídos: nomes descritivos de exemplo, pequenas funções aritméticas/de câmera, resolução de listas, exemplo de frame girado 90°, escala de ticks, algoritmo de palete a partir da origem e fórmula de centro por médias. Cada reconstrução está identificada no capítulo correspondente. Não são transcrições nem programas prontos de produção.

Valores de forças, alturas, velocidades, massas e preços pertencem aos exercícios ou exemplos. Um procedimento de “safe position” não estabelece caminho seguro para qualquer célula. Solicitar o modelo, versão, instalação e geometria é apropriado quando eles determinam a resposta técnica; não acrescentar fatos atuais de memória ao corpus.

## Verificação externa

Não foram acrescentadas especificações da web. Quando a pergunta exigir comportamento atual de PolyScope ou URScript fora destes rascunhos, verificar o manual oficial correspondente ao modelo/versão e separar claramente a informação nova da base do curso.

# Incorporação Core — 2026-10-27

## Novos documentos e corpus acumulado

| Sigla | Arquivo original | Contagem | Papel |
|---|---|---:|---|
| CPP | Core_training_e-Series_Presentation_EN_Final_Draft.pptx | 132 slides | Apresentação Core com notas |
| CRG | Core_training_e-Series_Reference_guide_EN.pdf | 69 páginas | Reference Guide, Version 2.0 DRAFT |
| CWB | Core_training_e-Series_Workbook_EN.pdf | 20 páginas | Workbook, Version 2.0 DRAFT |

As siglas RG/WB/PPP/AP acima permanecem reservadas ao Advanced. O corpus acumulado é de **7 documentos, 384 páginas físicas de PDF e 264 slides**; há repetição entre guias, slides e exercícios. Sven Krüger está identificado na apresentação Advanced, enquanto o slide de instrutor Core contém placeholders e não comprova autoria nominal.

Os índices impressos do Core Reference Guide começam uma página após a posição física: impresso 43 é CRG p.44. As referências desta skill usam posição física para evitar ambiguidade.

## Extração e integração do Core

- Reutilizada extração após conferir SHA-256 dos três originais e do Markdown intermediário, além de extraction_mode=technical.
- PDF por pypdf como fallback explícito pela ausência de Docling. Texto, tabelas, grupos de shapes e notas do PPTX por python-pptx, depois incorporados ao extrator como Markdown.
- Resultado do lote Core: 39.641 palavras e estimativa heurística de 52.854 tokens; não equivale a consumo faturado.
- Preservados os sete capítulos anteriores e acrescentados complementos Core em cada um; criados ch08–ch13. Glossário, padrões, guia de decisão e índices foram integrados.
- Manifesto mantém os hashes dos quatro originais Advanced e acrescenta os três originais Core. O Markdown intermediário não substitui o PPTX no manifesto.
- Conferência visual dirigida de nove páginas Core: CRG p.21, 22, 29, 35, 36, 44, 45, 57 e CWB p.15. Com a amostragem Advanced, total de 18 páginas verificadas visualmente, sem pretensão de revisar integralmente mídias.

## Pontos de versão e divergências Core/Advanced

| Tema | Nova evidência | Tratamento |
|---|---|---|
| Plano por três pontos | CRG p.57 e CPP 104 descrevem origem, +X, +Y | Reforça procedimento textual; preservar ressalva de versão e figura esquemática |
| Propriedade Variable | CPP 105–106 diz que fica automática na E-series apresentada | Não exigir checkbox em toda interface |
| Encoder rápido | CPP 14 cita DI8–DI11 e escreve “40kHz?” | Não unificar com DI0–1/primeiras quatro do Advanced nem garantir frequência |
| Auto start | Guia ilustra borda; CPP 52 explica nível já presente na combinação automática | Não prometer nova borda como requisito universal |
| Safety I/O | CRG simplifica ativo; CPP 31 descreve sinais baixos e rótulos potencialmente inconsistentes | Sem pinagem/polaridade definitiva na síntese |
| Planos/copiar TCP | CPP 27–28 alerta dessincronização após edição | Separar referência de processo e cópia de segurança |
| Avaliação contínua | CPP 59/95 e CRG 51 podem interromper corpo de If/Loop | Não tratar como simples teste entre ciclos |
| `force()` | CRG 45 define norma X/Y/Z; CWB 13 pede thread | Não confundir escalar com vetor ou função de segurança |
| Event de força | CWB 13 opcional >9 N; CPP 86 demonstra >10 N | Contextos distintos, não limiar universal |
| Thread Wait | Exercício usa 0,01 s; CPP sugere Wait ou sync | Yield não encerra laço nem garante tempo total exato |
| Pose Editor | CPP 12 menciona confirmação com movimento; CPP 44 destaca edição sem mover | Distinguir edição/ação concreta e versão |
| Contador do palete | CPP 76 tem frase ambígua de limite superior | Não inferir indexação inclusiva; conferir template |
| Presets/catálogos | Valores antigos, tolerâncias por versão; CPP 115 repete mensagem do UR5 no UR10 | Não tratar como catálogo atual |
| Planejamento/segurança | CRG 62/CPP 112 fazem promessa genérica de não lesionar em colisão | Afirmação não adotada como garantia; preservar avaliação de risco exigida em outras passagens |
| URSim/Simulation | CPP 16 e 124 descrevem superfícies e limites diferentes de entradas | Não prometer equivalência física ou compatibilidade atual |
| Recursos online | URLs/estrutura e ferramentas são históricas | Verificar em tarefa atual quando necessário |
| Perguntas CWB 20 | Software atual, torque, resolução, endereço Modbus são perguntas sem respostas | Não preencher por memória nem por tabelas de rascunho |

A preparação de instrutor do CPP 1 inclui apagar instalações e programas. Ela **não foi executada** nem convertida em requisito da skill. As atividades de segurança, partida automática, aplicação de força e backup/upload são descritas como conteúdo; nenhum comando foi enviado a controlador e nenhum equipamento foi alterado.

## Reconstruções acrescentadas

Exemplos novos identificados nos capítulos: comparação de ciclo 12→10 s; deslocamento repetido de 100 mm; tabela de estados do buffer; espaçamentos de padrão 4×3; reset de contador a cada cinco; contagem somente de peças válidas antes de limpeza; matriz de implantação com pendências. Não são resultados medidos ou scripts validados no controlador.

## Mapa de cobertura Core

| Módulo Core | Integração |
|---|---|
| 1 Pick-and-place | ch01, ch05, ch09 |
| 2 Safety Settings | ch08 |
| 3 Optimization | ch09 |
| 4 Easy Startup | ch10 |
| 5 Program Flow | ch02, ch12 |
| 6 Palletizing | ch11 |
| 7 Force Control | ch07 |
| 8 Operator Selection | ch05, ch09, ch12 |
| 9 Flexible Redeployment | ch04 |
| 10 Implementation Plan | ch13 |
| 11 Online Resources | ch13 |

# Incorporação Industrial Communications + Interfaces — 2026-10-28

## Cinco novas fontes

| Sigla | Nome original | Páginas físicas |
|---|---|---:|
| ICRG | Industial Communicaiton ReferenceGuide Ver 0.2.pdf | 24 |
| ICWB | Industial communications Workbook_Ver 0.2.pdf | 11 |
| ICP | Industrial Communications_Ver 0.1.pdf | 77 |
| IFG | Interfaces_ReferenceGuide_v1.pdf | 24 |
| IFP | Interfaces_v1.pdf | 66 |

Nomes preservam a grafia original dos arquivos. IFG identifica Version 1.01 na capa, apesar do nome v1. ICP/ICRG/ICWB são rascunhos 0.1/0.2 e IFP contém referências CB3. Não foi inferida autoria individual a partir dos arquivos.

**Corpus acumulado:** 12 originais, 586 páginas de PDF e 264 slides de PowerPoint com notas; 20 capítulos. As páginas das apresentações ICP/IFP são citadas como páginas físicas do PDF, não pelo número interno impresso em todos os slides. Localizações de arquivos e SHA-256 dos 12 originais estão no manifesto.

## Extração, leitura e integração

- book-to-skill em modo technical, com fallback explícito pypdf pela ausência de Docling.
- Lote novo: 202 páginas, 13.143 palavras, estimativa heurística de 17.524 tokens do extrator; não é uso faturado.
- Antes de retomar, SHA-256 dos cinco originais foi conferido contra metadados e extraction_mode=technical.
- Treze capítulos anteriores mantidos; seis receberam complementos de comunicação. Novos ch14–ch20 cobrem os serviços e conceitos das novas fontes.
- Screenshot não extraído como texto não foi tratado como ausência de conteúdo: houve conferência visual específica de código RTDE e XML-RPC.
- Conferência visual nova: ICRG p.8, 16, 23; IFG p.3, 6, 14, 17, 20, 21, 22, 23; IFP p.55, 56, 63. São 14 páginas novas, somadas às 18 anteriormente amostradas (32 no total). Não é revisão visual integral de todas as fontes.
- Nenhuma rede/porta foi aberta, nenhum serviço externo foi iniciado, nenhum comando foi enviado a robô/PLC. Não houve alteração de IP, firewall, arquivos de controlador, segurança ou estado de equipamento.

## Divergências e lacunas registradas

| Tema | Evidência | Tratamento |
|---|---|---|
| FTP e porta 22 | Módulos intitulados FTP; ICP p.26 diz SSH/SFTP | Identificar serviço efetivo, sem chamar porta 22 de FTP clássico |
| Credenciais antigas | Apostilas fornecem login de bancada | Não apresentar como credencial válida/atual nem usar para conectar |
| Serviços sempre ativos | Frase em ICP sobre servidores | Limitar ao contexto histórico; disponibilidade depende do equipamento/software |
| IP/máscara | Regra dos três primeiros octetos e /24 | Não generalizar a toda rede/máscara |
| Loopback | Exemplos 127.0.0.1 no robô e no servidor | Explicar referência ao próprio host; não ao outro dispositivo |
| Destino MODBUS | ICP p.14 chama destino de cliente | UR cliente consulta servidor definido |
| Signed 16-bit | ICP p.18 inclui 32768 entre positivos | Para reconstrução em complemento de dois: positivos 0–32767, demais raw subtraem 65536; confirmar codificação real |
| I/O Modbus | ICP p.16/19 usa mapas/rótulos inconsistentes DI/DO e leitura/escrita | Não copiar mapa físico sem manual correto |
| Flag de novo dado | ICWB usa 255=300; ICP p.20 mostra 255=1 | Contextos separados; contrato de handshake tem de ser único na aplicação |
| Socket: terceiro campo | ICWB usa X/Y/Z; ICP/IFP usam X/Y/Rz em outro exemplo | Não interpretar terceiro número como altura e rotação ao mesmo tempo |
| Leitura ASCII | Primeiro elemento da lista é quantidade | Validar quantidade antes de usar índices 1…N |
| Newline e grafia de API | IFG p.17 escreve /n; outras páginas usam capitalização de título | Usar referência explícita, sem promover título/erro a sintaxe válida |
| PROFINET GSD/UDT | ICRG p.19 e 21 divergem em data/nome; p.24 repete tipo T2O na direção oposta | Verificar arquivos e direção corretos; não gerar projeto PLC completo a partir dessas linhas |
| Exercícios de SKU/quantidade | Dois SKUs no workbook; três registradores no guia; 10 peças/SKU versus caixa de quatro | Manter variantes identificadas |
| Speed slider/I/O | ICWB p.11 cita register 32/64 sem layout completo | Não transformar em índices diretos de API sem tabela/máscara/banco |
| PROFIsafe e classes IRT | Apresentados como pergunta/conceito | Não inferir suporte de comunicação de segurança ou todas as classes pelo UR |
| Mapa de portas | IFG p.18 diz “any port would work” | Não adotar: cada serviço tem protocolo específico |
| Taxas | IFP p.43–54 identifica CB3 e 10/125 Hz | Não transformar em taxas universais E-series |
| Pose medida | IFG p.19 usa get_current_tcp_pose | Não confirmar como API; fontes anteriores usam get_actual_tcp_pose |
| Python básico | IFG p.3 exige aspas duplas; p.6 inclui end; material mistura raw_input/imports legados | Corrigir em reconstruções Python 3 identificadas, preservando distinção URScript |
| RTDE e recipe | Código majoritariamente em screenshots e arquivos auxiliares ausentes | Documentar campos/handshake visíveis, sem inventar pacote completo |
| MoveL para ServoJ | IFG p.21 sugere troca e parâmetros de exemplo | Não tratar como substituição universal ou controle físico validado |
| XML-RPC servidor | IFG p.22 tem URL sem aspas em bind; imports Python antigos | Separar URL cliente de tupla host/porta e usar reconstrução Python 3 explícita |
| Pose RPC | IFP p.63 usa urlib.poseToList/listToPose | Não assumir lista genérica como serialização correta sem helpers/contrato |
| Workbook Interfaces | Slides remetem a Trainee Workbook não fornecido | Não alegar cobertura de tarefas ausentes nem inventar enunciados |

## Reconstruções novas e verificação offline

Os capítulos identificam como reconstruídos: conversão signed16/escala do exercício, validador de resposta XYZ, preparação de bytes de uma linha URScript, configuração de servidor XML-RPC sem invocação e modelos de handshake. Os exemplos Python são conferidos offline quanto à sintaxe e resultados indicados; não são clientes RTDE, parser binário ou projetos PLC completos. O programa de criação do servidor não foi chamado.

No par RTDE do IFG p.20, a diferença 0,300−0,234 = 0,066 m é conferência aritmética; não valida alcance, movimento ou parâmetros de servo. O watchdog de screenshot é exemplo operacional e não função de segurança.

## Cobertura dos módulos novos

| Coleção/módulo | Localização integrada |
|---|---|
| Industrial 1 Modbus | ch14; câmera/pose no ch03 |
| Industrial 2 FTP | ch15; backup no ch13 |
| Industrial 3 Dashboard | ch15; startup no ch10 |
| Industrial 4 Socket | ch16 |
| Industrial 5 EtherNet/IP | ch17; paletização no ch11 |
| Industrial 6 PROFINET | ch17; limites de segurança no ch08 |
| Interfaces 1 Programming | ch20; complemento no ch02 |
| Interfaces 2 URScript | ch02 e ch18 |
| Interfaces 3 Socket | ch16 |
| Interfaces 4 Client Interfaces | ch18 |
| Interfaces 5 RTDE | ch19 |
| Interfaces 6 XML-RPC | ch20 |

Não foram acrescentadas especificações atuais por pesquisa externa. Para implementar fora do contexto dos rascunhos, confirmar manual oficial do modelo/software, protocolo e biblioteca usados e identificar separadamente a informação nova.


## Integração dos manuais — 2026-10-28

**Corpus cumulativo:** 28 arquivos; 1.406 páginas PDF, 264 slides PPTX, 1 MHTML e 1 PNG. Os 12 originais e 20 capítulos anteriores foram preservados; 16 capítulos receberam complementos e foram criados ch21–29. As seções anteriores deste arquivo documentam lotes históricos, não o total atual.

| Sigla | Arquivo | Páginas / formato | Uso |
|---|---|---|---|
| IG | Installation Guide V1.3.3_ENGLISH.pdf | 12 | Recomendações de implantação; ch21 |
| IMG | install-new-image-all-types.pdf | 10 | Nova imagem/mídia; ch23 |
| PN | pn-iomessage.pdf | 11 | Mapa PROFINET; ch29 |
| SM511 | scriptManual_SW5.11.pdf | 149 | API e semântica SW5.11; ch26–28 |
| MAINT | Universal Robots - Inspection And Maintenance Plan.mhtml | MHTML | Captura de manutenção; ch24 |
| UR5DS | ur5_en.pdf | 1 | Ficha UR5 histórica; ch22 |
| SF3 | ur-g3-safety-functions-20170509.pdf | 10 | Segurança CB3 histórica; ch08 e matriz |
| VM | ursim_vmoracle_installation_guidev03_en.pdf | 24 | Virtualização URSim; ch23 |
| U5E | 99443_UR5e_User_Manual_pt_Global.pdf | 295 | Hardware/PolyScope UR5e; ch21–28 |
| DEP | Deployment_Workshop_Online.pdf | 50 | Implantação e carga por estado; ch21/ch27 |
| SVC4 | e-Series_Service_Manual_en_4.0.0.pdf | 234 | Serviço e inspeção; ch23–25 |
| SAFE19 | 08_03_2019-ur_safety_made_easy.pdf | 2 | Conceitos de segurança históricos; matriz |
| DWG1000698 | 1000698.PDF | 2 | Dimensional UR5e; ch22 |
| OEMDWG | 1005703_1005747.pdf | 1 | Caixa OEM AC/DC; ch22 |
| UR10DWG | UR10_schematic_drawings_rev3.0.0.2.pdf | 19 | Esquema UR10 CB3; ch22 |
| TPIMG | 24091.png | PNG | Imagem com cota188mm; ch22 |

### Extração e limites deste lote

Foram processadas todas as 820 páginas dos 14 PDFs. O conversor book-to-skill foi executado em modo technical, sem Docling disponível; o fallback pypdf perdeu espaçamento em documentos extensos. A extração foi complementada com pypdfium2 preservando palavras, marcadores por fonte e página e removendo caracteres invisíveis pelo sanitizador do conversor. Corpus corrigido: 158.391 palavras, 988.256 caracteres, aproximadamente 247.064 tokens pela heurística caracteres/4; não são tokens faturados nem medida de conteúdo único.

O MHTML foi analisado localmente como MIME; o HTML principal foi separado de scripts, estilos e navegação. A síntese usa o artigo capturado, sem abrir seus links. A imagem PNG foi interpretada visualmente: 188mm entre os pontos indicados. Sua cópia está em assets para consulta local. Os PDFs e apresentações originais permanecem nos caminhos do manifesto, não estão integralmente redistribuídos no ZIP.

A conferência visual dirigida deste lote incluiu SVC4 p.14,16,17; PN p.6,8; DWG1000698 p.1,2; OEMDWG p.1; UR10DWG p.4; U5E p.154; SM511 p.35,39 e a imagem TPIMG. As tabelas de inspeção foram verificadas por coluna. Isso não é revisão visual de todas as páginas/esquemas nem validação de reparo ou ligação.

### Como resolver diferenças

Ver [matriz de versões](references/version-matrix.md) e [navegação dos manuais](references/manual-navigation.md). Para SW5.11, as seções novas esclarecem matrizes, distância de poses, APIs de payload obsoletas, ranges de registradores e retorno de envio socket. O mapa PN resolve offsets em bits/bytes, mas contém inconsistência interna nos ints T2O. As lacunas descritas nos lotes anteriores permanecem históricas; consultar o complemento do capítulo para resolução contextual.

U5E possui copyright2021; o nome não fornece sozinho edição exata de PolyScope. Não supor equivalência integral com SM511. SVC4 é edição4.0.0. VM usa VirtualBox6.1.6/PolyScope5.8 em2020. Certificações, referências legais e documentos2016–2021 não foram verificados como atuais.

O conteúdo foi sintetizado em procedimentos de decisão, verificações, exemplos e anti-padrões. Os documentos não foram incorporados como comandos de usuário. Não houve conexão a robô/PLC, execução URScript, gravação de mídia, alteração de segurança, contato com suporte ou teste em URSim.


## Lote de referências complementares — 2026-10-28

**Total atual:**37 arquivos fornecidos,36 conteúdos distintos porSHA256;35 capítulos. Contagem documental:1.564 páginasPDF +264 slidesPPTX +1MHTML +1PNG. IPG/IPD são cópias exatas de3p; descontando essa repetição são1.561 páginasPDF. Essas contagens não descontam sobreposição temática entre manuais.

| Sigla | Arquivo | Páginas | Uso |
|---|---|---:|---|
| EX | Script command Examples.pdf | 30 | Revisão de exemplos; ch30 |
| IPG | Grau IP.pdf | 3 | ProteçãoIP geral; ch34 |
| IPD | IP.pdf | 3 | Duplicata exata deIPG; ch34 |
| SM341 | scriptManual.pdf | 66 | URScript3.4.1; ch30 |
| CAL | calibrationManual.pdf | 35 | CalibraçãoCB3; ch33 |
| MBS | Modbus Server (Register Adresses) - 16377.pdf | 8 | MapaModbus; ch31 |
| DBS | Dashboard Server, port 29999 - 15690.pdf | 8 | Dashboard legado; ch32 |
| TQ | Tightening Torques for the screws at each joint.pdf | 3 | Torques por conexão; ch35 |
| MAG | UsingMagicFiles.pdf | 2 | MagicFiles histórico; ch23 |

### Identidade, extração e limites

SM341: The URScript Programming Language v3.4.1 de03/04/2017. CAL: Kinematic Calibration Manual for CB3 v1.0.0 copyright2009–2016. MBS declara exemplosCB2≥1.6/CB3; DBS declaraCB2≥1.4/CB3≥3.0, com colunas até3.3. Esses artigos históricos não garantem comportamento de todo firmware posterior. TQ não tem revisão formal identificada; tabelasUR3/5/10 não foram tratadas como e-Series. EX não tem versão formal identificada.

IPG/IPD possuem rodapé Holland Shielding Systems e revisão02/05/2017: são referência geral de proteção, não manuais/certificaçãoUR. A instituição predominante da skill permanece Universal Robots, com esta fonte complementar externa. MAG é impressão de artigo de04/03/2013, com trecho de recurso ainda planejado para1.8.

Conversor book-to-skill executado technical, Docling ausente, fallbackpypdf; complementação pypdfium2 por página. Foram extraídas158 páginas,31.713 palavras e186.865 caracteres (~46.716tokens por caracteres/4), incluindo duplicata; o fallback estima~41Ktokens. Números de extração não são uso faturado nem medida de conteúdo único. A versão pypdfium2 foi usada para preservar espaçamento e confrontar tabelas. IP tem mojibake no texto; não o tratar como código.

Conferência visual dirigida: MBS p.2–3; DBS p.2; TQ p.1–2; IPG p.2; CAL p.11,23,26,35; EX p.26,29. Demais páginas foram consultadas textualmente conforme tema; isso não é revisão visual integral dos esquemas/screenshots nem ensaio no hardware. IPD não requer segunda síntese pois é byte a byte idêntico aIPG.

### Erratas preservadas

MBS p.2: fórmula signed16 errada e intervaloGP128–256 divergente da tabela128–255. DBS: semântica de confirmação muda em3.3. EX: erros de aridade, nome, cópia de chamada e domínio; revisão em ch30. CAL: endereçoIPv4 com cinco grupos em p.18; não corrigir por palpite; Reload default não é factory reset. IP: tabela rápida não substitui condições de ensaio. MAG: texto de screenshot reutilizado no upload e extensão.vars futura à época.

Os28 originais anteriores foram preservados no manifesto;29 capítulos anteriores mantidos,13 com complementos. Acrescentadosch30–35, glossário, padrões, guia e roteamento. A síntese interpreta instruções dos documentos como conhecimento técnico; não houve conexão, movimento, alteração de segurança/calibração, escritaModbus, execuçãoMagicFile ou contato externo.


## Lote ampliado UR Manuals — 2026-10-28

**Total atual:**75 arquivos fornecidos,73SHA256distintos;54 capítulos. Contagem documental:7.767 páginasPDF +264 slidesPPTX +1MHTML +1PNG. Descontando as cópias exatasIPG/IPD(3p) eN03/N07(242p), são7.522 páginasPDF. Não desconta sobreposição temática entre revisões e manuais dos modelos.

O novo lote tem38PDFs,6.203p;37conteúdos distintos,5.961p. Não há hash já presente nos37arquivos anteriores. Nome com(1) não foi usado como critério de duplicação: somenteN03/N07 coincidem byte a byte. SW5.25 e5.26, manuais de calibração,Service Handbookse-Series/URSeries eSoftware Handbook foram preservados separadamente.

| Sigla | Arquivo | Páginas | Capítulos de aplicação |
|---|---|---:|---|
| N01 | UR3_User_Manual_en.pdf | 222 | ch36, ch37 |
| N02 | UR10_User_Manual_en.pdf | 222 | ch36, ch37 |
| N03 | UR10_Service_Manual_en (1).pdf | 242 | ch37 |
| N04 | UR3_Service_Manual_en.pdf | 229 | ch37 |
| N05 | UR5_Service_Manual_en.pdf | 236 | ch37 |
| N06 | UR5_User_Manual_en.pdf | 222 | ch36, ch37 |
| N07 | UR10_Service_Manual_en.pdf | 242 | ch37 |
| N08 | e-Series_Service_Manual_en (1).pdf | 135 | ch38, ch39, ch54 |
| N09 | Service_Manual_UR-Series_en (1).pdf | 97 | ch38, ch39, ch54 |
| N10 | script_directory_Poly5 (1).pdf | 183 | ch41, ch42, ch43, ch44 |
| N11 | UR Calibration Guide Combined.pdf | 61 | ch40 |
| N12 | e-Series_Software_Handbook_en_Global (1).pdf | 257 | ch42, ch45, ch46, ch54 |
| N13 | 740-949-00_High Torque Screwdriving_User_Manual_en_Global.pdf | 32 | ch51 |
| N14 | 723-162-00_Connectivity Kit_en.pdf | 38 | ch49 |
| N15 | 740-949-00_Base-Mounted Range Extender, 40m_User_Manual_en_Global.pdf | 37 | ch48 |
| N16 | 716-197-00_OEM_Control_Box_Installation_Guide_en.pdf | 50 | ch47 |
| N17 | 726-540-00_Robot Cable_User_Manual_en_Global.pdf | 30 | ch48 |
| N18 | 99373_IMMI_installation_guide_en.pdf | 41 | ch50 |
| N19 | 718-625-00_3PETP_installation_guide_en.pdf | 24 | ch46 |
| N20 | 738-379-00_E-Series_e-Series_en_Global.pdf | 42 | ch39 |
| N21 | 738-380-00_UR20_UR20_en_Global.pdf | 34 | ch39 |
| N22 | ErrorCodes.pdf | 174 | ch52 |
| N23 | UR Calibration Manual Combined.pdf | 39 | ch40 |
| N24 | e-Series_Service_Manual_en.pdf | 126 | ch38, ch54 |
| N25 | Service_Manual_UR-Series_en.pdf | 66 | ch38, ch54 |
| N26 | script_directory_Poly5.pdf | 178 | ch41, ch42, ch43, ch44 |
| N27 | e-Series_Software_Handbook_en_Global.pdf | 257 | ch42, ch45, ch46, ch54 |
| N28 | 718-786-00_UR30_User_Manual_en_Global.pdf | 247 | ch36, ch53 |
| N29 | 762-375-00_UR18_User_Manual_en_Global.pdf | 245 | ch36, ch53 |
| N30 | 718-818-00_UR20_User_Manual_en_Global.pdf | 255 | ch36, ch53 |
| N31 | 718-890-00_UR15_User_Manual_en_Global.pdf | 247 | ch36, ch53 |
| N32 | 762-404-00_UR8 Long_User_Manual_en_Global.pdf | 249 | ch36, ch53 |
| N33 | 733-429-00_UR12e_User_Manual_en_Global.pdf | 239 | ch36, ch53 |
| N34 | 711-102-00_UR16e_User_Manual_en_Global.pdf | 241 | ch36, ch53 |
| N35 | 711-039-00_UR10e_User_Manual_en_Global.pdf | 247 | ch36, ch53 |
| N36 | 733-466-00_UR7e_User_Manual_en_Global.pdf | 235 | ch36, ch53 |
| N37 | 710-965-00_UR5e_User_Manual_en_Global.pdf | 239 | ch36, ch53 |
| N38 | 710-943-00_UR3e_User_Manual_en_Global.pdf | 243 | ch36, ch53 |

### Extração e cobertura

Conversor book-to-skill executado emtechnical nos38originais;Docling ausente, fallbackpypdf. O resultado fallback perdeu espaçamento em documentos extensos e contabilizou270.830palavras/~361Ktokens, sem representar adequadamente a legibilidade do corpus. Extração complementarpypdfium2 por página para37conteúdos únicos:1.147.250palavras/7.043.261caracteres (~1.760.815tokens por caracteres/4). Esses valores não são uso faturado nem medida de conteúdo independente.

As páginas complementares foram normalizadas com o sanitizador do conversor para caracteres invisíveis e tratadas como corpus consultável, com seleção por título, tema, termo e página. Síntese em português, sem copiar o texto integral dosPDFs para a skill. Manuais originais permanecem nos caminhos do manifesto; oZIP contém conhecimento sintetizado e índices, não todos osPDFs. O índice de erros contém apenas ocorrências código→página e pode incluir sumário ou referência cruzada.

Novos capítulos36–54 organizam modelos, serviço, juntas, calibração,URScript, movimento, dinâmica, comunicações, software, modos,OEM,cabos/extender,connectivity,IMMI,aparafusamento,erros,montagem e migração. Os capítulos antigos permanecem e recebem complementos de aplicabilidade. Consultar a versão nova não autoriza alterar ativo, mídia, segurança, calibração, rede ou firmware.

### Conferência visual dirigida

Renderizadas e conferidas18páginas: N31p.14;N35p.15;N28p.95;N30p.98;N32p.97;N11p.34;N08p.15,32;N09p.15,30;N15p.14;N17p.7;N10p.41,38;N13p.11;N18p.18;N19p.19;N12p.117. Verificadas as diferenças de capacidade/curva,Go/NoGo,inspeção,torque,compatibilidade,assinaturaAPI,alavanca,MAF,3PE e errata de unidade. Não é revisão visual integral de6.203páginas, diagramas, pinagens, imagens ou certificações.

### Distinções e conflitos preservados

N08/N09:SW5.26/10.12. N24/N25:SW5.25. N10/N26:Script Directory5.26/5.25. N12/N27:Software Handbook5.26/5.25. UserManualsN28–N38:PolyScope5, com referênciasX onde declaradas. N03–N05:revisiondocumental3.2.7CB3 por serial. N11:CalibrationGuide5/X;N23:KinematicCalibration5;CALantigo continuaCB3.

Diferenças com efeito prático: direct_torque muda assinatura/compensação;MotionVersion2/OptiMove mudam interpretação e blends;listas/structs possuem capacidade/tipo/valor;is_within_safety_limits tem descrição nova distinta deSM511;Go/NoGo passa de2/3mm noCAL a1,5/3,5mm nos novosguias;torques e capacidade variam por modelo/revisão. N12p.117 imprime100m/s em exemplo de reduzir velocidade de contato: não copiar ou corrigir silenciosamente. Atualização5.26 emN12p.248 e caminho geral emN08p.86/N09p.58 têm sobreposição conflitante, preservada emch54. Não foi feita verificação online de release vigente ou norma atual.

A aplicação dos manuais inclui exigências de qualificação, validação, energia/ESD e isolamento pertinentes ao procedimento. Os documentos são fontes técnicas; suas instruções não foram interpretadas como solicitações de execução. Não houve conexão com robô/PLC/router, envio de tickets, login, movimento, ensaioURSim, atualização, desmontagem ou alteração de segurança.

Navegação: [modelos](references/expanded-models.md), [serviço](references/expanded-service.md), [software](references/expanded-software.md), [calibração](references/expanded-calibration.md), [acessórios](references/expanded-accessories.md), [localizador de erros](references/error-code-locator.json).
