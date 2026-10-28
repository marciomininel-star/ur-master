# 29 — PROFINET: direção, bits, bytes e máscaras

**Fonte:** PN p.1–11. Confrontar mapa histórico com GSD, revisão, módulos habilitados e configuração do PLC; não aplicar a EtherNet/IP.

## Três coordenadas diferentes

T=Target/robô; O=Originator/PLC. T2O leva estado ao PLC; O2T leva comando/dados ao robô. Distinguir offset global do mapa, offset local do módulo e endereço inicial configurado no PLC.

Bit32 começa em byte4: byte=floor(bit/8). Não é integer register32. O exemplo de slider do treinamento refere-se ao deslocamento de pacote. Bit64 começa em byte8 no bloco de máscaras/valores de saída, não é API genérica write_register(64).

## T2O

| Módulo | Tamanho | Base no mapa completo | Conteúdo |
|---|---|---|---|
| UR_1 State | 32 B | 0 | Versão, modo, tempo, corrente, slider/status |
| UR_2 IO | 68 B | 32 | I/O standard/configurable, analógicos, Euromap, ferramenta |
| UR_3 Joints | 104 B | 100 | Posição, velocidade, corrente, temperatura, modos |
| UR_4 TCP | 76 B | 204 | Pose, velocidade, wrench, força escalar |
| UR_5 BitRegisters | 8 B | 280 | Outputs boolean 0–63 |
| UR_6 IntRegisters | 96 B | 288 pela coluna Bit; ver ressalva | Outputs int 0–23 |
| UR_7 FloatRegisters | 96 B | 384 | Outputs float 0–23 |

PN p.6: int output0 tem Byte284, mas Bit2304 equivale a Byte288. As linhas seguintes repetem diferença de quatro bytes. A soma dos módulos anteriores aponta 288: BitRegisters ocupa 280–287. **288 é reconciliação aritmética, não endereço PLC confirmado.** Não corrigir silenciosamente fonte nem entregar pinagem validada; confirmar layout e base efetiva do módulo no projeto/GSD.

PN p.3 usa radianos nas posições articulares; p.4 usa m na posição TCP e N/Nm em força/torque. WORD, DINT e REAL não são intercambiáveis. Conferir representação e ordem de bytes com a configuração; não inferir só por usar Ethernet.

## O2T e máscaras

| Módulo | Tamanho | Base global completa | Conteúdo |
|---|---|---|---|
| UR_8 Robot_IO | 24 B | 0 | Slider, máscaras e saídas |
| UR_9 Reg1 | 100 B | 24 | Input bits0–31; ints0–11; floats0–11 |
| UR_10 Reg2 | 100 B | 124 | Input bits32–63; ints12–23; floats12–23 |

PN p.8: bit0 SSM; byte4 speed slider fraction REAL; byte8 inicia standard output mask, configurable mask e valores digitais. Byte12 tem AOM/AOT/TDOM/TDO; bytes16/20 saídas analógicas normalizadas [0..1]. Máscara seleciona participação no comando, não confirma valor aplicado.

PN p.9: bits0–31 em byte24, ints em28, floats em76. P.10: bits32–63 em124, ints12–23 em128, floats12–23 em176. São offsets do mapa completo; reendereçar módulos exige base PLC efetiva + offset local.

Definir dono da saída, máscara, domínio e handshake com ID/pedido/ocupado/concluído/falha. Planejar desconexão e retomada antes de teste real. Valor escrito não garante ação da ferramenta.

## Flags comuns não são canal seguro

Status de segurança p.1/11 serve para diagnóstico/coordenação comum. Presença no PROFINET não cria comunicação funcionalmente segura nem substitui I/O de segurança.

**Exercício:** esclarecer “slider no register32”: O2T, bit32/byte4, REAL, SSM, módulo/base PLC e diferença do banco GP. Fazer primeiro só o cálculo de offset.
