# 19 — RTDE: recipes, registradores e handshake

**Fontes:** IFG p.20–21; IFP p.52–58. Páginas com código em imagem conferidas visualmente: IFG 20–21 e IFP 55–56.

## Ideia central

RTDE troca um conjunto configurado de campos entre processo externo e controlador. Receber setpoints não movimenta o robô por si só: o exemplo depende também de um programa no robô que lê registradores, monta a pose e executa sua lógica.

## Escopo documentado

IFG p.14 e IFP p.54 indicam porta **30004**. O módulo apresenta RTDE como alternativa robusta à decodificação do fluxo Real Time para dados de estado selecionados. “Alternativa” não significa mesmo protocolo, comandos ou formato.

IFP p.54 lista saídas como estado do robô/juntas/ferramenta/segurança, I/Os e registradores de saída; entradas incluem dados de controle e registradores de entrada. O nome input/output é do ponto de vista do controlador.

A taxa 125 Hz do slide pertence ao exemplo identificado como CB3. Não assumir taxa universal E-series nem equivalência entre frequência de rede, frequência de aquisição e frequência do controle da aplicação.

## Peças do exemplo

| Peça | Função apresentada |
|---|---|
| `example_control_loop.py` | Configura IP/porta, carrega recipes, envia setpoints e recebe estado |
| Arquivo de configuração | Define nomes/tipos de campos dos conjuntos trocados |
| `.urp` de controle | Lê registradores e aplica fluxo/movimentos |
| Thread de pose | Copia componentes de registradores para uma pose temporária |
| Registrador de handshake | Indica estado do ciclo/prontidão para próximo alvo |
| Watchdog | Detecta ausência de atualização conforme configuração do exemplo |

Os arquivos completos e dependências do exemplo não foram fornecidos como código: há screenshots. A skill não reconstrói um cliente RTDE de produção nem inventa XML de recipe ausente.

## Mapeamento e direção

Na imagem IFP p.55, Python acessa `input_double_register_0`…`5` por helpers que convertem objeto em lista e vice-versa. Na p.56, o `.urp` usa `read_input_float_register(0)`…`5` para preencher componentes de pose. O nome de campo do transporte e o nome da função URScript não são grafias intercambiáveis; são camadas diferentes do mesmo exemplo.

A pose temporária tem seis componentes: metros e vetor de rotação em radianos (ch03). Não inserir mm, graus ou junta diretamente nessa estrutura por ser uma lista de seis números.

As faixas de registradores disponíveis/reservadas e concorrência com fieldbus dependem de versão. Antes de combinar PLC e RTDE, defina dono, faixa e direção de cada campo; mesmo índice numérico em protocolos diferentes não demonstra memória idêntica ou livre.

## Exemplo trabalhado — dois alvos alternados

IFG p.20 mostra:

```text
setp1 = [-0.266, 0.478, 0.234, 3.05, 0.25, -0.1]
setp2 = [-0.266, 0.478, 0.300, 3.05, 0.25, -0.1]
```

São alvos históricos do exemplo e não posições apropriadas a qualquer célula. A diferença de Z é **0,066 m = 66 mm**; x/y/orientação permanecem iguais nesse par. Conferir a diferença offline não valida a trajetória.

A imagem mostra o Python receber estado; se `state is None`, sair do laço; quando `output_int_register_0 != 0`, selecionar o outro setpoint e enviar. A imagem do `.urp` escreve 0 antes do MoveL e 1 após o movimento, intercalando `sync()`. A thread copia os seis valores para `tmp` e atualiza `setp` quando não é pose nula.

Reconstrução do raciocínio:

```text
Configurar campos/tipos iguais nos dois lados
Conectar e iniciar troca de estado
Robô sinaliza fase do ciclo
Cliente só publica próximo alvo na fase esperada
Programa do robô consome alvo coerente e executa seu trecho
Falha de recepção/watchdog → política de interrupção definida
```

Não copiar automaticamente o teste “pose diferente de zero” como validação de integridade: uma pose não nula ainda pode estar fora do processo ou conter componentes de atualizações distintas. A coerência entre atualização de campos e sinalização precisa ser projetada; o material não especifica transação universal para qualquer laço modificado.

## Watchdog e mudança de controlador de movimento

IFP p.56 mostra `rtde_set_watchdog("input_int_register_0", 1, "PAUSE")`. O 1 é a frequência mínima configurada naquele exemplo, não taxa global do RTDE nem prova de supervisão de segurança. Defina significado e atualização do campo observado antes de reutilizar o mecanismo.

IFG p.21 sugere substituir MoveL por `servoj` com cinemática inversa e parâmetros em screenshot. Isso altera a arquitetura de controle e não é substituição trivial. Não recomendar o tempo/lookahead/gain do rascunho como valores universais; faltam implementação completa, período coerente, comportamento de perda de comunicação e validação física.

## Antipadrões e conferência

- Enviar dados sem `.urp`/lógica consumidora correspondente.
- Usar nomes de campo da recipe como nomes de API URScript.
- Confundir input/output do UR com direção do PC.
- Escrever em registradores usados por outro cliente/PLC.
- Mandar próximo setpoint sem verificar fase do ciclo.
- Tratar watchdog ou flag de software como função de segurança.
- Prometer controle validado a 125 Hz em qualquer modelo.

**Conexões:** ch03 pose; ch07 threads; ch17 bancos/PLC; ch18 fluxo binário; ch20 serviço remoto para cálculos em vez de streaming.


## Complemento dos manuais — 2026-10-28

SM511 p.105–109 esclarece reservas de registradores por tipo e watchdog ignore/pause/stop; watchdog removido ao parar programa. Um watchdog por pacote pode bastar. Não inferir taxa de rede do ciclo500Hz. Ver ch28.


## Complemento do lote ampliado — 2026-10-28

N10p.117 descreveget_rtde_value com umpasso de atraso; sensorF/Texterno tem contrato/watchdog próprios. Campos/tipos/receitas seguem versão. Verch43/44.
