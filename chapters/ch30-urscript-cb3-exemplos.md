# 30 — URScript 3.4.1 e revisão dos exemplos legados

**Fontes:** SM341 p.1–66, versão3.4.1 de 03/04/2017; EX p.1–30, sem versão formal identificada. SM511 é outra fonte, para e-Series5.11. A numeração de SM341 coincide com a física nas páginas citadas.

## Escolher manual antes de adaptar exemplo

Use SM341 para interpretar código legado do contexto CB3. Use SM511 para e-Series5.11. O fato de uma função existir nas duas edições não torna assinaturas, faixas, temporização e obsolescência idênticas. EX omite comandos sem argumentos e contém erros; não é catálogo completo nem código pronto para executar.

| Aspecto | SM341 | Comparação contextual |
|---|---|---|
| Controle/escalonamento | 125Hz, quadro8ms, p.9–10 | SM511 descreve500Hz/2ms; isso não define taxa de rede |
| servoj | t=0.008; a/v não usados naquela edição, p.17 | Não converter período por simples troca de modelo |
| GP boolean | 0–63, p.48–49,64 | SM511 acrescenta faixas superiores reservadas |
| GP int/float | 0–23, p.49–50,65 | Não presumir índices24–47 no legado |
| read_port_register | Já retorna signed16, p.50 | Mesma cautela sobre conversão duplicada do ch28 |
| Socket leitura | Timeouts de2s nas funções p.60–62 | Não acrescentar parâmetros da versão nova sem suporte |
| Socket envio | Retorno bool, p.63–64 | Sucesso não confirma atuação física |

SM341 p.35 define point_dist sem rotação. P.36 define pose_add como soma de posições e produto de rotações; pose_dist apenas diz distância, sem detalhar a métrica. Para a interpretação mais explícita SW5.11, citar SM511, sem atribuir essa explicação ao texto antigo. Ausência de um recurso no manual3.4.1 não prova inexistência em todas as versõesCB3.

## Método de revisão: intenção → assinatura → domínio → estado → retorno

1. Identificar a intenção: cálculo, I/O, movimento, consulta ou comando remoto.
2. Confrontar nome, argumentos, ordem e defaults com manual da versão. Conferir underscores, aspas ASCII, dimensão de listas/poses e indentação.
3. Registrar unidade/frame e domínio. Um canal analógico configurado em volts pode receber uma fração normalizada, não volts diretamente.
4. Para efeitos físicos, definir pré-condição, conclusão e falha; dado enviado não é confirmação.
5. Ensaiar primeiro cálculos puros e parsing; só integrar hardware no contexto de execução solicitado.

## Erratas de EX que afetam decisões

| Página EX | Problema | Tratamento |
|---|---|---|
| 1 | force_mode com pose de5 componentes | Reconstruir com6 apenas após confirmar intenção/frame |
| 3 | Seção movep chama movel e inclui t | Usar assinatura movep da versão; são movimentos distintos |
| 6 | Lista speedl com7 componentes | Exigir6 componentes e unidade correta |
| 9 | Seções set_payload_cog/mass mostram chamadas set_payload | Não inferir assinatura pelo exemplo errado |
| 23 | Nomes com espaços no lugar de underscore | Conferir no manual e no original visual |
| 26 | set_standard_analog_out(1,4) descrito como4V | API usa valor relativo; conferir domínio antes de converter |
| 26 | Argumento domain0 descrito como domain1 | Confirmar enumeração em manual, não na legenda |
| 28 | Binary/byte list exemplificados com read_ascii_float | Escolher função pelo formato combinado |
| 29 | send_line/string exemplificados com send_int | Usar função correspondente; newline muda framing |
| 29 | socket_set_var_name diverge de socket_set_var do manual | Preferir nome documentado na versão |
| 30 | write_port_register exemplificado com write_port_bit | Não escrever número como booleano |

Para4V num domínio0–10V, a reconstrução conceitual é fração0,4; não executar set de saída para demonstrar. SM341 p.57 descreve domínio de entrada e nível relativo de saída; confirmar faixa antes de aplicar. Aspas e nomes quebrados podem resultar da extração, mas erros de chamada e escala em EX p.26/29 foram vistos na página renderizada.

## Navegação

SM341: linguagem/escopo/RPC/threads p.3–10; motion p.11–21; internals/carga/TCP p.22–30; matemática/poses p.30–40; interfaces p.40–66. EX: movimento1–6; internals7–10; matemática11–16; I/O/Modbus17–27; sockets27–29; registradores29–30.

**Exercício:** revisar três exemplos sem enviar ao robô: pose malformada, saída analógica4V e send_int com string. Resultado: erro identificado, versão indicada e contrato reconstruído, sem tratar exemplo como ação solicitada.
