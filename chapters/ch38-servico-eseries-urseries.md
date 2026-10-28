# 38 — Service Handbooks5.25/5.26 e Safe Brake System

**Fontes:** N08 e-SeriesSW5.26/10.12, N24 e-SeriesSW5.25; N09UR SeriesSW5.26/10.12, N25UR SeriesSW5.25. N08/N09 não são duplicatas dos arquivos sem(1). O títuloUR Series não inclui automaticamente CB3.

## Seleção do procedimento

Escolher família → controlador → software → componente/serial → tipo de conexão. N08 abrangeUR3e/5e/7e/10e/12e/16e e caixa padrão/OEM; N09 identifica controlboxes5.5/5.6. Não transportar torque, junta ou imagem entre esses grupos.

N08 p.31–48 distingue conexões por parafuso e clamp, conectores e flange. O procedimento pode exigir torque diferente para parafuso regular e escareado; “torque da junta” sem especificar conexão é insuficiente. N09 p.28 permite intercâmbio somente nos grupos declarados: UR20/UR30 entre si e UR8Long/UR15/UR18 entre si, com wrist1/wrist2 como posições intercambiáveis de serviço. Isso não torna todas as juntas do grupo idênticas: conferir partnumber e procedimento.

## Inspeção por família

N08 p.15 e N09 p.15 foram conferidas visualmente. A tabelae-Series inclui tampas/parafusos e junções; aUR Series inclui round sling e não repete todas as linhas. N09 marca inspeção funcional de sling anual e fixações do braço/ferramenta mensais; demais itens visuais listados são semestrais. Não copiar o planoe-Series inteiro paraUR Series.

Os procedimentos visuais/funcionais separam desligamento, estabilidade, fixações, cabos, limpeza e testes de segurança. Atualizar o plano por modelo e condição ambiental sem interpretar “biannually” como dois anos. Conservar distinção entre limpar filtro removido e aplicar ar comprimido à eletrônica.

## Verificação e calibração após serviço

N08 p.49–52 distingue Joint VerificationPolyScope 5 eX. Robot Type Validation, zeroing e calibração têm objetivos distintos; não executar verificação como se fosse correção de todos os parâmetros. Preservar dados do serial e evidências do que foi trocado. Guias de substituição e calibração complementam o handbook; ver ch39/40.

## Safe Brake System: falha muda a resposta operacional

N09 p.85–86 descreve detecção de engate de freio no shutdown, violation ouE-stop. Se falhar, motores podem permanecer energizados para sustentar o braço. Portanto, tela de falha não garante ausência de energia e retirar alimentação pode permitir queda do braço.

N09 p.86 também diz que Dashboard responde às consultas com informação de erro de freio, fora do contrato usual. Parser deve reconhecer essa situação como estado de falha, não repetir comandos até obter a string esperada. Identificar junta e log, proteger pessoas/equipamento e encaminhar serviço/validação do freio. Não transformar procedimento de limpar erro em recuperação automática; a fonte restringe liberação de falha quando há freio defeituoso.

## Navegação e entrega

N08: inspeção15–24; serviço25–84; software85–110 aproximadamente, seguido de troubleshooting. N24 tem outra paginação: não somar deslocamento fixo para migrar citações. N09: intercâmbio28, torques29–30, software57–73 e freios85–89. N25: versões anteriores do fluxo com paginação própria.

**Método:** preservar evidência → confirmar família/peça → procedimento específico → verificação → calibração quando aplicável → testes de segurança/liberação. A inclusão na skill não habilita acesso de serviço, movimento ou alteração persistente.
