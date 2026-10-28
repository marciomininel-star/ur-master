# 17 — PLC, EtherNet/IP e PROFINET

**Fontes:** ICRG p.17–24; ICWB p.10–11; ICP p.51–77.

## Ideia central

A integração com PLC combina papéis de rede, descrição do dispositivo e mapa de dados. Documente cada sinal pela perspectiva de quem lê/escreve: entrada do robô é saída do PLC, e vice-versa.

## Seleção e papéis

| Tema | EtherNet/IP do curso | PROFINET do curso |
|---|---|---|
| Papel UR | Adapter | IO-Device |
| Controle externo | Scanner | IO-Controller |
| Ferramenta do exemplo | Studio 5000, CompactLogix L16ER | TIA Portal, Siemens S7-1200 |
| Configuração | Projeto/módulo e conexão, RPI | GSD/GSDML, nome, IP e módulos |
| Diagnóstico inicial | Estado do adapter e scanner | Estado do IO-Device e controller |

ICP 53–55 distingue explicit messaging, cliente/servidor em TCP, de implicit I/O messaging, scanner/adapter em UDP e atualização periódica. Isso é introdução conceitual: não prova que o UR suporte todas as modalidades descritas. A frase de “entrega garantida” no slide não é garantia de conclusão física de um processo.

Para PROFINET, ICP 67 separa IO-Controller, IO-Supervisor de engenharia/diagnóstico e IO-Device. Não confunda equipamento de supervisão com o controlador do ciclo.

## Sequência de configuração documental

1. Identificar controlador, versão de PLC/software e descrição do dispositivo compatível.
2. Configurar endereços e nomes sem duplicidade no segmento do exercício.
3. Habilitar o serviço no PolyScope e salvar instalação.
4. No PLC, adicionar dispositivo e módulos/dimensões esperados.
5. Definir mapa de tags com tipo, direção e endereço.
6. Verificar estado da conexão e testar dados conhecidos antes de ligar ao processo.
7. Especificar comportamento de perda de comunicação e retomada.

O curso usa amarelo para serviço habilitado sem controlador conectado e verde para conexão com scanner/controller. Verde não prova que tipos e significado dos dados estejam corretos.

EtherNet/IP: ICP p.59 cita conferir RPI e Unicast no projeto de exemplo. RPI não é sinônimo de ciclo completo do robô. PROFINET: ICRG p.19–22 cita nome do dispositivo, GSDML e UDT e adição de módulos. Os nomes/datas de arquivos divergem entre páginas e algumas instruções UR/PLC estão trocadas; não copiar como receita infalível.

## Banco de registradores pela perspectiva do UR

ICP p.60 e 75 documenta:

| Tipo | Ler entrada do UR | Ler saída do UR | Escrever saída do UR |
|---|---|---|---|
| Boolean | `read_input_boolean_register(a)` | `read_output_boolean_register(a)` | `write_output_boolean_register(a,v)` |
| Integer | `read_input_integer_register(a)` | `read_output_integer_register(a)` | `write_output_integer_register(a,v)` |
| Float | `read_input_float_register(a)` | `read_output_float_register(a)` | `write_output_float_register(a,v)` |

Endereço é índice no banco dessa API. Não equivale automaticamente a endereço Modbus nem offset de bytes do módulo PLC. Mapa, faixa reservada e propriedade do registrador precisam ser conferidos na versão, sobretudo quando fieldbus e RTDE coexistem.

Para mapear um sinal, escreva por exemplo: “UR output boolean register 0 → PLC tag de entrada PaletePronto”. Nome da tag no projeto é do integrador; não o transforme em nome universal da API.

## Exemplo trabalhado — palete pronto e confirmação

ICWB p.10 pede contagens de dois SKUs, sinal de palete completo aos 10 produtos por SKU e espera por troca do palete. O ICRG p.17 relaciona três registradores de SKU; são variantes do exemplo, não razão para inventar terceiro produto no workbook.

Reconstrução de handshake:

```text
Contar somente colocação concluída
Ao atingir quantidade requerida:
  publicar contagens e PaletePronto
  esperar confirmação válida de palete removido/substituído
  reconciliar estado físico e índice do palete
  retirar PaletePronto e resetar contagem para próximo lote
```

Para evitar confirmar com sinal antigo, defina estado inicial, dono e momento de reset/acknowledgment. Essa especificação adicional não está completa nos PDFs.

ICRG p.23 apresenta case packer com **4 peças**, `write_output_boolean_register(0, True)` e espera lógica por input boolean register 1. É outro exercício, diferente das 10 peças/SKU. A espera deve corresponder ao bit configurado no PLC. Corrija a sintaxe para identificadores válidos apenas quando a API estiver confirmada; o texto do guia separa palavras de um identificador e usa capitalização inconsistente.

## Limites e lacunas

ICWB p.11 propõe controlar speed slider e saídas por “register 32/64”, mas não define suficientemente layout, máscara, tipo e banco. Não usar esses números como chamada direta a `write_output_*_register(32/64,...)` sem mapa oficial.

A pergunta sobre PROFIsafe é reflexão sem resposta. Suporte a PROFINET I/O comum não demonstra suporte a comunicação de segurança. Da mesma forma, as classes NRT/RT/IRT e tempos gerais de ICP p.68 não garantem todas essas capacidades no dispositivo UR.

## Antipadrões e conferência

- Inverter sentido PLC/UR pelo nome “input”.
- Copiar UDT de saída para entrada devido a erro da apostila.
- Confundir registrador API, endereço Modbus e offset do módulo.
- Resetar contador ao sinal de palete cheio antes da troca física.
- Assumir conclusão só porque rede está verde.
- Promover classe IRT ou PROFIsafe a recurso comprovado do UR.

**Conexões:** ch11 ocupação; ch12 contagem; ch14 Modbus; ch19 propriedade de dados e handshake RTDE; ch08 segurança.


## Complemento dos manuais — 2026-10-28

PN p.8 resolve o exemplo do slider: bit32=byte4, REAL O2T com máscara SSM. Bit64=byte8 do bloco de máscaras/saídas; não índice GP32/64. PN p.6 contém inconsistência Byte/Bit nos ints T2O, que exige validação no projeto. Ver ch29 antes de recomendar offsets.


## Complemento do lote ampliado — 2026-10-28

FlexibleEtherNet/IP emN10p.175–182 permiteassemblies customizadas com serviço/configuração e limitações próprios; não confundir comPROFINET ou bancoGP. Verch44.
