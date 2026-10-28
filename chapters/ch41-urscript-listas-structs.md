# 41 — URScript5.25/5.26: listas, structs e matrizes

**Fontes:** N10Script Directory5.26 e N26Script Directory5.25, p.17–23 e100–101. Esses recursos ampliam os contextos3.4.1/SW 5.11, sem os reescrever retroativamente.

## Tipar pelo contrato do dado

struct agrega membros nomeados e inicializados com tipos que não mudam depois. Pode conter lista e struct aninhada. RPC pode retornar struct; a menção a mensagemROS2 é explicitamentePolyScope X, não suporteROS2 automático emPolyScope 5. Acesso por nome facilita revisão de unidade/frame e reduz listas posicionais ambíguas.

Lista tem length atual e capacity fixa após criação. Literal[1,2,3] nasce com comprimento/capacidade3; make_list permite reservar maior capacidade. A atribuição para lista existente conserva a capacidade do destino e só cabe se ela acomodar o comprimento da origem. Reserva grande não elimina validação de tipo, índice ou volume de dados.

N10 p.18–19 diz passagem/retorno por cópia de valor; elementos não mudam de tipo e listas de listas não são suportadas como coleção geral, pois representam matrizes. Structs numa lista precisam ter tipo compatível idêntico. Não aplicar referências mutáveis dePython ou array dinâmico ilimitado.

## Métodos com capacidade explícita

N10 p.19 situa métodos a partir dePolyScope 5.15. append/extend/insert podem falhar ao exceder capacidade. clear reduz comprimento sem aumentar reserva; pop/remove alteram conteúdo. slice usa fim exclusivo e retorna cópia. Método de matriz get_row/get_column também retorna por valor; editar a cópia não altera automaticamente a matriz original.

Exemplo reconstruído, apenas dados, compatível com o contexto documentado:

```urscript
amostras = make_list(0, 0.0, 3)
if amostras.excess_capacity() > 0:
  amostras.append(0.25)
end
leitura = struct(valor=0.25, unidade="m", valido=True)
```

Não foi executado emURSim/robô. A condição protege capacidade, não valida a medição ou atualidade. Para receber dados externos, adicionar unidade/frame, quantidade esperada, estado de validade e identificação da amostra.

## Matrizes e transformação

N10 p.22–23 descreve expressões de matriz/array; p.111 traz transpose e wrench_trans. Distinguir produto matricial, operação elemento a elemento, composição de poses e transformação de wrench. A torque resultante de mudar ponto de referência inclui braço de alavanca; não reinterpretar uma lista6D como pose só por ter seis números.

O manual contém exemplos longos cuja indentação pode se perder noPDF. Revisar assinaturas/capacidade antes de migrar. Exemplo de retorno de função deve ter destino inicializado com reserva suficiente, evitando falha apenas quando a câmera fornecer mais alvos.

**Exercício:** armazenar até3 medidas e recusar a quarta sem erro de capacidade; explicar por que copiar uma struct/lista para uma função não cria referência compartilhada comoPython.
