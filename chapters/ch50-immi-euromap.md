# 50 — IMMI/EUROMAP67: máquina de injeção e sinais de segurança

**Fonte:** N18Installation GuideIMMI41p, particularmente10–13,18,22–26,32–36. Não tratar tabela geralModbus/PROFINET como pinagemIMMI.

## Escopo antes dos sinais

IMMI conecta controladore-Series aIMM. N18 p.13 restringe certificação da interface de parada ao hardware/configuração de caixase-Series produzidas após01fev2020 e condições deEMC indicadas. Isso não certifica todo o ciclo nem transforma todos os sinaisEUROMAP em segurança.

A integração deve verificar comportamento da máquina de injeção também: emergência, safeguard, perda de energia e reset. Emergência do robô/IMM é bidirecional no caminho descrito; safeguard daIMM para robô tem outro comportamento. N18 p.12 distingue paradaCat1 eCat2 do robô e remete à documentaçãoIMM para a outra máquina.

## MAF não protege pessoas

N18 p.18 é explícita: MoldAreaFree é sinal de processo/proteção do molde, não salvaguarda de pessoal. Mesmo sendo chamado lightcurtain, sua presença não autoriza entrada de operador. O sinalMAF éPNP e indica livre sob condição definida; não equipararHIGH a ausência de pessoas certificada.

P.32 descreveMAF final condicionado à supervisão elétrica eMAF de software. Uma saída de softwareHIGH não comprova que o molde está livre. Definir falhas de sensor, robô fora da zona e recuperação de ciclo; uma ferramenta/peça caída pode manter zona ocupada apesar de posiçãoTCP esperada.

## Nós como protocolo de processo

N18 p.25–26 organizaTest,SetOutput eWaitUntil e nósStartupCheck,WaitForItem,EjectorBack/Forward,CorePullersIn/Out,FreeToMold eTemplate. São etapas verificáveis; não remover testes essenciais para evitar espera. Coordenar permissão de fechamento do molde com estado físico e confirmação de retirada, não apenas contador lógico.

No diagnóstico deI/O, verificar modo operacional e condições de partida: valores iniciais/Automático da edição não devem ser universalizados a instalações diferentes. Pinagem, correntes e contatos de segurança precisam da figura/tabela específica; p.36 separaMAF,StopI/O eI/O digital comum.

## Instalação e operação semIMM

N18 p.11 exige ligaçãoGND e cuidado com ribboncable/energia. Retirar/conectar módulo energizado não é ação de configuração de software. A fonte inclui bypassplug e modo semIMM; isso é procedimento específico, não instrução de eliminar salvaguardas para produção.

**Entregável:** mapa de sinais com função, direção, condiçãoHIGH/LOW, segurança/processo, dono e comportamento de falha; sequência do ciclo e ensaios conjuntosIMM/robô. Nenhum conector, preset ou programa foi alterado por esta incorporação.
