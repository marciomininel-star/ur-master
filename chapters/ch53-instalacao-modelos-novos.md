# 53 — Montagem, ambiente e comissionamento dos modelos do lote

**Fontes:** User ManualsN28–N38, capítulos3–9; CB3N01/N02/N06; N16OEM. Páginas físicas abaixo, distintas dos números impressos. Este capítulo organiza decisões; não substitui desenho/fixação do serial.

## Fixação: evidência da edição

| Modelos/fontes | Base segundo seção consultada | Página física |
|---|---|---|
| UR3e N38 | QuatroM6classe8.8,9 Nm | 40 |
| UR5e/7e/10e/12e/16e N33–N37 | QuatroM8classe8.8,20 Nm | 40 |
| UR18 N29 | SeisM10classe8.8,45 Nm | 41 |
| UR8Long/15/30 N32/N31/N28 | SeisM10classe8.8,45 Nm | 42 |
| UR20 N30 | SeisM10classe8.8,45 Nm | 44 |

A tabela identifica somente conexão da base descrita; não torques internos, de flange ou do dispositivo de calibração. A edição antigaU5Ep.42 citava10 Nm paraUR5e: agora há evidência diferente emN37. Preservar revisão e confirmar ativo/procedimento antes de intervir, sem mudar parafuso de máquina instalada apenas porque apareceu outro número.

N29 p.41 registra atualização de valores a partirSW5.18 e avisa que impressos anteriores mostram outros valores. Não generalizar o ano/software como regra mecânica para qualquer produto. A escolha depende do conjunto e orientação de montagem,classe/comprimento de parafuso e superfície.

## Suporte e manuseio

Carga nominal na flange não dimensiona a mesa. Considerar momentos de base, dinâmica, fixação, nivelamento, rigidez, vibração e comprimento do braço. Modelos grandes incluem içamento e sling específicos; não reutilizar gesto manual doUR3 para63–64 kg de braço. Posição de transporte e ferramenta instalada mudam o centro de massa.

UR20/UR30 têm alcance/massa diferentes mesmo quando compartilham partes. Um suporte que acomoda furação não comprova rigidez ou resistência. Preservar espaço para raio de cabos, serviço e circulação de ar.

## Ambiente por componente

N28–N32 p.14 discriminamIP65 no braço,IP44 na caixa eIP54 no pendant, no escopo dessas edições. E-SeriesN33–N38 possui tabelas próprias. Classificação do braço não autoriza jato na caixa nem remove requisito de gabineteOEM. Temperatura acima35°C pode reduzir desempenho conforme tabelas; não assumir que operar dentro da faixa0–50°C preserva velocidade nominal.

Consultar curva de payload/CoG, limites de junta, ferramenta e cabos em toda trajetória; verificar singularidades, superfícies, peça e movimento após reconexão/partida. EndEffectorIntegrationN28p.94–96/N30 p.97–99/N32 p.96–98 detalha carga expandida; verch36.

## Matriz de comissionamento

Identidade/revisão → montagem/cabos → energia/PE → ferramenta/TCP/carga/inércia →I/O/handshake → modos/segurança → movimento/processo → retomada → evidência. Registrar ensaio separado da montagem correta: torque aplicado não valida geometria, e programa que roda não valida parada compartilhada com outra máquina.

**Exercício:** reaproveitar mesaUR5e paraUR20. Resultado: exigir cálculo/compatibilidade de esforços e fixação, envelope e içamento; sem extrapolar pela semelhança do controlador.
