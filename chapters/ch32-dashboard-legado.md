# 32 — Dashboard legado: contrato e mudança em3.3

**Fonte:** DBS p.1–7, artigo15690; CB2≥1.4 e CB3≥3.0 no escopo declarado. A p.8 não oferece texto útil. Não extrapolar para e-Series atual. Complementa ch15.

## O que mudou na confirmação

Servidor na porta29999, comandos terminados por newline. Definir mensagem, resposta esperada e consulta posterior. A tabela separa comportamento v1.4–v3.2 de v3.3 para operações centrais:

| Comando | Até3.2 segundo tabela | A partir3.3 segundo tabela |
|---|---|---|
| load | Retorna antes de terminar carregamento | Retorna após carregamento concluído |
| play | Retorna antes de terminar início | Retorna quando execução foi iniciada |
| stop | Retorna imediatamente | Retorna quando parada concluída |
| pause | Retorna imediatamente | Retorna quando pausa concluída |

Fontes: DBS p.1–3, com tabela renderizada p.2. Retornar após iniciar não significa ciclo concluído ou ferramenta atuada. Evitar sequência load/play baseada apenas em atraso fixo no legado; consultar programa carregado e estados, com timeout e erros tratados.

## Consultas e capacidades por versão

| Comando | Dado / observação | Página |
|---|---|---|
| running | Robot running: True/False | 3 |
| robotmode | CB2 enum numérica; CB3 texto Robotmode | 3–4 |
| get loaded program | Caminho ou nenhum programa | 4 |
| isProgramSaved | Estado de salvamento | 4 |
| programState | STOPPED/PLAYING na fonte | 5 |
| polyscopeVersion | Versão do software | 5 |
| safetymode | Modo da segurança, diagnóstico comum | 6 |

Não reutilizar o número de robotmode Modbus258 para analisar texto DashboardCB3. Versionar o parser. O PDF enumera programState sem descrever todos os estados imagináveis; não completar por analogia com outra edição.

DBS p.4–6 também descreve popup/close popup, addToLog, setUserRole, power on/off, brake release, unlock protective stop, close safety popup e load installation. A tabela marca versões mínimas; load installation aparece em3.2.18654 e unlock protective stop em3.1. Elas são capacidades, não ações a executar por ter consultado esta skill.

setUserRole muda opções da Welcome screen no legado, com efeito condicionado a essa tela; não equivale a autenticação forte ou modo Local/Remote e-Series. Uma comparação com controle remoto moderno precisa de manual aplicável.

## Falha e retomada

Definir desconexão, comando sem resposta, resposta inesperada e reconexão com estado desconhecido. Não repetir play ou comando físico só porque a leitura expirou: primeira tentativa pode ter sido executada. Consulta resolve estado, confirmação da aplicação resolve resultado.

unlock protective stop não corrige causa. SVC4 p.177–178, de outra geração, reforça investigação e reset deliberado; não usar como licença para laço automático no CB3. Referências de rede/IP em DBS p.7 são exemplo de bancada, não configuração obrigatória.

**Exercício:** explicar diferença entre “Starting program”, running=True e peça concluída; resultado esperado: três evidências de fases distintas, com parser por versão e handshake da aplicação.
