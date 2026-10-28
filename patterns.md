# Padrões reutilizáveis — UR_Master

Fontes e exemplos nos capítulos. Os complementos SW5.11 têm precedência sobre simplificações do treinamento quando aplicáveis àquela versão.

| Quando | Método | Trade-off / limite | Capítulos |
|---|---|---|---|
| Ciclo com máquina/inspeção | Fluxograma de estados, sinais e exceções; confirmar estado físico antes do contador | Planejamento não substitui interface da máquina | ch01 |
| Lógica repetida | Função para argumentos/retorno, SubProg para sequência, Folder para agrupar | Escopo e versão precisam ser explícitos | ch02/12/26 |
| Câmera fornece pose local | Converter unidades e compor pose_trans(frame,local) | Depende de frame calibrado e aproximação válida | ch03 |
| Mesma trajetória em várias peças | Feature variável, origem preservada e offsets locais | Requer retirada e restauração do frame | ch04 |
| Pegou/soltou peça | Carga por estado: TCP, massa, CoG e inércia separados | Configurar não comprova peça presente | ch05/27 |
| Esteira móvel | Capturar referência no gatilho, conferir escala/direção e tracking | Encoder/API/borne dependem da versão | ch06/28 |
| Mudar paletização/despaletização | Fixar modo no BeforeStart ou aceitar alteração após ciclo | Resposta à seleção versus consistência da transferência | ch06 |
| Contato complacente | Definir frame, eixo, sinal, limites e saída; medir separadamente | force() perde direção; thread não é segurança | ch07/27 |
| Retângulo alinhado sem rotação | Quatro contatos, médias de opostos com simetria da ponta | Deflexão/assimetria afeta centro; não estima orientação | ch07 |
| Reensinar feature/TCP | Revisar cópia em Safety, esferas, eixos e lado permitido | Geometria de processo pode mudar sem atualizar cópia | ch08 |
| Reduzir ciclo | Medir base, separar transferência/contato, escolher movimento e blend | Blend desvia do ponto; aceleração pode trazer pouco ganho | ch09/21 |
| Iniciar/retomar | Conferir instalação, ocupação, peça e pose atual; afastamento planejado | Evitar Auto-Move não garante caminho livre | ch10/03 |
| Buffer intermediário | Pattern para posições, PalletSequence para ação, política cheio/vazio | Estado lógico deve corresponder à ocupação real | ch11 |
| Inspeção/limpeza por contagem | Incrementar no evento de conclusão definido; validar seleção | Índice do Loop não é contagem de sucesso | ch12 |
| Avaliar implantação | Dez dimensões, evidências, pendências e ensaios | Triagem não garante viabilidade ou payback | ch13/21 |
| Investigar versão/parada | Preservar programas/configurações/logs antes de alterar | Logs não comprovam segurança física | ch13/25 |
| Receber dados externos | Contrato de produtor, banco, tipo, escala, frame e atualidade | Link ativo não prova dado novo | ch14/16/17/19 |
| Coordenar visão ou máquina | Pronto → disparo → dados → confirmação, com donos e resets | Exige timeout, vínculo à peça e tratamento de reconexão | ch14/16/17 |
| Implantar remotamente | Separar transferência, carregamento e execução; consultar estado | Upload/resposta não comprova resultado físico | ch15/18 |
| Escolher interface | Modbus/fieldbus para bancos, Dashboard para operação, socket para protocolo, cliente para script, RTDE para campos, RPC para métodos | Papéis, formatos e falhas não são intercambiáveis | ch14–20 |
| Enviar setpoints RTDE | Recipe/tipos/direção, registradores reservados e fase do handshake | Envio não gera movimento por si nem sincronização física certificada | ch19/28 |
| Chamar cálculo RPC | Endpoint, método registrado, assinatura, retorno e repetição | Chamada bloqueia e falha pode parar programa; não é tempo determinístico | ch20/28 |
| Preparar recuperação | Identidade → inventário de backup → mídia/imagem → restauração → ensaio | Support File não é imagem completa; gravação destrói destino | ch23 |
| Planejar manutenção | Ativo + item V/F + intervalo + condição + instrumento + registro | Tabela histórica requer versão/qualificação aplicáveis | ch24 |
| Tratar parada protetiva | Evento → evidência → causa → intervenção → validação/reset deliberado | Não automatizar desbloqueio nem mascarar com limites maiores | ch25 |
| Migrar código URScript | Conferir semântica, escopo, aridade, unidades e API por versão | Transcrição do PDF pode conter erros da própria fonte | ch26–28 |
| Mapear PROFINET | Direção → módulo → base PLC → offset local → tipo → máscara → handshake | PN6 tem divergência Byte/Bit que exige confirmação | ch29 |


## Padrões das referências complementares

| Quando | Método | Limite | Capítulos |
|---|---|---|---|
| Migrar exemplo legado | Intenção → assinatura → domínio → estado → retorno | EX tem chamadas/escala incorretas; validar versão | ch30 |
| Interpretar Modbus | Papel → função → base → endereço → R/W → sinal/escala/frame | Word/bit eGP/offset são espaços diferentes | ch31 |
| Controlar via Dashboard | Parser por versão, resposta e consulta de estado | Resposta de início não confirma ciclo; não repetir comando físico às cegas | ch32 |
| Preparar calibração | Objetivo → método/ferramental → coleta/avaliação → validação → persistência | Backup próprio, original preservado e procedimentos qualificados | ch33 |
| Avaliar proteção/torque | Identificar componente/conexão, revisão e condição | GrauIP e tamanho de parafuso não bastam | ch34/35 |


## Lote ampliado

| Quando | Método | Limite | Capítulos |
|---|---|---|---|
| Selecionar modelo | Identidade → curva/CoG → orientação → dinâmica → integração | Nome comercial e carga máxima não bastam | ch36/53 |
| Serviço de junta | Família/serial → conexão/peça → procedimento → verificação/calibração | Não transferir torque/clamp entre grupos | ch37–40 |
| Migrar URScript | Edição → assinatura → tipo/capacidade → unidade → estado/retorno | direct_torque e servoj mudam por versão | ch41–44 |
| Configurar movimento | Caminho → perfil/unidades → blends → medição → validação | OptiMovepercentual não é a/v físico | ch42 |
| Conectar acessório | Produto/modelo/controlador/versão → topologia → falha/retomada | Cor de cabo, porta ou protocolo não prova compatibilidade | ch47–50 |
| Aparafusar | Reação/alavanca → pose/carga → steady/estratégia → OK/NOK | Nm da ferramenta não é capacidade genérica do braço | ch51 |
| Investigar erro | Código/subcódigo → contexto → página → evidência → hipótese | Índice de ocorrência não é reparo automático | ch52 |
| Atualizar software | Hardware/origem/destino → conflitos → backups → ensaio/recuperação | Backups pré5.25 versus5.26 ePolyScopeX diferem | ch54 |
