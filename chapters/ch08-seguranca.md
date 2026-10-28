# 8 — Segurança: limites, planos e estados

**Fontes Core:** CRG p.20–28; CPP slides 20–35; CWB p.4–5. As siglas sem C continuam designando o Advanced.

## Ideia central

O Core organiza a configuração de segurança por perigos da aplicação: região ocupada, juntas, ferramenta, interação com pessoas e estados externos. Os presets são pontos de partida, não comprovação de adequação. CPP 23–24 exige avaliação de risco e ressalta dependência da versão do software.

Esta síntese descreve os recursos do rascunho. Não certifica uma célula nem fornece valores de segurança universais; para aplicar uma configuração, é necessário manual da versão/modelo e validação da aplicação.

## Escolha do recurso

| Necessidade | Recurso apresentado | Limite da interpretação |
|---|---|---|
| Limitar região de ferramenta/cotovelo | Safety Planes | Não engloba automaticamente todo o braço |
| Representar volume da ferramenta | Tool Position com esfera(s) | TCP sozinho não descreve todo o volume |
| Restringir direção da ferramenta | Tool Direction, cone angular | É diferente de limite de posição |
| Restringir uma junta | Joint Limits: posição/velocidade | Restrição em espaço de juntas |
| Reduzir limites em condição específica | Reduced Mode por entrada ou plano | Requer definir gatilho e limites dos modos |
| Comunicar estado a outros equipamentos | Safety I/O | Exige interpretar polaridade e função corretamente |

CPP 22 cita potência, momentum, tempo/distância de parada, velocidade/força da ferramenta e do cotovelo. Restringir tempo/distância de parada pode limitar velocidade de execução. Não trate valor do preset como velocidade sempre alcançável: o controlador também precisa respeitar demais restrições.

## Fluxo de configuração descrito

1. Identificar o perigo e qual região/geometria será limitada.
2. Criar e nomear previamente as features necessárias. CPP 26 explica que desbloquear Safety retira potência nessa interface, impedindo ensinar pontos naquele estado.
3. Associar a feature ao plano e escolher restrição: Normal, Reduced, ambos ou Trigger Reduced Mode, conforme a tela.
4. Configurar representação da ferramenta e, se necessário, cone de orientação.
5. Definir limites de juntas e de cada modo coerentes com a análise da aplicação.
6. Revisar, aplicar, reiniciar e confirmar a configuração conforme o procedimento do guia.
7. Verificar os resultados no processo de validação da instalação.

Esses são passos documentais, não ações executadas pela skill. Ela não altera senhas, entradas ou limites de um robô.

## Cópia de feature/TCP não é vínculo automaticamente atualizado

CPP 27 descreve aviso de dessincronização se a feature original é alterada depois de copiada para o plano de segurança. CPP 28 descreve situação equivalente entre TCP de processo e Tool Position.

**Regra:** depois de alterar uma referência de processo, revise também a geometria copiada para segurança. Não presuma que reensinar uma peça move automaticamente seu limite de segurança.

Na versão apresentada, planos podem limitar esferas de ferramenta e cotovelo; as notas alertam que outras partes do braço não ficam automaticamente contidas por esses planos. Tool Position usa posição e raio em relação ao flange. Tool Direction limita orientação dentro de um cone centrado na direção de referência.

## Exemplo trabalhado — área de operador

CWB p.4 propõe reduzir risco na região da cabeça do operador usando plano, orientação para baixo e volume que contenha a ferramenta. Reconstrução do raciocínio:

- Mapear posição/volume do operador e da ferramenta.
- Definir fronteira e lado permitido.
- Representar a ferramenta inteira relevante, não só sua ponta.
- Considerar movimentos do cotovelo e outras partes não cobertas pelo plano.
- Definir condições de Reduced Mode e suas restrições quando houver aproximação da área.
- Verificar juntas e geometria nas posições críticas.

O exercício também pede provocar violação em ambiente de treinamento. Esta skill preserva o objetivo de entender a reação e a recuperação, sem converter o enunciado em uma ordem para forçar limites numa instalação real.

## Reduced, Recovery e paradas

- **Normal/Reduced:** conjuntos de limites e condições de ativação; Reduced não significa risco automaticamente eliminado.
- **Recovery:** CPP 33 descreve retorno manual à região permitida e impossibilidade de executar programa nesse estado. Limites de posição de junta, planos e orientação da ferramenta ficam desabilitados nesse modo; não interpretar como operação normal protegida pelos mesmos limites.
- **Emergency Stop, Safeguard Stop e protective stop:** são termos distintos do curso. Não substitua um pelo outro ou conclua que qualquer parada prova uma colisão.
- **Teach Pendant removido:** CPP 32 exige outra fonte de parada de emergência e alerta que o botão do pendant desconectado não estará ativo. É contexto de projeto/validação, não autorização para desabilitar recurso.

## I/Os e divergências do rascunho

CPP 30–31 menciona canais pareados, funções e polaridades. O RG p.28 simplifica o estado ativo de saídas, enquanto as notas do CPP 31 usam lógica baixa para algumas funções. Não use uma regra genérica “ativo = alto” nem forneça pinagem a partir dessa síntese. Algumas descrições de entradas/saídas e rótulos nas notas também são inconsistentes; consulte o esquema e o manual corretos antes de implementar.

## Antipadrões

- Copiar o preset ou exemplo numérico como configuração aprovada.
- Tratar plano como envelope de todos os elos.
- Reensinar TCP/feature sem revisar cópia de segurança.
- Equiparar redução de velocidade a ausência de risco.
- Usar thread, event ou `force()` como circuito de segurança.
- Aplicar afirmação promocional do rascunho de que colisão não causa lesão como garantia técnica; essa afirmação não é adotada pela skill.

## Conferência de estudo

Explique qual recurso limita posição, volume, orientação e junta; diferencie gatilho de Reduced de limite rígido; identifique o que muda em Recovery. CWB p.4–5 e CPP 22–33 são as referências. Para movimentos e tempo de ciclo, consulte ch09; para retomada, ch10.

## Integração Industrial/Interfaces — canal operacional não é canal de segurança

**Fontes:** ICWB p.5–7, 11; ICP p.5, 34, 77; IFP p.44–54. Flags de área compartilhada entre dois robôs, `unlock protective stop`, estado de rede ou valores RTDE não constituem, por si, função de segurança validada. A pergunta sobre PROFIsafe não vem acompanhada de comprovação de suporte: PROFINET I/O comum não demonstra capacidade de comunicação de segurança.

O exercício de provocar/desbloquear parada não foi executado. Fechar popup não elimina a causa da parada nem autoriza reinício. Para projeto remoto, separar consulta de estado, comando operacional e função de segurança. Ch15 documenta Dashboard; ch17 registra as lacunas dos mapas PLC; ch18–19 distinguem serviços e dados históricos CB3/E-series.


## Complemento dos manuais — 2026-10-28

U5E p.137–154 confirma limites por esferas/cotovelo, sem cobrir automaticamente todo o braço. Safe Home p.154 é função configurada de posição articular e saída ativa com robô parado, distinta de chamar uma pose qualquer de segura. SF3 (2017) trata CB3 e SAFE19 (2019) distingue gerações; categorias de parada IEC não são categorias de arquitetura ISO. Referências históricas não comprovam conformidade atual. Ver matriz de versões e ch21/ch25.


## Complemento do lote ampliado — 2026-10-28

Os modelos eServiceHandbooks novos não têm aplicabilidadeCB3 automática. ModoManual/Automatic,Local/Remote,Normal/Reduced/Recovery e3PE são eixos distintos; verch46. Nenhum estado de rede comum substitui circuito de segurança.
