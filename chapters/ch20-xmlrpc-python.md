# 20 — XML-RPC e fundamentos Python do curso

**Fontes:** IFG p.3–9 e 22–23; IFP p.3–10 e 60–66. Código em imagem conferido em IFP p.63.

## Ideia central

XML-RPC permite ao robô chamar funções registradas em um servidor remoto. Use esse modelo para cálculo, configuração ou serviço, distinguindo chamada de função de fluxo cíclico de setpoints RTDE.

## Papéis e dados

IFP p.61 apresenta o UR como cliente de métodos do servidor externo. O servidor executa o cálculo e devolve um resultado; a aplicação no robô decide como usá-lo.

O IFG p.22 mostra no robô:

```urscript
c = rpc_factory("xmlrpc", "http://127.0.0.1:8080")
```

É referência do material, sem execução. `c` é o handle usado para a chamada, não o objeto Python do servidor. Se o servidor está no PC externo, o endereço precisa apontar ao PC alcançável pelo robô: 127.0.0.1 no robô aponta ao próprio robô. Porta 8080 é a escolhida no exemplo, não uma porta obrigatória de todo XML-RPC.

## Definir contrato antes da chamada

| Item | O que combinar |
|---|---|
| Nome registrado | Nome usado pelo cliente, não apenas nome do arquivo |
| Argumentos | Quantidade, tipos, unidade e referencial |
| Retorno | Número, estrutura ou pose com convenção conhecida |
| Falha | Ausência do servidor, exceção de método ou resultado inválido |
| Efeito | Cálculo puro versus operação que modifica outro equipamento |
| Repetição | Se repetir após falha é permitido ou duplica uma ação |

Esses campos são reconstrução de projeto a partir do exemplo, não implementação completa incluída na apostila.

## Exemplo trabalhado — função add

IFG p.23 cria `add(x,y)`, registra com `register_function` e chama `add(2,2)`, com retorno 4. Reconstrução mínima em **Python 3**, sem iniciar servidor automaticamente:

```python
from xmlrpc.server import SimpleXMLRPCServer

def add(x, y):
    return x + y

def criar_servidor(host, port=8080):
    server = SimpleXMLRPCServer((host, port), allow_none=True)
    server.register_function(add, "add")
    return server
```

A função `criar_servidor` só é referência: não foi chamada nesta conversão, não houve bind/listen nem comunicação de rede. Um executável que quisesse atender teria de configurar endereço, executar o laço de atendimento e definir política de encerramento/erros. O teste offline desta skill chama apenas `add`.

No lado UR, o handle `c` chamaria conceitualmente `c.add(2,2)`. Não trocar o nome desse handle pelo nome da variável Python do servidor.

## Python 2, Python 3 e URScript

O material mistura `raw_input`, `SimpleXMLRPCServer`, `xmlrpclib`, `print` e `input`, além de incluir `end` em laços apresentados como Python. Esses elementos não podem ser colados indistintamente em um único runtime.

- A reconstrução acima usa `xmlrpc.server` de Python 3.
- Python usa indentação e não fecha `while` com `end`; URScript usa `end` nos blocos do curso.
- `print('hello')` com aspas simples ASCII é válido em Python 3; a alegação IFG p.3 de que texto precisa sempre de aspas duplas não é adotada.
- Aspas tipográficas de PDF não são delimitadores normais de código.
- Entrada por `input` requer decisão explícita de conversão/validação para número; não assumir que o mesmo exemplo se comporta igual em Python 2/3.
- Nome de função pode ser livre sintaticamente, mas deve comunicar a tarefa; o comentário didático de que “nome não importa” não substitui legibilidade.

Para estudo da linguagem do controlador, consulte ch02; as correções de Python não alteram sintaxe de URScript.

## Exemplo da câmera remota e pose

IFP p.63 mostra servidor legado que recebe uma pose, converte com `urlib.poseToList`, substitui por uma lista fixa e retorna via `urlib.listToPose`; a função registrada é `get_next_pose`. O slide usa `SimpleXMLRPCServer(("",8080), allow_none=True)` e imports de Python antigo.

A pose retornada da câmera do exemplo deve ser interpretada como dados de serviço. Não assumir que uma lista Python qualquer seja serializada automaticamente como pose UR: o exemplo usa helpers de conversão que não foram fornecidos como arquivo nesta solicitação. Unidade, frame e estrutura de serialização precisam ser confirmados antes de produzir um cliente que use o retorno para movimento.

IFG p.22 imprime uma criação de servidor com URL sem aspas dentro da tupla de bind, que não é código Python válido. Separar endereço de bind `(host, porta)` de URL de cliente `http://host:porta` resolve a distinção conceitual; não executar a linha original.

## Antipadrões e conferência

- Usar loopback para alcançar servidor de outro equipamento.
- Definir função e esquecer registro no servidor.
- Misturar sintaxe/imports de Python 2 e 3.
- Copiar `end` para Python ou removê-lo de URScript.
- Tratar todo retorno numérico/lista como pose já válida.
- Usar RPC síncrono como promessa de controle temporal determinístico.
- Repetir chamada com efeito físico sem saber se a primeira concluiu.

**Conferência:** `add(2,2)=4`, nome registrado corresponde ao cliente, endpoint tem host/porta coerentes e retorno tem contrato. **Conexões:** ch02 linguagens; ch03 representação de pose; ch16 protocolo de mensagem; ch19 troca cíclica configurada; ch13 implantação.


## Complemento dos manuais — 2026-10-28

SM511 p.21,108: chamada RPC bloqueia, exige método/assinatura/endpoint compatíveis e pode interromper programa em falha. /RPC2 depende do servidor. Semântica URScript de arrays por valor e matrizes não deve ser substituída pela de Python (ch26).


## Complemento CB3 e artigos de referência — 2026-10-28

SM341 p.5,51 também documenta RPC bloqueante e endpoint dependente do servidor. Não executar exemplos ao incorporar documentos; parsing e funções puras podem ser verificados offline. Ver ch30.
