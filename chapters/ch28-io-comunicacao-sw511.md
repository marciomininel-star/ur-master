# 28 — I/O e comunicação: contratos SW5.11

**Fontes:** SM511 p.21, 34–37, 100–120; U5E p.238. Complementa ch06 e ch14–20.

## Encoder: versão, API e borne

U5E p.238 e SM511 p.35 documentam entradas digitais 8–11 e decodificação até 40 kHz no contexto e-Series. encoder_enable_pulse_decode recebe encoder_index,decoder_type,A,B; encoder 0 ou 1. Tipos incluem quadratura e bordas num canal. Frequência máxima não garante qualquer combinação de encoder/cabo/velocidade.

conveyor_pulse_decode antiga está obsoleta e usa outra faixa. Não misturar números antigos/CB3 com nova API. SM511 p.35 tem exemplo de cinco argumentos para assinatura de quatro: conferir assinatura na versão alvo. Definir escala, sentido, captura de referência e rollover antes de converter ticks.

## Bancos de registradores

| Banco GP SW5.11 | Faixa | Reserva |
|---|---|---|
| boolean input/output | 0–127 | 0–63 Fieldbus/PLC; 64–127 RTDE |
| integer input/output | 0–47 | 0–23 Fieldbus/PLC; 24–47 RTDE |
| float input/output | 0–47 | 0–23 Fieldbus/PLC; 24–47 RTDE |

Entradas/saídas têm memórias distintas. Input entra no robô; não é necessariamente entrada do PLC. Índice URScript não equivale a endereço Modbus ou offset de pacote (p.105–107).

read_port_register retorna inteiro **já com sinal** (-32768..32767), p.108. Conversão uint16→int16 do ch14 vale para palavra bruta sem sinal, não se aplica outra vez aqui. set_digital_out está obsoleta; APIs específicas distinguem standard/configurable/tool. Índices antigos 8/9 da ferramenta diferem de 0/1 nas funções tool.

## Socket: validar contrato

socket_open retorna bool e tem timeout de conexão de dois segundos nesta edição. TCP não preserva fronteiras das chamadas de envio. Validar contagem e formato antes de usar resposta.

socket_read_ascii_float lê até 30 valores entre parênteses, separados por vírgulas; retorna contagem e valores. Falha pode produzir zero e NaN. timeout<=0 bloqueia indefinidamente. socket_read_binary_integer usa int32 em ordem de bytes de rede; conferir contagem e falha. byte_list tem outro contrato.

socket_read_line está obsoleta: socket_read_string oferece prefix/suffix e interpret_escape para newline e outras sequências. Definir consumo de sobras e mensagens concatenadas. socket_send_line acrescenta newline; socket_send_string não. Envios byte/int/line/string retornam bool de sucesso em SW5.11, esclarecendo a simplificação do treinamento. Sucesso de transporte não confirma ação física; exigir resposta da aplicação.

socket_get_var é protocolo GET com resposta nome/inteiro, não leitor JSON genérico. rpc_factory exige método, assinatura e endpoint, inclusive caminho /RPC2 quando o servidor o usa. XML-RPC bloqueia o chamador e indisponibilidade/erro de método pode interromper programa (p.21,108).

## Watchdog

rtde_set_watchdog monitora atualização da entrada, com ação ignore/pause/stop. Um watchdog por pacote pode bastar; removido quando programa para (p.109). Definir frequência e retomada conforme processo. Não é função de segurança.

**Exercício:** contrato de visão com unidade/frame, formato, contagem, ID, timeout e resposta inválida. Dados incompletos ou antigos não devem gerar alvo novo.


## Complemento CB3 e artigos de referência — 2026-10-28

MBS descreve mapaModbus CB2/CB3, não índices GP/PN. SM341 também define read_port_register signed e envios com retorno bool. Ch30/ch31 distinguem APIs por versão; não acrescentar parâmetros de timeout novos a chamadas legadas sem suporte.


## Complemento do lote ampliado — 2026-10-28

N10/N26 incluemFlexibleEtherNet/IP,Modbusmúltiplo/FC23/diagnóstico eF/Texterno. No watchdogModbus, leitura pode retornar último valor válido enquanto há erro. Verch43/44.
