# 16 — Sockets TCP/IP e protocolo da aplicação

**Fontes:** ICRG p.15–16; ICWB p.8–9; ICP p.40–49; IFG p.15–17; IFP p.31–40.

## Ideia central

Socket oferece canal de bytes; a aplicação precisa combinar quem conecta, como delimita mensagens e o que cada resposta significa. Conexão aberta não prova que a câmera tem dados novos ou que uma resposta contém coordenadas completas.

## Papéis e configuração

No exercício, **robô é cliente e PC/câmera é servidor**. O servidor escuta em endereço/porta configurados e o UR chama `socket_open(address, port, socket_name)`. O retorno booleano indica sucesso ou falha na abertura (ICP 42 / IFP 34).

A porta **50000** do ICRG p.15 é escolha do servidor do exercício, não serviço reservado do controlador. IFG também mostra outro número em screenshot; isso não altera o protocolo: os dois lados devem concordar. Loopback 127.0.0.1 não aponta ao PC remoto quando usado no robô.

Para vários canais, use nomes coerentes e passe o socket correto em cada operação. O curso pergunta quantos sockets são possíveis sem dar limite universal: não invente o máximo de uma versão.

## Contrato de mensagem

Antes do script, registre:

| Campo | Exemplo do curso / decisão |
|---|---|
| Estado de prontidão | `Ready` ou `ready`: combinar grafia exata |
| Disparo | String `T` |
| Pedido/resposta | Qual mensagem solicita a coordenada e qual resposta pertence a ela |
| Corpo numérico | Tupla ASCII como `(0.1,0.1,0.1)` |
| Unidades e referencial | X/Y/Z em metros no exemplo; Base/câmera precisa ser explicitado |
| Framing | Parênteses, contagem e/ou delimitador conforme parser |
| Timeout/reconexão | Limite e tratamento de resposta incompleta ou antiga |

Os slides usam X/Y/Rz em um exemplo e o workbook usa X/Y/Z em outro. Não chame o terceiro número de ângulo e altura ao mesmo tempo. Delimitação e capitalização precisam ser acertadas com o servidor, não deduzidas da aparência de uma tela.

## Leitura ASCII e índices

`socket_read_ascii_float(n, socket_name)` é descrita no ICP p.46 / IFP p.37 como retorno de **um inteiro de quantidade seguido dos floats válidos**. Para três valores completos, a forma lógica é:

```text
[3, x, y, z]
 índice 0 = quantidade
 índices 1,2,3 = dados
```

O IFG p.17 menciona até 30 floats no contexto da versão; não adote o limite para toda versão sem confirmar. Verifique quantidade e sucesso antes de indexar. A ausência de dados não pode ser tratada como `(0,0,0)` válido só porque a lista foi inicializada com zeros.

### Exemplo trabalhado — validar resposta sem socket

Reconstrução em Python 3, destinada somente a conferir o contrato:

```python
def validar_xyz(data):
    import math
    if len(data) != 4 or data[0] != 3:
        raise ValueError("resposta incompleta")
    xyz = tuple(float(v) for v in data[1:])
    if not all(math.isfinite(v) for v in xyz):
        raise ValueError("coordenada não finita")
    return xyz
```

`[3,0.1,0.2,0.3]` produz três coordenadas; `[2,0.1,0.2]` falha. A checagem de finitude é uma proteção de formato acrescentada na reconstrução; ela não valida alcance, obstáculos, limites de processo ou referência da pose.

## Fluxo da câmera no workbook

1. Iniciar servidor PC para simular câmera.
2. Abrir conexão do robô e tratar falha de abertura.
3. Enviar prontidão e disparo no protocolo combinado.
4. Solicitar/aguardar dados e ler três floats.
5. Validar quantidade, unidades, referencial e associação à peça.
6. Atribuir X/Y/Z e calcular pose/pré-posição com geometria apropriada.
7. Executar processo somente na aplicação autorizada e devolver prontidão/confirmação.
8. Fechar ou manter conexão conforme contrato, tratando desconexão.

ICWB p.9 pede `pose_trans()` para o alvo. O transporte não informa sozinho a transformação: a pose do frame precisa ser conhecida (ch03). O curso não fornece uma política completa de mensagens atrasadas, ID de peça ou retomada; para projeto real, explicite esses campos.

Reconexão não deve repetir automaticamente o último movimento sem saber se a peça já foi pega. Uma estratégia de tentativas limitadas, timeout e espera definida é reconstrução de integração, não comportamento garantido pela função.

## Formatos e APIs citados

IFG p.16–17 relaciona envio de byte/int/line/string e leitura ascii_float/binary_integer/byte_list/line/string. ICP/IFP cita também set/get de variável, sem fornecer protocolo completo do servidor. Para cada variante, conferir tamanho, sinal, ordem de bytes e terminador no manual da versão.

`socket_send_line` é descrito como texto com newline, enquanto `socket_send_string` envia string. O guia escreve `/n` ao explicar newline: o exemplo de código em outra página usa `\n`. Não copie `/n` como terminador. Nomes de função capitalizados em títulos não são sintaxe validada.

## Antipadrões e conferência

- Trocar cliente/servidor ou usar IP do cliente no lugar do servidor.
- Supor que socket aberto significa resposta válida.
- Ler índice 0 como X.
- Misturar três floats X/Y/Z com X/Y/Rz.
- Repetir conexão/disparo eternamente sem política de falha.
- Confundir uma chamada de leitura com o comando de aplicação que faz a câmera produzir dados.
- Reutilizar dados de ciclo anterior após reconexão.

**Conexões:** ch14 handshake por registradores; ch18 sockets no sentido PC→UR; ch19 dados estruturados RTDE; ch20 chamada remota de função.


## Complemento dos manuais — 2026-10-28

SM511 p.114–120 especifica bool no retorno dos envios, limites de 30 valores em leituras e timeout<=0 indefinido em leituras documentadas. socket_read_line é obsoleta, substituída por socket_read_string. Delimitação e contagem em ch28; envio aceito não confirma ação física.


## Complemento CB3 e artigos de referência — 2026-10-28

SM341 p.60–64 oferece contratos legados de timeout/retorno. EX p.28–29 troca nomes das funções; os exemplos errados não substituem a assinatura. Ver ch30.
