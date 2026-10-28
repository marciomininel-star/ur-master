# 45 — Software HandbookPolyScope 5: operação e programação5.25/5.26

**Fontes:** N12SW5.26 e N27SW5.25. Ambos têm257p, mas conteúdos/paginação diferem: não são duplicatas. Referências abaixo usamN12 quando não indicado.

## Carregar a parte certa do manual

Os novosUser Manuals se concentram no robô/hardware; oSoftware Handbook organiza GUI, segurança, programação, instalação e configurações. Pergunta de montagem exige manual do modelo; pergunta de nó exige edição do handbook e versão efetiva do controlador. PolyScope X precisa de fluxo próprio quando descrito; uma imagemX noService Handbook não torna aGUI5 equivalente.

## Programa por caminho e estado

N12 p.90–92 diferenciaMoveJ/L/P eOptiMove. “Use joint angles” emMoveJ conserva valores articulares na transferência de programa e desabilita opçõesTCP/feature nesse fluxo; não conserva automaticamente o mesmo ponto cartesiano em outro braço/calibração. Waypoints, blends e estados de peça continuam exigindo revisão.

N12 p.135 descreveLoop ePayload Transition Time: oLoop contado vai0 aN-1; expressão continuamente avaliada pode interromper trecho interno. Transição de carga ocorre enquanto programa continua. Não usar contador como confirmação de peça, nem inserir movimento que presume payload já estabilizado sem condição adequada.

## Contato: fonte também pode ter errata

N12 p.117 recomenda reduzir velocidade antes deUntil Tool Contact, mas imprime exemplo “100m/s”, incompatível com a intenção e capacidades do contexto. A página foi renderizada e a unidade problemática é da própria fonte. Não copiar o valor, não corrigi-lo silenciosamente para100 mm/s como se estivesse confirmado. Fornecer princípio e solicitar/consultar valor da versão/aplicação quando necessário. Vibração da ferramenta pode impedir detecção correta;Until contact não é proteção de pessoal.

## Estado operacional e controle remoto

N12 p.245: habilitarRemoteControl nasSettings apenas disponibiliza a escolha; selecionarRemote é outra ação. Sistema desligado emRemote pode voltar nesse modo. P.246 restringe ações locais/remotas: Local não permite determinados comandos de rede/autostart;Remote restringeMoveTab, início e carregamento pelo pendant eFreedrive. Não inferir restrição de consulta de estado a partir da restrição de comando.

Home eSafeHome são diferentes: N12 p.191 usaHome definido pelo usuário e referência à configuraçãoSafeHome. ReensinarHome exige revisar a relação com a função de segurança e outros pontos de processo; caminho atéHome não é certificado só pelo nome.

## Configurações e evidência

N12 p.190–191 separaSmooth Transition, Motion Version eMotionLimits; são ajustes distintos. Atualização pode alterar movimento; preservar instalação e validar processo após mudança. Backup e caminhos5.26 emch54; segurança/3PE emch46; listas/API emch41–44.

**Exercício:** distinguir edição doUser Manual,Software Handbook eScript Directory ao revisar uma rotina; cada recomendação deve trazer modelo, versão, nó/API e estado físico esperado.
