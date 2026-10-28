# 11 — Pallet Template, buffer e estado de ocupação

**Fontes Core:** CRG p.44; CPP slides 71–79; CWB p.12. **Advanced relacionado:** ch04 e ch06.

## Ideia central

Separe a distribuição de posições do que a ferramenta faz em cada posição. O template do Core usa Pattern para geometria e PalletSequence para aproximação, ação e saída. Isso atende armazenamento/recolha repetitiva sem reensinar manualmente cada ponto.

## Escolha do padrão

| Pattern do curso | Configuração descrita |
|---|---|
| Line | Início, fim e número de posições |
| Square | Quatro cantos e quantidades nos eixos do padrão |
| Box | Oito cantos e distribuição volumétrica |
| List | Posições não regulares ensinadas conforme necessidade |

CRG p.44 mostra Square com 4×3 = **12 peças**. CPP 76 esclarece que cantos devem ser ensinados nas posições das peças dos cantos, não na borda externa da embalagem. A quantidade/configuração do template da versão deve ser conferida: não confunda número de peças com número de intervalos entre elas.

## Pattern versus PalletSequence

1. Defina padrão, contagens e geometria.
2. Ensine os pontos requeridos pelo assistente.
3. Em PatternPoint, defina a posição âncora da operação local.
4. Em Approach e Exit, ensine a aproximação e a retirada que acompanham a âncora.
5. Insira ação de garra e atualização de carga onde apropriado.
6. Defina comportamento ao chegar à última posição.
7. Determine política para reinício e ocupação física.

CPP 77 alerta que inserir um Move arbitrário dentro da sequência pode não ficar relativo à âncora no template demonstrado. Use os pontos/estrutura previstos pelo assistente e confira como pontos adicionais são vinculados. Não generalize a restrição a todos os templates e versões posteriores.

CPP 75 menciona sequências opcionais antes da primeira posição e após a última. Elas não são automaticamente a mesma coisa que BeforeStart do programa principal.

## Exemplo trabalhado — destino ocupado vira buffer

CWB p.12 amplia o pick-and-place: quando a posição de saída está ocupada, armazenar em palete Square em vez de esperar. Opcionalmente, quando não houver nova peça na origem, retirar do buffer e abastecer a saída.

Reconstrução de decisão com condições explícitas:

| Origem | Saída | Buffer | Decisão de exemplo |
|---|---|---|---|
| Tem peça | Livre | Qualquer | Transferir diretamente |
| Tem peça | Ocupada | Tem vaga | Guardar no buffer |
| Sem peça | Livre | Tem peça | Despaletizar do buffer |
| Tem peça | Ocupada | Cheio | Esperar/sinalizar segundo requisito |
| Sem peça | Ocupada | Qualquer | Não depositar em destino ocupado |

O último conjunto de decisões completa lacunas do enunciado; é reconstrução de projeto, não uma política pronta do curso. O WB não define fila FIFO/LIFO, precedência entre atender origem e esvaziar buffer ou recuperação após queda de energia. Decida esses requisitos antes de implementar.

### Conferência dimensional reconstruída

Se 4 posições ocupam uma fileira com extremos separados por 300 mm, há 3 intervalos e espaçamento de 100 mm. Para 3 posições com extremos separados por 200 mm, há 2 intervalos de 100 mm. Um padrão 4×3 contém 12 posições. Os espaçamentos são didáticos e não vêm do dispositivo real do usuário.

## Contadores e reinício

CPP 76 cita contador de percurso; CPP 77 mostra opção **Remember traversal position between program runs** e demonstra reset quando o wizard é abortado sem política de memória apropriada.

- Não trate reset lógico como esvaziamento físico do palete.
- Se a posição é lembrada, confira se o operador removeu peças ou trocou o palete enquanto o programa estava parado.
- Ao alternar paletização/despaletização, mantenha índice, ocupação e estado da peça transportada coerentes.
- O trecho das notas sobre faixa do contador é ambíguo no limite superior: não derive índices inclusivos a partir da frase. Consulte o contador real do template antes de escrever expressões.

## Template ou feature variável?

| Necessidade | Caminho mais próximo do curso |
|---|---|
| Depositar/recolher em conjunto de posições | Pallet Template do Core |
| Repetir trajetória de processo inteira em peças/níveis | Feature variável do Advanced, ch04 |
| Peça acompanha esteira móvel | Conveyor Tracking, ch06 |

CWB Advanced afirma que o paletizador interno não era adequado para o exercício de cola daquela versão. Isso não é proibição geral de templates para qualquer processo; preserve o contexto e escolha pela geometria/versão.

## Antipadrões e conferência

- Ensinar a borda do palete em vez do centro de peças dos cantos.
- Confundir contagens com intervalos.
- Guardar peça em buffer cheio por não ter ramo de exceção.
- Perder âncora ao inserir movimento fora da estrutura relativa.
- Reiniciar contador e colidir com peças ainda presentes.
- Recolher do buffer sem saída livre.

**Exercício:** implementar buffer do CWB p.12 e explicar todos os estados da tabela antes de executar. **Conexões:** ch01 organização; ch05 carga; ch10 reinício; ch12 contagem e lógica.

## Integração Industrial — contagens e confirmação com PLC

**Fontes:** ICWB p.10–11; ICRG p.17, 23–24. O novo módulo publica contagem por SKU e palete pronto, aguarda troca física e então retoma. Reset de contador deve corresponder a palete efetivamente substituído, e não apenas ao momento em que a flag foi enviada.

Workbook usa dois SKUs e 10 peças por SKU; guia mostra três registradores de SKU e outro exercício de caixa com quatro peças. Manter contextos separados. No projeto, definir dono da flag e acknowledgment para não consumir sinal antigo. Ch17 mostra os dois sentidos PLC/UR e ch14/ch16 a associação entre dado de câmera e peça.
