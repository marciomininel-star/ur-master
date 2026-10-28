# Aplicabilidade e divergências

Use quando fontes discordarem ou uma resposta envolver geração, software, dimensão, segurança ou API.

## Prioridade de evidência

Escolher primeiro o mesmo modelo/controlador/software. Dentro desse contexto, manual aplicável prevalece sobre slide de treinamento simplificado. Uma edição mais nova de outro modelo não prevalece automaticamente. Se o manual contiver inconsistência interna, declarar a inconsistência e confirmar no sistema/documentação aplicável antes de implementar.

| Tema | Evidência nova | Decisão |
|---|---|---|
| Matrizes | SM511 p.16–17 | Existem em SW5.11; não universalizar rascunho antigo |
| Distância | SM511 p.85–86 | point_dist ignora rotação; pose_dist considera |
| Payload | SM511 p.66–69, DEP21–22 | set_target_payload e cargas nomeadas; API antiga obsoleta |
| Encoder | SM511 p.34–35, U5E238 | Separar API legada e nova; e-Series8–11/40kHz |
| Simple | U5E207 | Z da feature, Y para linha |
| Force/Offset/Conveyor | U5E206, SM51146 | Não combinar no escopo da edição |
| Partida | U5E233–234 | Nível já presente pode iniciar sem nova borda |
| Bancos GP | SM511105–107 | Fieldbus e RTDE têm reservas por tipo |
| PN slider | PN8 | Bit32/byte4; não índice de intregister |
| PN ints | PN6 | Byte284 contradiz Bit2304; derivação288 exige confirmação |
| Inspeção | SVC4 14,16–17; U5E69–70 | Biannually interpretado semestral; “Bienal” é ambíguo |
| Popup | SM51165–66 | blocking=False por padrão |
| Socket | SM511114–120 | Bool de envio; line obsoleta; contratos por tipo |

## Erros documentais que não viram exemplos executáveis

- SM511 p.35: cabeçalho encoder_enable_pulse_decode com quatro parâmetros, exemplo com cinco.
- SM511 p.39: exemplo force_mode com pose de cinco componentes e descrição de limite divergente.
- SM511 p.132–133: lista gráfica pode omitir sexto argumento de mc_add_circular; exemplo comenta r=0 e 50mm de modo inconsistente.
- PN p.6: colunas Byte/Bit dos ints T2O diferem em quatro bytes.
- IMG: grafias de arquivos de backup exigem conferir nomes reais.
- MAINT: captura tem navegação e caracteres corrompidos. Não usar esses fragmentos como comandos.

## Segurança histórica

SF3 é tabela Generation3/CB3 de 09/05/2017. SAFE19 de 08/03/2019 diferencia CB3 e e-Series. Não transportar PL/categoria/PFHd de uma geração à outra. Categoria0/1/2 de parada IEC60204-1 descreve remoção/retensão de potência e controle da parada; categoria de arquitetura ISO13849 é outro atributo. Certificação do robô não certifica integração.

As referências a ISO10218, ISO13849, ISO/TS15066 e regras legais nas fontes pertencem àquelas datas. Para exigência vigente ou cálculo de conformidade, consultar fonte oficial atual aplicável e separar do corpus. Não assumir limiar universal de contato ou reutilizar o procedimento antigo de elevar limites até parar de falhar.

## Identificação

UR5DS: UR5/2016. U5E: UR5e/copyright2021. SVC4: e-Series rev4.0.0. SM511: e-Series SW5.11. VM: VirtualBox6.1.6/PolyScope5.8/abril2020. UR10DWG: CB3.0/3.1/2015. OEMDWG: caixa OEM AC/DC/2019. TPIMG: modelo não confirmado. Fontes detalhadas em [sources.md](../sources.md).


## Complemento legadoCB3 e artigos

| Fonte/tema | Aplicabilidade e decisão |
|---|---|
| SM341 | Versão3.4.1/03abr2017;125Hz e faixasGP antigas. Não projetar recursosSW5.11 nesse contexto |
| EX | Sem versão formal; coleção incompleta com erros. Usar para estudo/revisão, não autoridade de assinatura |
| MBS | CB2/CB3, mapaWord/bit; signed16 e intervaloGP têm erratas preservadas em ch31 |
| DBS | CB2/CB3; confirmação load/play/stop/pause muda em3.3; não generalizar a e-Series |
| CAL | ManualCB3v1.0.0; placa,dualrobot e correção de programa distintos; sem extensão automática a e-Series |
| IPG/IPD | Duplicatas de referência Holland Shielding Systems/2017; não certificam proteção de produtosUR |
| TQ | ModelosUR3/5/10 sem revisão confirmada; conferir conexão e manual aplicável; não usar em e-Series por analogia |
| MAG | Artigo2013,controllerbox2.0 e previsão.vars para1.8; não é matriz atual de suporteMagicFiles |

CAL p.18 imprime endereços com cinco grupos numéricos: não fornecê-los comoIPv4 válido. CAL p.16 descreve preset de bancada e p.12/25 descreve reset persistente; são conteúdo técnico contextual, não ações autorizadas por uma consulta à skill.

MBS p.2 sugere limiar>32768/subtração65535; a reconstrução signed16 usa>=32768/subtração65536. Explicitar a correção e não reconverter read_port_register já assinado. EX p.26 usa4 onde saída analógica espera fração[0,1]; conferir domínio e converter unidade fisicamente, em vez de repetir o exemplo.


## Lote ampliadoN01–N38

- N03/N07 são duplicatas; os demais nomes com(1) podem ser revisões diferentes.
- N10/N26 sãoScriptDirectory5.26/5.25: direct_torque muda assinatura/modelo de atrito;servoj está documentado com t0.002. Não retroagir aSM341/SM511.
- N10p.82 documenta is_within_safety_limits comqNear e limites ativos, distinto da obsolescência registrada emSM511. Alcançabilidade e validação de trajetória permanecem conceitos distintos.
- N11/N23 usamGo1,5/NoGo3,5mm;CALCB3 usa2/3mm. Kit/guia precisam coincidir.
- N12p.248 descreve atualizaçãoURUPCB5.5/anterior para5.26 e incompatibilidade de backups pré5.25;N08p.86/N09p.58 repetem caminho geral incremental antigo. Conflito preservado emch54, sem planoexecutável decidido automaticamente.
- N12p.117 imprime100m/s no exemplo de reduzir velocidadeUntilContact. Página conferida; não converter silenciosamente a100mm/s.
- N09p.28 restringe intercâmbio de juntas aos gruposUR20/30 eUR8Long/15/18, com posições específicas;N21UR20 não substitui tabelaN09para modelos menores.
- N15RangeExtender40m limitaUR3e/5e/7e,OEMCB5.5+;N17proíbe emendar cabos comuns. Sistema ativo não contradiz cabo passivo.
- N14ConnectivityKit limita a listae-Series/UR20/30 e5.17+ na edição. Não ampliar a modelos não nomeados por inferência.
- N28/N30/N32 trazem cargaexpandida condicionada;N35UR10e pede etiqueta10/12,5kg. Não inferir capacidade do nomeUR15/UR7e.
- N37fixação20Nm difere deU5Eantigo10Nm;conexão/revisão precisam ser resolvidas antes de aplicação. Não atualizar montagem física por simples leitura da skill.

Navegação e hashes em sources.md e source_manifest.json. Documentos fornecidos não foram verificados como release/norma vigente online.
