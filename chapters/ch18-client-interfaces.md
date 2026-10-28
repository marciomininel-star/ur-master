# 18 — Interfaces cliente e envio de URScript

**Fontes:** IFG p.14, 18–19; IFP p.42–50. Industrial relacionado: ICP p.5 e Dashboard no ch15.

## Ideia central

Neste módulo o sentido da conexão se inverte em relação ao socket de câmera: **PC é cliente, controlador UR é servidor**. Escolha a interface pelo serviço pretendido; uma porta TCP aberta não aceita qualquer protocolo ou comando.

## Interfaces descritas no curso

| Porta | Nome | Conteúdo indicado |
|---|---|---|
| 30001 | Primary | Estado do robô e mensagens adicionais; recepção de URScript |
| 30002 | Secondary | Estado do robô; recepção de URScript |
| 30003 | Real Time | Estado do robô; recepção de URScript |
| 30004 | RTDE | Dados configurados e registradores/setpoints; ch19 |
| 29999 | Dashboard | Comandos operacionais de vocabulário próprio; ch15 |

IFP p.44 associa 10/10/125 Hz às três primeiras interfaces. A apresentação identifica CB3 em p.43, e os números são históricos. Não prometer a mesma taxa, disponibilidade ou aceitação de comando em toda versão E-series/PolyScope. Para projeto real, identificar modelo, software e modo Local/Remote.

O comentário IFG p.18 “any port would work” não é adotado: porta determina serviço e formato. Não envie URScript a 502, 29999 ou 30004 como se fossem equivalentes.

## Três entregas distintas

1. **Conexão de transporte:** socket TCP ao endpoint correto.
2. **Mensagem entregue:** bytes, codificação e terminador definidos; envio completo.
3. **Execução/conclusão:** robô interpreta o programa e produz o resultado esperado.

Uma operação de envio não comprova a terceira etapa. O programa pode ser rejeitado, interrompido, depender de instalação ou ser incompatível com o estado atual.

IFG p.19 mostra `.encode()` e `\n` no envio de uma linha. O guia usa `send`; uma implementação deve cuidar do envio completo e do tratamento de falhas, em vez de presumir que qualquer chamada equivale a todo o programa entregue.

## Exemplo trabalhado — preparar bytes sem conectar

Reconstrução Python 3 apenas para verificar enquadramento de uma linha ASCII:

```python
def preparar_linha(script):
    if not script.strip():
        raise ValueError("comando vazio")
    return script.rstrip("\r\n").encode("ascii") + b"\n"

mensagem = preparar_linha('popup("Exemplo de estudo")')
```

Esse exemplo produz bytes terminados em newline; **não abre conexão nem envia comando**. A escolha ASCII serve ao pequeno exemplo, sem declarar que toda interface só aceita essa codificação. Para bloco de várias linhas, preservar indentação, `def`/`end` e formato exigido pela versão. Não transformar esse helper de linha em validador de programas URScript.

O exercício sugere testar popup, saída digital e movimento. A skill não executa esses testes em equipamento. Movimento ou Set enviado remotamente continua sendo ação física, mesmo fora de um arquivo `.urp`.

## Estado recebido não é texto livre

IFP p.45 explica que o fluxo de estado contém muitos bytes e precisa de decodificação; pode mudar com releases. Não use `decode()` de texto ou offsets fixos inventados para obter uma pose de pacote binário. Escolha parser/esquema da versão ou avalie RTDE quando a necessidade for receber campos selecionados.

Uma fronteira de leitura TCP não deve ser tratada automaticamente como fronteira de mensagem completa. O curso não fornece aqui um parser binário integral, nem política de remontagem, limites de tamanho e tratamento de pacotes desconhecidos. Essas são pendências de implementação.

## Limitações e erros da fonte

- IFG p.18 imprime `Import socket` e exemplos com aspas tipográficas; isso não é Python válido sem correção.
- IFG p.19 usa `get_current_tcp_pose()` no exemplo de deslocamento; outros materiais usam `get_actual_tcp_pose()`. Não publique o primeiro como API confirmada: verificar manual da versão antes de gerar movimento.
- IFP p.46 capitaliza nomes como `Movel` em lista de tentativa; títulos/listas não comprovam grafia exata da API.
- O princípio de execução na ordem recebida não estabelece transação completa entre vários clientes, conclusão de trajetória ou prioridade sobre programa existente.

## Antipadrões e conferência

- Escolher porta por parecer “a mais rápida”, sem definir objetivo.
- Confundir estado CB3 com contrato de todas as E-series.
- Decodificar fluxo binário como linha de texto.
- Considerar bytes enviados iguais a peça processada.
- Misturar servidor da câmera (ch16) e servidor do controlador.
- Gerar movimento remoto antes de definir estado, instalação e autorização da operação.

**Conferência de estudo:** desenhe os dois sentidos de conexão, compare 29999/30001/30004 e prepare bytes offline. **Conexões:** ch02 sintaxe URScript; ch10 modo/partida; ch15 Dashboard; ch19 troca de dados configurada.


## Complemento dos manuais — 2026-10-28

SM511 p.14 descreve script completo, indentado e terminado em newline na porta30002. Programa secundário não consome tempo físico, sem sleep/movimento (p.28). Interpreter Mode na porta30020 é mecanismo distinto, com sessão/fila/ack; ver ch26.
