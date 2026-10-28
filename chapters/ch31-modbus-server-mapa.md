# 31 — Modbus Server: endereços, escala e sinal

**Fonte:** MBS p.1–7, artigo16377, exemplos CB2≥1.6/CB3 segundo a fonte. P.8 sem texto útil. Não é declaração de validade para todo firmware atual. Relacionados: ch14 e ch28.

## Separar papel e espaço de endereços

Aqui o robô é servidor TCP na porta502 e o PC/PLC é cliente. Isso difere do robô cliente de módulosI/O configurados no PolyScope; ele pode exercer os dois papéis. Declarar papel por conexão.

MBS p.1–2 lista funções01/02 para bits,03/04 para leitura de registradores,05/0F para escrita de bits e06/10 para escrita de registradores. Mesmo endereço numérico em bit e word não significa mesmo objeto. Verificar coluna R/W e disponibilidade da versão. Exceções01/02/03 indicam função, endereço ou valor inválido; não são confirmação de comando.

O servidor usa base0. Se biblioteca ou PLC apresenta base1, traduzir uma vez na fronteira; não somar1 em várias camadas. P.7 descreve espelhos10000+x,20000+x,30000+x,40000+x e9999 para endereço0 em contextos Siemens da época. Não aplicar prefixos4xxxx cegamente à API de biblioteca que espera offset bruto. Unit Identifier é ignorado pelo servidor no escopo documentado; não generalizar para gateways.

## Mapa de consulta dirigido

| Endereço word | Conteúdo / escala | Página MBS |
|---|---|---|
| 0 / 1 | Bits de entradas / saídas em palavra | 2 |
| 4–11 | Analógicos e seus domínios | 2–3 |
| 16–19 | Saídas analógicas/domínios | 3 |
| 20–22 | Tensão da ferramenta, bitsI/O | 3 |
| 30–33 | I/O configurável e máscaras, conforme versão | 3 |
| 128–255 | GP16bit R/W | 3 |
| 256–258 | Versão alta/baixa e robotmode | 4 |
| 260–265 | Flags de estado | 4 |
| 270–275 | Juntas em mrad | 4 |
| 280–285 | Velocidade articular em mrad/s | 4 |
| 290–295 | Corrente articular em mA | 4–5 |
| 300–305 / 310–315 | Temperatura / modo das juntas | 5 |
| 320–325 | Contagem de revoluções, conforme versão | 5–6 |
| 400–402 | TCP XYZ na Base, décimo de mm | 6 |
| 403–405 | Orientação TCP na Base, mrad | 6 |
| 410–415 | Velocidade TCP: mm/s e mrad/s | 6 |
| 420–425 | Offset TCP no tool frame: mm e mrad | 6 |
| 450–451 | Corrente robô/I/O, mA | 6 |
| 768–770 | Estado, temperatura e corrente da ferramenta | 6 |

A frase p.2 “128–256” contradiz a tabela p.3 “128–255”;128 endereços terminam em255. Endereço256 é versão do controlador. Usar a tabela como reconciliação documentada, conferindo implementação antes de escrever. Program/GUI state marcados “not implemented” não são indicadores operacionais confiáveis.

Mapa de bits p.7:0–15 entradas;16–31 saídas;32–47/48–63 máscaras;128–159 configuráveis/máscaras;260–265 flags. Colunas e desenhos indicam bits indefinidos e disponibilidade; não presumir todos utilizáveis ou graváveis.

## Conversão signed16: corrigir a errata explicitamente

MBS p.2 sugere val>32768 e subtrair65535; isso erra a fronteira e o módulo. Para palavra bruta unsigned de16bits, usar:

```python
def uint16_to_int16(raw):
    if not isinstance(raw, int) or not 0 <= raw <= 65535:
        raise ValueError("Esperada palavra uint16")
    return raw - 65536 if raw >= 32768 else raw
```

Casos:32767→32767,32768→-32768,65535→-1. Aplicar apenas quando o campo exige sinal e a biblioteca entrega unsigned. read_port_register em SM341 p.50 e SM511 p.108 já entrega signed; não reconverter.

Exemplo reconstruído: XYZ raw65526 pode representar -10 unidades de0,1mm, logo -1mm=-0,001m. Orientação usa mrad, não graus nem décimos de mm. OffsetTCP420 usa mm, diferente do XYZ400. Corrente é mA; não aplicar signed arbitrariamente a versão/flags.

**Método:** conexão → função → base → endereço → R/W → tipo/sinal → escala/frame → atualidade → handshake. Consultar estado via rede comum não transforma Modbus em canal de segurança.
