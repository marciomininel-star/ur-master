# UR_Master — guia de decisão

## Programa, geometria e estado

| Quando… | Faça… | Motivo |
|---|---|---|
| Há ramos, máquina ou inspeção | Desenhe fluxo e sinais | Expõe esperas/exceções |
| Precisa de cálculo reutilizável | Função com argumentos/retorno | Evita duplicação |
| Reutiliza sequência gráfica | SubProg; pasta só organiza | Chamada não é agrupamento |
| Caminho do TCP não é requisito e há espaço | Avalie MoveJ | Movimento articular |
| Aproximação exige linha | Avalie MoveL | Caminho cartesiano |
| Processo pede velocidade contínua | Avalie MoveP | Não inserir espera de operador no trecho contínuo |
| Waypoint precisa ser atingido exatamente | Não contorne com blend | Blend desvia do ponto |
| Entradas estão em mm/graus | `/1000.0`, `d2r()` | Script usa m/rad |
| Orientação vem em RPY | `rpy2rotvec()` | Pose usa vetor de rotação |
| Offset segue câmera/peça girada | `pose_trans(frame, local)` | Compõe referenciais |
| Repete forma em outras peças | Feature variável e origem preservada | Reutiliza trajetória |
| Deposita em padrão de posições | Pallet Template | Separa distribuição e ação local |
| Pegou/soltou peça | Rever massa/CoG | Estado físico mudou |
| Entrada do operador é inválida | Default/erro, sem contar sucesso | Tentativa não é peça concluída |

## Partida e reinício

- Carregar programa ≠ inicializar braço ≠ iniciar execução.
- BeforeStart/Init Variables: uma vez antes do principal; não resetar contador a cada ciclo.
- Auto load/start/initialize combinados: CPP 52 admite nível já presente, sem nova borda.
- Primeiro waypoint relativo/variável: pode evitar Auto-Move; verificar caminho atual.
- Buffer/palete: reset lógico não esvazia peças reais.
- Reensinar feature/TCP: revisar cópia para Safety; pode estar dessincronizada.

## Contato e segurança

| Necessidade | Recurso | Verificação |
|---|---|---|
| Pressão em uma direção | Simple | Z usual; Y para Line no RG Advanced |
| Compliance em eixos escolhidos | Frame | Selecionado a 0 N cede |
| Pressão radial a centro | Point | Y à origem; distância ≥10 mm no RG |
| Força transversal ao avanço | Motion | Frame depende do movimento |
| Ler força escalar | `force()` em thread | Norma, não direção nem função de segurança |
| Fronteira espacial | Safety Plane | Não limita automaticamente todos os elos |
| Volume/orientação de ferramenta | Tool Position/Direction | Esfera versus cone |
| Região de operação reduzida | Reduced Mode | Não elimina risco automaticamente |
| Retorno manual após violação | Recovery | Não presumir planos/posição/orientação ativos |

## Tracking

Escala e direção → PICK_POS no acionamento da barreira → espera do evento → trecho no tracking. Pinagem/frequência dos rascunhos divergem entre coleções; identificar controlador/versão antes de orientar ligação. Novo modo só no fim do ciclo quando esse for o requisito.

## Números: sempre manter contexto

| Valor | Fonte/uso didático |
|---|---|
| 3 s | WB p.5: janela de qualidade |
| Cada 5ª peça | CWB p.10: inspeção |
| 7 peças válidas | CWB p.15: limpeza de bico |
| 4×3 = 12 | CRG p.44: padrão Square |
| ≥3 poses, quarta adicional | RG p.25 / CPP 92: calibração TCP |
| 10 mm mínimo | RG p.34: TCP–origem no Point |
| 20 s / máximo 50 mm/s | WB 7.2: ensaio circular |
| 10 N por 10 s; opcional >9 N | CWB p.13: ensaio e Event |
| Wait 0,01 s | CWB p.13: thread; não garantia de período total |

Ensaios não definem parâmetros universais. GUI pode usar mm/°; comandos de pose/CoG usam m/rad e massa kg. Catálogos antigos, presets e URSim não comprovam especificação atual ou validação física.

## Comunicação: rota rápida

| Necessidade | Serviço / direção no curso |
|---|---|
| Bits/registros de dispositivo | Modbus TCP, 502; UR pode ser cliente/servidor |
| Transferir arquivo | SSH/SFTP, 22, apesar do título FTP |
| Consultar/carregar/iniciar programa | Dashboard, 29999; PC/PLC cliente |
| Receber câmera em protocolo próprio | UR cliente → servidor PC/câmera, porta combinada |
| PLC cíclico | EtherNet/IP Adapter ou PROFINET IO-Device no UR |
| Enviar URScript | PC cliente → 30001/30002/30003 da versão |
| Campos/registradores escolhidos | RTDE, 30004; recipe + programa consumidor |
| Função no PC chamada pelo UR | XML-RPC; endpoint do servidor, 8080 no exemplo |

- Mesmo índice não significa mesmo banco Modbus/PLC/RTDE.
- ASCII de três floats: `[3,x,y,z]`; índice 0 é contagem.
- Modbus signed16/escala do exercício: converter sinal antes de dividir por 10000.
- Input/output é perspectiva do UR no banco de APIs; conferir direção no PLC.
- Handshake: conexão ≠ dado atual ≠ operação concluída; definir dono/reset/timeout.
- Loopback aponta ao próprio host; não ao outro equipamento.
- 125 Hz CB3, credenciais e mapas antigos não são especificações atuais.

Capítulos por tema no SKILL.md. Divergências: [sources.md](sources.md).


## Complementos dos manuais

| Situação | Decisão / consulta |
|---|---|
| Comparar UR5 e UR5e | Identificar geração e edição; ch22 |
| Configurar célula | Identidade → montagem → carga/interfaces → validação; ch21 |
| Recuperar software | Distinguir arquivo, Support File, backup e imagem; ch23 |
| Planejar inspeção | Plano por componente; Biannually não é dois anos; ch24 |
| C153 ou paradas repetidas | Preservar evidência, investigar carga/contato/montagem; ch25 |
| Atualizar API de payload | set_target_payload em SW5.11, revisar inércia/tara; ch27 |
| Dúvida sobre matrizes | SW5.11 documenta; arrays por valor; ch26 |
| Force limits | Velocidade nos eixos complacentes; desvio nos demais; ch27 |
| Slider PROFINET | O2T bit32=byte4 + máscara SSM; ch29 |
| Int T2O PN | Colunas Byte/Bit inconsistentes; confirmar módulo/GSD; ch29 |
| read_port_register | Resultado já assinado; não converter novamente; ch28 |
| Função socket retornou sucesso | Transporte não confirma ação física; ch28 |

Essas resoluções são da versão documentada. Ver matriz de versões antes de aplicar em outra geração.


## ReferênciasCB3 e artigos históricos

| Pergunta | Resposta dirigida |
|---|---|
| URScript3.4.1 é SW5.11? | Não: manual3.4.1 descreve125Hz/8ms; consultar ch30 |
| EX é código pronto? | Não: revisar aridade, nome, escala e versão; ch30 |
| PalavraModbus32768 ou65535 | Signed16:-32768 ou-1; apenas se vier unsigned e campo exigir sinal; ch31 |
| GPModbus128–256? | TabelaMBS:128–255;256 é versão do controlador; ch31 |
| XYZ400 e offset420 usam mesma escala? | Não: décimo de mm versus mm; ch31 |
| Dashboardload/play retornou | Momento da resposta muda em3.3; não equivale a ciclo concluído; ch32 |
| Corrigir programa após troca de junta | CAL: key-waypoints tem limitações e exige original preservado; ch33 |
| Reload default é restauração de fábrica? | Não, no fluxoCAL por placa; ch33 |
| NúmeroDH nominal corrige o serial? | Não substituir calibração por tabela nominal; ch33 |
| IP54 permite jato/imersão? | O quadro geral não sustenta isso; verificar componente/exposição; ch34 |
| Qual torqueM6UR? | Identificar conexão/modelo/revisão; TQ não é tabela e-Series; ch35 |
| Backup com MagicFile é restauração/upload? | Ações/direções distintas; scripts podem iniciar ao inserirUSB; ch23 |

Referências históricas não foram verificadas como normas/especificações atuais.


## Lote ampliado: diferenças que mudam a aplicação

| Situação | Consulta/decisão |
|---|---|
| Maior carga do catálogo | Aplicar curva/CoG/orientação e verificar etiqueta; ch36 |
| Lista deve crescer | Reservar capacity, conferir tipo e excesso; ch41 |
| a/v deOptiMove | Frações(0,1], não unidades físicas; ch42 |
| direct_torque5.25→5.26 | friction_comp versus escalas viscous/coulomb; ch43 |
| servoj novo | Diretórios5.25/26 documentam t0.002; não migrar legado automaticamente; ch42 |
| is_within_safety_limits | Descrição5.26 difere da notaSM511; ch43 |
| Modbus tem valor apesar de erro | Pode ser último válido dentro do watchdog; checar idade/erro; ch44 |
| CalibrationGo/NoGo | Novosguias1,5/3,5mm;CALCB3antigo2/3mm; ch40 |
| RangeExtender40m | OEMCB5.5+ eUR3e/5e/7e na fonte; ch48 |
| ConnectivityKit | Lista de modelos e5.17+ da fonte; nãoCB3; ch49 |
| IMMIlightcurtain/MAF | Processo, não salvaguarda de pessoal; ch50 |
| Erro de freioURSeries | Pode manter braço energizado e mudar respostaDashboard; ch38 |
| Atualização5.26 | Caminhos sobrepostos conflitantes e backups por versão; ch54 |
| UntilContact100m/s | Errata da fonte; não copiar/corrigir por palpite; ch45 |

Fontes e versões prevalecem sobre esta tabela abreviada. Não houve teste físico ouURSim.
