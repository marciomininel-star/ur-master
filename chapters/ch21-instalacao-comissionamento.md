# 21 — Instalação e comissionamento

**Fontes:** IG p.4–12; U5E p.21–38, 41–67, 137–154; SVC4 p.21; DEP p.1–24. Páginas físicas; exemplos IG incluem CB3.

## Método: identidade → montagem → configuração → validação

Registre modelo, geração/controlador, serial quando disponível, PolyScope, ferramenta/peça e orientação de montagem. Um desenho UR10 CB3 não resolve uma ligação e-Series OEM. Relacione cada parâmetro à fonte e versão.

1. Mapeie processo, operadores, manutenção, peças, máquinas vizinhas e energias externas. Inclua perda de energia, erro de pega e retomada.
2. Confira suporte, vibração, acesso e proteção ambiental por componente. U5E p.42 pede superfície capaz de suportar pelo menos dez vezes o torque total da junta de base e cinco vezes o peso do braço. São critérios daquela edição, não cálculo estrutural completo.
3. Configure montagem, TCP, massa, CoG e inércia por estado físico. O assistente de estimativa não corrige uma montagem configurada incorretamente.
4. Defina sinais com função, tipo, direção, tensão/corrente, estado inicial e falha. Preserve os dois canais de segurança e sua independência (U5E p.48).
5. Revise caminhos, blends, estados persistentes e retomada. U5E p.233–234 confirma início automático com nível já presente na energização em certas combinações.
6. Valide emergência, safeguard/reset, limites, modos, habilitador e saídas para outras máquinas conforme SVC4 p.21. Registre resultados e responsáveis; simulação não comprova ensaio físico.

## Guia de instalação: recomendações não são limites

IG p.4 destina-se a integradores experientes e oferece pontos de partida. Blend de 15 mm e acelerações de 800 graus/s² ou 2500 mm/s² são sugestões contextuais; não transformá-las em parâmetros universais. Primeiro melhorar caminho/transições, depois avaliar velocidade e aceleração com carga e processo.

Em plataforma móvel, considerar aceleração da base e condição do braço. is_steady não autoriza transporte. MoveP contínuo não combina com bloqueio por operador/I/O no trecho (IG p.7). Interrupção de movimento por condição continuamente avaliada requer perfil de parada projetado.

## Entregável e anti-padrões

Use matriz com item, configuração, estado esperado, ensaio, resultado, fonte/versão e pendência. Preserve backup conhecido e identificação da instalação. Não aceitar checksum padrão como avaliação concluída, nem aumentar limites para esconder parada protetiva. Classificação IP e corrente de ferramenta dependem do componente/modelo.

**Exercício:** para garra com peça, listar estados vazio/carregado, sinais de confirmação e retomada sem peça. Resultado: configuração de carga coerente e fluxo definido antes de escolher movimento.


## Complemento do lote ampliado — 2026-10-28

Instalação e cargas dos modelos adicionais: ch36/53. Valores de fixação e capacidade das edições novas não mudam silenciosamente o ativo da edição antiga. Validar revisão/hardware e conexão.
