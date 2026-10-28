# 15 — Transferência de arquivos e Dashboard Server

**Fontes:** ICRG p.12–14; ICWB p.6–7; ICP p.5, 24–37; IFG p.14.

## Ideia central

Transferir arquivo, carregar programa e executar programa são ações distintas. Dashboard é interface de comandos operacionais; não é interpretador arbitrário de URScript nem validação da célula.

## Arquivos: terminologia e escopo

Os materiais chamam o módulo de FTP e mostram porta 22. ICP p.26 explicita conexão por SSH/SFTP; portanto a descrição operacional é transferência pelo recurso SSH/SFTP do exemplo, não FTP clássico pela simples equivalência de nome.

O curso fornece credenciais antigas de bancada. A skill não as transforma em credenciais válidas para equipamento real; use configuração e conta autorizada da instalação. A afirmação de servidor “sempre ativo” é histórica: disponibilidade depende do equipamento/software e ajustes efetivos.

Fluxo documental do exercício:

1. Identificar programa e instalação usados na tarefa.
2. Preservar cópia antes de editar/reenviar.
3. Conectar cliente de transferência ao endpoint correto.
4. Transferir somente arquivos necessários ao diretório previsto.
5. Conferir o arquivo/versão no destino antes de carregar.
6. Verificar instalação associada e condições para execução.

O ICRG p.12 fala em Program directory, sem definir contrato universal de todos os caminhos de sistema. Não altere arquivos do controlador fora do escopo por estarem acessíveis. ICP p.26 alerta que uso incorreto do sistema de arquivos pode causar mau funcionamento.

## Dashboard: canal e categorias

O PC/PLC é cliente; UR oferece Dashboard Server na porta **29999** do curso. As tabelas ICP p.31–35 distinguem:

| Categoria | Comandos documentados |
|---|---|
| Consultar | `running`, `robotmode`, `get loaded program`, `isProgramSaved`, `programState` |
| Preparar | `power on`, `brake release`, `load <program.urp>` |
| Execução | `play`, `pause`, `stop` |
| Desligar | `power off`, `shutdown` |
| Mensagem | `popup <text>`, `close popup` |
| Situação de proteção | `close safety popup`, `unlock protective stop` |

Os últimos grupos podem ter efeitos físicos ou operacionais: uma consulta não equivale a autorização para liberar freios, iniciar, remover uma parada ou desligar. Os exercícios não autorizam enviar esses comandos ao equipamento do usuário.

ICP p.35 ainda apresenta `setUserRole` com papéis e restrições antigos. Compatibilidade e persistência após reinício não estão comprovadas: o workbook pergunta sobre persistência sem dar a resposta. Não usar a lista como mecanismo atual de controle de acesso sem verificar a versão.

## Modelo de estado, confirmação e falha

Reconstrução de integração a partir do exercício:

```text
Identificar IP, modo e programa esperado
  → conectar e consultar estado
  → preparar somente quando a ação foi autorizada
  → carregar arquivo esperado
  → confirmar programa carregado
  → verificar condições/permissivos da aplicação
  → iniciar se autorizado e acompanhar resposta/estado
Falha ou resposta inesperada → interromper sequência automática e diagnosticar
```

ICP p.5 pede Remote Control para operação remota, em contraste com descrições de servidor sem configuração adicional. Estar conectado não prova que comandos operacionais serão aceitos. O estado carregado, o modo do robô, as respostas do servidor e as condições de processo são verificações separadas.

A implementação deve tratar enquadramento de mensagem e respostas conforme manual da versão. O texto extraído não traz uma especificação completa de terminadores/retornos para todos os comandos; não invente uma API pronta a partir da tabela de ações.

## Exemplo trabalhado — programa enviado à estação

ICWB p.6 pede baixar programa do módulo Modbus, editá-lo, reenviar e então executar remotamente. O trabalho tem duas entregas verificáveis:

- **Arquivo:** nome, conteúdo e instalação correspondem ao pretendido.
- **Operação:** o programa carregado no controlador é esse arquivo e o sistema está no estado apropriado para o ciclo.

Um upload concluído não significa que o programa foi carregado; uma resposta a `play` não comprova peça processada. A conclusão física deve vir do contrato de processo, como contagem e confirmação do PLC (ch17).

## Paradas e recuperação

ICP p.34 afirma que Fault não é recuperável por Dashboard naquele contexto, mas fornece comandos para fechar popup/liberar protective stop. Fechar mensagem não elimina a causa. Não programe sequência que desbloqueia e reinicia repetidamente sem diagnóstico. O enunciado que pede provocar parada é atividade de treinamento e não foi executado.

O opcional ICWB p.7 cita Remote On e ligação elétrica de bancada. Esta síntese não converte isso em pinagem universal: para ligar fisicamente, são necessários modelo, esquema e validação do circuito. Ch10 aborda partida por sinais; ch08 mantém diferenças entre paradas e estados.

## Antipadrões

- Tratar FTP, SFTP, SSH e Dashboard como o mesmo serviço.
- Copiar credencial antiga como padrão garantido atual.
- Reenviar arquivo sem preservar instalação/versão anterior.
- Confundir `load` com executar ou `running` com êxito do processo.
- Conectar a 29999 e enviar qualquer URScript esperando execução.
- Usar fechamento de popup como reparo da causa da parada.
- Ignorar modo Local/Remote ou trocar papéis de acesso com base em tabela antiga.

**Conferência:** descreva endpoint, arquivo, programa carregado, resposta esperada e condição de processo, sem precisar executar movimentos. **Conexões:** ch10 startup; ch13 backup; ch18 envio de URScript por outro serviço.


## Complemento dos manuais — 2026-10-28

SVC4 p.177–178 exige investigação e reset deliberado de parada protetiva; capacidade de desbloqueio não autoriza loop automático. Arquivo de suporte não é backup restaurável; ver ch23/ch25.


## Complemento CB3 e artigos de referência — 2026-10-28

DBS p.1–3 diferencia confirmações até3.2 e a partir3.3; load/play/stop/pause mudam o momento da resposta. Parser de robotmode depende CB2/CB3. Ver ch32; não inferir semântica e-Series desse artigo.


## Complemento do lote ampliado — 2026-10-28

SafeBrakeSystemURSeries pode alterar respostaDashboard enquanto sustenta braço energizado(N09p.85–86). N12p.252 descreveSSH/portforwarding para interfaces abertas, sem habilitação implícita. Verch38/45/54.
