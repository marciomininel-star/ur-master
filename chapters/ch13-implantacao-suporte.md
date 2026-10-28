# 13 — Planejamento de implantação e recursos de suporte

**Fontes Core:** CRG p.59–69; CPP slides 111–131; CWB p.18–20.

## Ideia central

Avalie a complexidade da aplicação inteira, não apenas o alcance do braço ou a facilidade de ensinar pontos. O Core usa dez dimensões para organizar incertezas e escolher o que precisa de estudo, ensaio ou integração adicional.

## Matriz de dez dimensões do curso

“Simple/Advanced” é classificação didática de complexidade, não homologação ou garantia de retorno financeiro.

| Dimensão | Mais simples no curso | O que aumenta complexidade |
|---|---|---|
| Throughput | Ritmo semelhante ao processo manual | Ritmo maior e suas implicações na aplicação |
| Reach & Payload | Uma estação alcançável e carga compatível | Layout alterado, múltiplos robôs, conjunto pesado |
| Parts & Presentation | Peças consistentes e apresentação ordenada | Variação de forma/tamanho, apresentação desordenada |
| Collaborative with Humans | Espaços e interações definidos | Velocidade, ferramentas ou peças que exigem medidas adicionais |
| Connectivity & Integration | Interação básica e sinais de ciclo | Handshakes e integração mais estreita com máquinas |
| Grippers & End Effectors | Ferramenta padrão para peça consistente | Ferramenta especial e variação de peças |
| Mounting & Fencing | Montagem fixa e contexto bem definido | Relocalização e proteção adicional |
| Environment | Ambiente usual compatível com o equipamento | Fluidos, corrosão, particulados, higiene ou condições extremas |
| Programming & Logic | Processo repetitivo e feedback simples | Visão, força e coordenação de dispositivos |
| Future Needs | Processo atual estável | Mudanças futuras ainda não definidas |

CRG p.59/CPP 112 usam 10 kg/1300 mm como regra didática da época. Não são especificações de toda a família UR nem limites atuais. Para selecionar produto real, verifique datasheet do modelo/versão e inclua ferramenta, adaptadores e peça na carga. Os quadros históricos UR3e/UR5e/UR10e têm erros de cópia e não são mantidos como catálogo nesta skill.

## Método de planejamento

1. Nomeie o projeto e descreva entradas, saídas e taxa requerida.
2. Liste as dez dimensões e a evidência disponível em cada uma.
3. Marque “simples”, “avançado” ou “a confirmar” conforme a lógica do curso.
4. Identifique o que precisa ser medido: tempo de ciclo, massa, alcance, tolerância de apresentação, sinal/interface.
5. Identifique condições de interação humana e exigências do ambiente.
6. Defina próximos ensaios ou consultas e critério de aceitação para cada incerteza.
7. Reavalie o conjunto após obter evidências, sem converter a contagem de itens “simples” em aprovação automática.

Os passos de evidência/aceitação são reconstrução prática da matriz do curso, não um formulário original adicional.

## Exemplo trabalhado — alimentar máquina com peças ordenadas

Reconstrução:

| Item | Evidência inicial | Pendência |
|---|---|---|
| Peças | Uma família em bandeja regular | Confirmar dispersão da posição |
| Carga | Garra e peça identificadas | Medir massa/CoG carregado |
| Alcance | Pontos de pegar e máquina definidos | Conferir postura e aproximação |
| Interface | Start/complete previstos | Definir permissivos e falhas |
| Ritmo | Meta de ciclo registrada | Medir ciclo com espera da máquina |
| Pessoas | Abastecimento e inspeção descritos | Avaliar riscos da interação real |

Esse projeto pode ter apresentação simples e integração avançada ao mesmo tempo. Uma feature bem ensinada resolve localização, mas não resolve tempo da máquina, riscos ou confiabilidade de sinais. Ao mover o robô, reavalie localização das peças/máquinas e configuração da instalação (ch04/ch10).

## Recurso de suporte por pergunta

| Pergunta | Fonte indicada pelo curso |
|---|---|
| Como configurar uma função? | User/Script manual da versão e artigos de suporte |
| Como ligar equipamento? | Esquemas elétricos e manual de hardware correspondente |
| Por que ocorreu parada? | Histórico de log e ferramentas de leitura; contexto do evento |
| Como preservar trabalho para análise? | Backup de programas, instalações, configurações e logs |
| Como estudar sem robô? | URSim, com limitações |
| Como ampliar aprendizagem? | Academy, webinars e materiais de treinamento |

CRG p.64–69 / CPP 121–127 descrevem o suporte da Universal Robots. O endereço histórico citado é `https://www.universal-robots.com/support/`; a estrutura atual do site, disponibilidade de ferramentas e URLs não foram verificadas nesta conversão. Use documentação oficial atual quando a pergunta exigir atualização.

## Magic files, logs e URSim: limites do corpus

CPP 122 descreve Magic files como arquivos autoexecutáveis em USB para copiar logs, programas, instalações/configurações, fazer upload ou screenshot. Diferencie **backup** de **upload**: uma ação lê e preserva; outra pode modificar o controlador. Não execute arquivos nem a preparação destrutiva de treinamento do CPP 1 por constarem na fonte.

O curso orienta aguardar sinal de conclusão antes de retirar USB e informa execução sequencial quando vários arquivos estão presentes. Para uso real, siga as instruções do arquivo e versão corretos, sem assumir que o fluxo histórico vale para todo equipamento.

CPP 123 apresenta Support Log Reader para Windows e formatos CB2/CB3. Não inferir cobertura de todos os logs E-series atuais a partir dessa lista.

CPP 124 apresenta URSim como software Linux, inclusive em máquina virtual para Windows. Nas limitações daquela versão, force mode, colisões e funções de segurança não são reproduzidos integralmente. A formulação sobre simular entradas difere da opção Simulation do CPP 16; não confunda as duas superfícies. Um programa que executa offline não comprova interação física, força ou ausência de colisão.

## Exercício de pesquisa não é resposta factual

CWB p.20 pergunta versão de software mais recente, torque de junta, resolução do sensor e endereço Modbus; não fornece as respostas. Registre-as como perguntas que exigem fonte oficial do modelo/versão, sem preencher por memória. Também não registra conclusão de cadastro em treinamento nem autorização para fazê-lo.

## Antipadrões e conferência

- Escolher robô por massa da peça sem ferramenta.
- Tratar matriz como promessa de payback ou aprovação de segurança.
- Copiar datasheet histórico com erros de rascunho como especificação atual.
- Considerar backup, upload e limpeza de programas ações equivalentes.
- Tratar URSim como validação física de força/colisão.
- Responder pergunta de pesquisa como se a fonte contivesse o valor.

**Exercícios:** CWB módulos 10–11. **Conexões:** ch01 processo; ch05 carga; ch08 segurança; ch09 tempo de ciclo; ch04 relocalização.

## Integração Industrial/Interfaces — agora com fontes próprias

Os cinco PDFs acrescentados cobrem comunicação antes apenas mencionada: Modbus/rede (ch14), transferência e Dashboard (ch15), socket (ch16), PLC/fieldbus (ch17), interfaces cliente (ch18), RTDE (ch19) e XML-RPC (ch20). As referências são ICRG/ICWB/ICP e IFG/IFP, definidas em sources.md.

Ao planejar, atribua papéis, banco/tipo/unidade do dado, dono, handshake, timeout e retomada. Diferencie estado de conexão de êxito físico. Ferramentas Ananas, SocketTest, FileZilla, projetos de PLC e exemplos RTDE/XML-RPC são referências históricas, não dependências instaladas automaticamente. Os PDFs não fornecem todos os arquivos de apoio, layouts e programas; consultar versão oficial continua necessário para preencher as lacunas.


## Complemento dos manuais — 2026-10-28

IG oferece recomendações de implantação; DEP p.17–24 detalha carga por estado e cálculo de CoG. Backup/Support File/imagem são distintos (ch23). Plano de inspeção e diagnóstico agora em ch24/ch25. Modelos, edições e desenhos em ch22; não usar informação promocional como especificação.


## Complemento do lote ampliado — 2026-10-28

O corpus agora inclui modelos adicionais,OEM,cabos,RangeExtender,ConnectivityKit eIMMI. Usar matriz de modelo/versão e compatibilidadech36–54, em vez de transferir recomendações do treinamento antigo.
