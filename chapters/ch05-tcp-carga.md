# 5 — TCP, payload e centro de gravidade

**Fontes:** PPP slides 87–100; RG p.2–5 e 25–28; WB p.4 e 24; AP p.181–197.

## Ideia central

Modele separadamente a geometria da ferramenta e a carga transportada. O ponto ativo pode mudar sem que a massa mude; pegar uma peça pode mudar massa e CoG sem que seja necessário redefinir a geometria do bico.

## Conceitos e decisões

| Grandeza | O que representa | Quando rever |
|---|---|---|
| TCP posição | Ponto de trabalho em relação ao flange | Troca de ferramenta/bico, montagem alterada |
| TCP orientação | Orientação de trabalho | Ferramenta inclinada ou outro eixo de processo |
| Payload | Massa da ferramenta + peça transportada | Pegar/soltar peça, mudança física da montagem |
| CoG | Localização do centro de gravidade da carga | Redistribuição de massa ao pegar/soltar |

As definições introdutórias do RG falam no peso da ferramenta; RG p.27 explicita que **total payload = ferramenta + peça**. Use a condição física atual.

PPP 93 relaciona cálculo de correntes ao mounting, payload e CoG. Uma carga mal configurada pode produzir protective stop mesmo sem colisão. Isso não prova que toda parada seja de carga; identifique primeiro evento e circunstância.

## Calibração de posição do TCP

RG p.25 e PPP 89:

1. Monte a ferramenta e identifique o bico que será calibrado.
2. Abra Installation → General → TCP, conforme a interface do material.
3. Abra o assistente de posição.
4. Leve o mesmo ponto físico da ferramenta a uma referência fixa, com orientações distintas.
5. Registre pelo menos três posições; RG fornece uma quarta para maior precisão.
6. Finalize o cálculo e confira a repetibilidade em torno da referência.

O requisito geométrico é manter o ponto físico na mesma referência e variar a orientação. Mover para locais diferentes não é a calibração ensinada. A skill não inclui tolerância numérica de aceitação, ausente nas fontes.

## Calibração de orientação

RG p.26 e PPP 90:

1. Abra o assistente de orientação do TCP.
2. Selecione o referencial desejado.
3. Alinhe o eixo de trabalho da ferramenta com Z do referencial selecionado.
4. Registre e confirme a orientação.

Posição e orientação são etapas distintas. Não atribua rx/ry/rz de uma pose como se fossem ângulos RPY independentes; veja ch03.

## Identificação de carga

RG p.27 apresenta um assistente que usa quatro posições com orientações distintas para estimar payload/CoG. Faça-o no estado de carga pretendido. O curso não fornece aqui um algoritmo para calcular o CoG combinado por conta própria ou uma tabela para todas as peças.

### Comandos legados das fontes

```urscript
set_payload_cog([CX, CY, CZ])
set_payload(payload, [CX, CY, CZ])
```

São assinaturas documentadas no RG p.28, com nomes simbólicos. `CX/CY/CZ` usam **metros** e `payload` usa **kg**. O bloco é referência, não um script autônomo com variáveis já preenchidas.

| Desejo | Recurso do material |
|---|---|
| Corrigir apenas CoG | `set_payload_cog(...)` |
| Corrigir massa e CoG | `set_payload(m, [...])` |
| Trocar TCP em programa | Nó Set do PolyScope |
| Medir carga | Assistente de payload/CoG |

PPP 96 destaca que o Set usado para trocar TCP não ajusta CoG naquele fluxo. Slides 92–95 mencionam associação de CoG ao TCP quando não informado; por envolver comportamento legado, não dependa dessa omissão sem confirmar a versão. Prefira dados explícitos e documentados para o estado atual.

## Exemplo trabalhado — cola com dois componentes

WB 5.1 usa uma ferramenta de dois bicos:

1. Monte o conjunto e calibre posição e orientação do primeiro TCP.
2. Calibre o segundo TCP separadamente.
3. Ensine início/fim e o logo com o TCP escolhido, usando MoveP para o processo no exercício.
4. Após a primeira passagem, selecione o segundo TCP.
5. Execute o processo com o TCP correspondente e alterne novamente no próximo ciclo.
6. Confira que o ponto efetivamente usado é o bico selecionado; examine aproximação e retirada para os dois.

O benefício é reutilizar a geometria do processo sem fingir que dois bicos ocupam o mesmo lugar no flange. Se ambos ficam montados o tempo todo, trocar o TCP ativo não significa automaticamente mudar a massa física do conjunto.

### Exemplo de conversão com dados de bancada

WB p.4 fornece para o kit ROBOTIQ 2-Finger 0,950 kg e CZ=45 mm, CX=CY=0. A representação em metros para CoG é `[0, 0, 0.045]`. Isso apenas converte unidades do dado de treinamento; não confirma que a montagem real do usuário tem essa massa/CoG. Ao agarrar peça, é necessário obter o conjunto carregado.

## Antipadrões

- Confundir ponto de contato com centro de gravidade.
- Trocar TCP esperando que o controlador descubra a peça agarrada.
- Informar 45 em comando que espera 0,045 m.
- Aplicar massa da ferramenta vazia durante transporte de peça.
- Calibrar vários pontos físicos diferentes como se fossem um único TCP.
- Copiar carga de uma tabela de kit sem conferir adaptador, bico e montagem.
- Tratar comandos legados como recomendação atual para qualquer versão.

## Exercício e conferência

WB 5.1, p.24: alternar TCPs no logo. Confira identidade do bico, orientação, repetibilidade e estado de carga. Não há teste em robô/URSim incluído na skill.

**Conexões:** ch01 configuração; ch03 representação angular; ch04 trajetória relativa; ch07 influência dos dados de carga na aplicação de força.

## Integração Core — estado inicial, calibração e Set

**Fontes:** CRG p.6–11, 19, 49–50; CPP 13, 92–93; CWB p.2, 15. Na instalação, os dados devem corresponder ao estado inicial real: se começa vazio, considerar ferramenta vazia; se começa com peça, representar esse conjunto. O Core reforça atualização de carga ao pegar/soltar.

CRG p.19 mostra Set the total payload to no nó Set. Isso complementa a troca de TCP pelo mesmo tipo de nó: alterar TCP e alterar massa são ações distintas, e o procedimento não comprova atualização automática de CoG. Mantenha a separação entre geometria, massa e centro de gravidade.

CPP 92 explica que as orientações da calibração de posição precisam ser suficientemente diversas e apresenta LEDs de qualidade dos pontos; quarta pose pode verificar a solução. CPP 93 usa quatro posições na estimativa de massa/CoG e pede conferir se o resultado é razoável antes de aplicar. Um wizard concluído não substitui verificar montagem e estado de carga. CWB p.2 pede conferir TCP por rotação em torno da referência, sem tolerância numérica universal fornecida.

O TCP usado no processo também pode ser copiado para Tool Position de segurança; CPP 28 alerta que mudanças posteriores podem deixar as cópias dessincronizadas (ch08).


## Complemento dos manuais — 2026-10-28

SM511 p.66–69 marca setters antigos como obsoletos desde 5.10 e recomenda set_target_payload com massa, CoG e inércia. U5E p.226–227 confirma cargas nomeadas e assistente de quatro posições; DEP p.21–22 situa o nó Set Payload em 5.10+. Trocar carga nomeada inclui os dados configurados; isso resolve a lacuna do curso sem afirmar o mesmo para versões antigas. Ver ch27.


## Complemento CB3 e artigos de referência — 2026-10-28

SM341 p.28 confirma CoG em metros desde toolmount e dependência do TCP quando CoG é omitido em set_payload. Não confundir calibraçãoTCP com identificação cinemáticaCAL/ch33. A recomendação set_target_payload permanece específica SW5.11.


## Complemento do lote ampliado — 2026-10-28

Novosmodelos e cargas expandidas emch36. N10p.86–87 acrescenta transition_time e ressalva de tara ao final; não copiar assinatura nova paraSW5.11. N12p.135 descreve programa continuando durante ajuste. Verch43.
