# 33 — Calibração cinemática CB3 e correção por key-waypoints

**Fonte:** CAL, Kinematic Calibration Manual for CB3 v1.0.0, copyright2009–2016,35p. No corpo: página física=impressa+2. Não é procedimento e-Series nem calibração cotidiana de TCP.

## Escolher o problema correto

| Necessidade | Método da fonte | Limite |
|---|---|---|
| Identificar parâmetros com referência de posições conhecidas | Plate Calibration, p.3–12 | Não identifica todos os parâmetros |
| Calibração cinemática com cadeia fechada | Dual Robot Calibration, p.13–25 | Par do mesmo tipo/versão, kit e controladorCB3 |
| Adaptar programa após troca de braço/junta | Program Correction by Key-waypoints, p.26–32 | Corrige programa/modelo por instalação, não equivale a recalibrar braço |
| Definir ferramenta ou peça | TCP/feature, ch03–05 | Não substitui identificação cinemática |

A finalidade desta síntese é preparar e avaliar serviço. A presença no manual de acesso Expert Mode, mudança de preset, movimento coordenado, gravação ou reset de calibração não constitui pedido para executá-los. Preservar configuração e procedimento aplicável antes de qualquer intervenção solicitada.

## Plate Calibration: distribuição e validação separada

CAL p.3 assume fixação repetível da flange paralela a placa rígida com posições conhecidas. P.7 recomenda pelo menos30 posições por placa numa calibração completa, distribuídas na placa e no espaço articular, incluindo soluções de cotovelo/punho. O exemplo posterior usa26; não converter tamanho de exemplo em recomendação mínima.

P.9 distingue Offset angles (ombro, cotovelo, wrist1 e wrist2; não base/wrist3) e Partially, ainda limitado pela observabilidade do método. Não prometer precisão absoluta integral por mudar o nível de ajuste.

P.10–11 separa Measurement data e Evaluation data. Compare média, desvio padrão, máximo e Q range; diferença grande entre ajuste e avaliação indica problema. Verificar coordenadas e contribuição de cada posição; acrescentar variedade quando configuração for monótona. Bom erro nos pontos usados no ajuste pode ocultar modelo fraco fora deles.

A coleta usa salvamento próprio da calibração; o menu de salvar programa não o substitui (p.7). Use Calibration é ensaio temporário; Save as default persiste; Reload default recarrega o default atual, **não** calibração de fábrica (p.11). Reset descrito p.12/25 altera ou remove dados persistentes: não tratar como limpeza de rotina nem fornecer ação automática para zerar arquivos.

## Dual Robot: cadeia fechada e evidências de aceitação

CAL p.13–15 requer par UR3/UR5/UR10 do mesmo tipo/versão, CB3 e kit185500 conforme edição. Bancada, bases e Tool Connector definem distâncias conhecidas. Existe exceção de pino de alinhamento para UR5 anterior ao fim de2014 (p.14). Confirmar ferramental/revisão em vez de improvisar acoplamento.

Há movimentos coordenados e início automático de coleta após confirmação de montagem (p.20–22). Rede Master/Slave pode configurar IP automaticamente; o texto p.18 imprime endereços com cinco grupos numéricos, inválidos comoIPv4. Não copiar esses endereços nem corrigi-los por palpite. A proposta precisa de rede de serviço apropriada e configuração verificada.

CAL p.16 orienta preset menos restritivo no procedimento histórico. Isso depende da bancada/procedimento específico e não se torna regra de ajuste da célula. Não recomendar esse preset como solução genérica para falha de calibração ou parada.

Critérios históricos p.23: média<1mm e<2mrad; desvio padrão<0,5mm e<1mrad; diferença entre Calibration e Control results não maior que50%. Esses valores são do ensaio descrito, não tolerância universal de processo/repetibilidade.

Há ainda validação física Go/NoGo: distância2,5±0,5mm, ferramentaGo2mm passa e NoGo3mm não passa, com flanges livres de conector/pinos; seguida da verificação de Home (p.23–24). Resultado estatístico aprovado não substitui validação mecânica. A persistência pode ocorrer conforme opções Save calibration/Correct home position p.19; não presumir que nada foi alterado até sair da tela.

## Key-waypoints: corrigir sem perder original

CAL p.26 pede backup do programa antes de corrigir: depois de corrigido e salvo, ele não pode ser novamente corrigido pelo mesmo fluxo. Modelo específico por instalação, qualidade dependente dos key-waypoints e proximidade dos alvos. Melhorar modelo e reaplicar a partir do original preservado, salvando resultado com outro nome.

Limitações da edição: apenas waypoints fixos; não cobre BeforeStart, Move com feature diferente da Base nem Move que seleciona TCP. Esses nós podem precisar correção manual. CTP (Corresponding Tool Position) pode variar por key-waypoint e representar ponto de contato da ferramenta/peça; não confundir com um TCP global único (p.29).

## DH e limites de transferência

CAL p.35 lista parâmetros Denavit-Hartenberg nominais, com tabelas distintas para UR5 séries1/2/3. Não substituir parâmetros calibrados do serial pela tabela nominal nem usar essa geometria como UR5e. CAL p.33–34 contém desenhos do ferramental: dimensões extraídas linearmente não bastam para fabricar um dispositivo de precisão.

**Entregável:** identidade/versão, objetivo, método escolhido, ferramental, backup, conjunto de medições/avaliação, estatísticas com unidades, validação física e destino dos dados persistidos. Ensaios e movimentos exigem contexto de serviço real; esta incorporação não os executa.


## Complemento do lote ampliado — 2026-10-28

N11/N23 apresentam calibração moderna5/X eGo1,5mm/NoGo3,5mm, distintos doCALCB3Go2mm/NoGo3mm. N11 forneceIPv4válidos no procedimento de rede direta. Não usar ferramentas/critério antigo com novo kit. Verch40.
