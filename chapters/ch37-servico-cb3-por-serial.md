# 37 — Serviço CB3: escolher procedimento por serial

**Fontes:** N03/N07UR10, N04UR3 e N05UR5, revisões3.2.7; N01/N02/N06 manuais de usuárioCB3. N03/N07 são duplicatas exatas. Não confundir revisão do documento3.2.7 com versãoPolyScope.

## Roteamento de hardware

| Manual | Limites de serial declarados na capa p.1 |
|---|---|
| N04UR3 | CB3.1:2014330001–2019330944; CB3.2 desde2019330945 |
| N05UR5 | CB3.0:2014350001–2016351863; CB3.1:2016351864–2019351625; CB3.2 desde2019351626 |
| N03UR10 | CB3.0:2014300001–2016301178; CB3.1:2016301179–2019301731; CB3.2 desde2019301732 |

N03 também traz uma linha adicional “Valid from” que não coincide com o primeiro limiteCB3.0. Preservar a inconsistência e confirmar identidade/hardware; não decidir revisão apenas pela linha isolada. Substituições anteriores podem ter alterado a combinação de componentes.

## Diagnóstico e serviço como fluxo

1. Identificar modelo, serial, controlador e alterações anteriores.
2. Preservar programa/instalação, calibração e evidências; registrar código/subcódigo e estado de carga.
3. Distinguir falha de interface, montagem/carga, programa, comunicação e componente.
4. Localizar procedimento do modelo/revisão, com peças e ferramentas correspondentes.
5. Após reparação, executar verificações e validação aplicáveis antes de liberar produção.

Os manuais incluem inspeção, ESD, desmontagem, juntas, zeroing, calibração, controladores, software, códigos e peças. Presença desses procedimentos não solicita execução. Trocar uma junta sem observar orientação, cabeamento, torque da conexão e validação pode criar problema diferente do inicial.

## Navegação útil

Inspeção do controlador: N03/N04/N05 p.9 em diante. Inspeção do braço: N03/N04 p.16–17; N05 p.16–17 possui duas subseções próximas, devendo conferir tabela e contexto. Torques de juntas: N03 p.26–27, N04 p.30, N05 p.28. Não aplicar tabelaUR10 aoUR3 por semelhança do punho.

Manuais de usuárioN01/N02/N06 incluem requisitos mecânicos/elétricos, parada e segurança da geração. Suas especificaçõesCB3p.90 não substituem manuale-Series ouUR Series. A seção de esquema/peças no serviço é um índice histórico, não disponibilidade atual de compra.

## Decisões que evitam erro

Não tratar backup de programas como calibração restaurável; não usar pacote de outra família para corrigir firmware. Zeroing ajusta referência articular; calibração cinemática e correção de programa são outros processos. “Robô parou de falhar” não comprova reparo se proteção foi afrouxada ou diagnóstico apagado.

**Entregável:** ficha de identificação, hipótese e evidência, capítulo de procedimento, peças/revisão a confirmar, dados preservados e critérios de liberação. Quando houver conflito entre a capa, componente instalado e registro de manutenção, resolver a identidade antes de recomendar intervenção.
