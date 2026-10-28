# 49 — Connectivity Kit: compatibilidade e rede planejada

**Fonte:** N14ConnectivityKit38p. Produto específico, não manual genérico de routerTeltonika nem autorização para conectar a internet.

## Portas e propósito

N14 p.12 restringe a e-Series/UR20/UR30 comPolyScope 5.17 ou posterior e proíbeCB3/software anterior. O nome de modelo mais novo não amplia essa lista por inferência; confirmar revisão do produto quando não nomeado.

P.19 descreveRUT951 do kit eVLAN para até dois dispositivos nasLAN2/LAN3 comunicarem entre si e com robô sem internet. Isso não significa que qualquer equipamento ligado a qualquer porta fique isolado. Definir portas, rede industrial existente, endereços, rota e dono do dado.

## Restrições fazem parte do produto

N14 p.21 informaWiFi/RMS/VPN/SSH/CLI desativados, sem conexões de entrada e saída restrita a443 e endereços controladosUR, além de proteções. Não usar a presença de antenaWiFi como prova deWiFi habilitado nem transformar o kit em acesso remoto genérico por liberar portas.

P.22 distingue firmware customizado do kit de umRUT951 avulso. Reset do kit restaura restrições e credencial padrão física; não é remediação neutra e pode alterar disponibilidade/configuração. Mudanças exigem análise de impacto e pessoal responsável conforme a fonte; a consulta à skill não executa reset, login ou exposição de credenciais.

## DHCP e integração com rede existente

N14 p.30 descreve configuração emLocalControl eDHCP para reconhecimento automático. SelecionarStatic desativa configuração automática e pode criar conflitos. Antes de implementar, mapear redePLC/PC, subnets e rotas; DHCP não é substituto de planejamento. Se cabeada em vez de4G, p.31 apresentaWAN e condição relativa aoSIM; seguir fluxo do produto, sem combinar modos por palpite.

## Diagnóstico em camadas

N14 p.32: dependências/URCap →TCP443 →DNS/serviçoIoT → aplicação → armazenamento. Falha de serviço não é prova de cabo defeituoso;ping não valida443/DNS ou autenticação. Coletar estado e mensagens sem enviar dados do ativo a destinos externos por iniciativa da skill.

Registrar topologia, modelo/versão, necessidade de serviço online, política de dados, responsáveis e critérios de desconexão/retomada. OsFQDNs do documento são referências daquela edição, não endpoints garantidos atuais. Se usuário pedir implantação atual, verificar documentação de serviço vigente.

**Exercício:** distinguirConnectivityKit, canalPLC local e conexãoDashboard; resultado: três finalidades/contratos distintos, com portas e efeitos explicitados.
