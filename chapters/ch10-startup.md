# 10 — Inicialização, BeforeStart e retomada

**Fontes Core:** CRG p.34–39; CPP slides 49–54 e 66; CWB p.8. **Advanced relacionado:** ch03.

## Ideia central

Carregar programa, inicializar braço e iniciar execução são decisões diferentes. BeforeStart prepara o estado antes do programa principal; não é um passo repetido a cada ciclo nem uma garantia de caminho de retorno a partir de qualquer posição.

## Separar as etapas

| Etapa | O que faz no curso | O que não garante |
|---|---|---|
| Load Default Program | Escolhe/carrega o programa padrão | Braço pronto ou ciclo autorizado |
| Auto Initialize | Automatiza energização/liberação de freios conforme configuração | Espaço livre para execução |
| Auto Start | Inicia o programa segundo sinais/configuração | Estado de peça/garra coerente |
| Init Variables | Atribui valores iniciais às variáveis já usadas | Persistência ou recuperação física do lote |
| BeforeStart | Executa preparação uma vez antes do fluxo principal | Movimento seguro de qualquer posição |
| Program Loops Forever | Repete o nó principal | Reexecução de BeforeStart a cada volta |

CRG p.39 explica que Init Variables não cria novas variáveis naquele editor: inicializa as já utilizadas. CPP 50/66 também permite inicialização por Assignment em BeforeStart. Escolha uma política clara e evite resetar um contador a cada ciclo por engano.

## Waypoint relativo versus posição absoluta

CRG p.35 / CPP 51: waypoint relativo registra diferença entre From e To. Na execução, aplica o deslocamento à posição então atual; não vai necessariamente ao local em que o offset foi ensinado. A orientação também pode mudar conforme o deslocamento definido.

**Exemplo reconstruído:** ensinar um deslocamento de +100 mm e executá-lo cinco vezes acumula 500 mm de translação na mesma direção, sob a hipótese de orientação/referencial constante. Isso não é um “home” absoluto e pode sair do espaço previsto. A direção efetiva precisa ser conferida no modo/referencial da versão.

CPP 50–51 informa que primeiro waypoint relativo ou variável pode evitar a tela Auto-Move. Preserve a consequência: o robô pode mover diretamente da posição atual. Não use esse comportamento como prova de retirada válida; analise a posição corrente e o caminho. A rotina Advanced ch03 troca Z em uma pose na Base e tem suas próprias hipóteses.

## Partida por nível e por transição

CRG p.36–37 ilustra seleção de entrada e transição para auto start/initialize. CPP 52 acrescenta:

- Com auto load, auto start e auto initialize habilitados, o nível que já coincide com o selecionado na energização pode iniciar a execução sem nova borda.
- Low merece atenção porque pode ser o estado inicial dos sinais no circuito descrito.
- As notas exigem Remote Control no cenário de auto start/initialize apresentado.
- `default.installation` participa da partida; se o programa usa outra instalação, revisar também os ajustes Startup associados a ela.

Não resolva essa diferença dizendo “sempre exige borda”. Pergunte qual instalação está ativa, quais opções estão habilitadas e qual nível existe no momento da energização. São descrições do rascunho, não confirmação do comportamento de todas as versões atuais.

## Exemplo trabalhado — preparação antes do ciclo

CWB p.8 pede carregar automaticamente o pick-and-place e acrescenta verificações opcionais. Reconstrução de planejamento:

```text
Identificar programa e instalação esperados
Inicializar variáveis que devem começar do zero
BeforeStart:
  verificar estado de garra e presença de peça
  decidir como tratar peça já agarrada/posição ocupada
  conferir posição atual e retirada possível
  aguardar permissivos/confirmar operação conforme projeto
Programa principal:
  executar ciclos com estado coerente
```

O WB sugere abrir a garra se estiver fechada. Aplicar isso cegamente pode soltar peça já presente; a reconstrução exige resolver o estado físico antes de comandar a garra. O objetivo do curso é verificar condições, não ordenar uma abertura em toda situação.

O exercício compara posição inicial fixa e relativa, configura programa padrão e observa o comportamento após reinício. Esta skill descreve esse teste, mas não altera Startup de equipamento conectado.

## Matriz de retomada

| Situação observada | Pergunta antes de continuar |
|---|---|
| Garra fechada | Contém peça? Onde seria apropriado soltá-la? |
| Destino ocupado | O ciclo deve esperar, usar buffer ou parar? |
| Contador reiniciado | O palete/inspeção físico também foi reiniciado? |
| TCP em região de contato | Existe retirada adequada ao frame e aos obstáculos? |
| Programa mudou de instalação | TCP, carga, I/Os e Startup correspondem? |
| Reinício automático imediato | Nível de entrada e opções combinadas explicam a execução? |

Essas perguntas conectam os exercícios do Core e Advanced, sem inventar uma rotina universal.

## Antipadrões e conferência

- Colocar inicialização dentro do ciclo e perder contagem.
- Confundir Home fixo com deslocamento relativo.
- Iniciar automaticamente antes de identificar a instalação.
- Pressupor nova borda quando nível já satisfaz a configuração.
- Evitar Auto-Move sem analisar o primeiro movimento.
- Resetar estado lógico enquanto peças físicas permanecem no buffer.

Para estudar, explique as três etapas de Startup, quando BeforeStart executa e como o estado físico se relaciona às variáveis. Consulte ch08 para estados de segurança; ch11 para retomada de palete; ch12 para seleção/contadores.

## Integração Industrial/Interfaces — comando remoto e estado confirmado

**Fontes:** ICWB p.6–7; ICP p.5, 29–37. Dashboard na porta 29999 distingue `power on`, `brake release`, `load` e `play`. Arquivo transferido não significa programa carregado; programa carregado não significa condições prontas; resposta ao início não comprova conclusão física. Detalhes e comandos históricos no ch15.

Local/Remote e instalação ativa continuam relevantes. A atividade opcional Remote On inclui ligação de bancada que exige esquema/modelo corretos antes de uso real; não é pinagem geral. Não usar sequência de comandos para desbloquear/reiniciar repetidamente sem diagnóstico. A persistência dos papéis `setUserRole` é pergunta do workbook, não fato estabelecido.


## Complemento dos manuais — 2026-10-28

U5E p.233–234 confirma partida por nível já presente na energização quando combinadas as opções; não pressupor nova borda. Requer modo de controle remoto no fluxo descrito. Sair da tela Executar ou tocar Parar desativa arranque automático até nova ação Executar conforme edição. Planejar retomada com estado físico; ver ch21/ch23.


## Complemento do lote ampliado — 2026-10-28

N12p.245–246 distingue habilitarRemoteControl de selecionarRemote e descreve restrições da origem do comando. Preservar estado da partida/retomada e carga em transição. Verch45/54.
