# 36 — Modelo, geração e capacidade nominal/expandida

**Fontes:** N01/N02/N06 p.90; N28–N32 p.14; N33–N38 p.15. São páginas físicas das edições fornecidas, não catálogo online. Todas as siglas Nxx estão em sources.md e no inventário expandido.

## Identidade antes da comparação

Registrar nome completo, serial, controlador, firmware, PolyScope 5/X, varianteOEM/padrão e ferramenta. UR10CB3, UR10e10 kg, UR10e12,5 kg e UR12e não são identificações intercambiáveis. Nome comercial não é fórmula para deduzir carga: N31UR15 informa17,5 kg e N36UR7e informa7,5 kg.

| Modelo da fonte | Carga nominal indicada | Alcance | Massa do braço | Fonte física |
|---|---:|---:|---:|---|
| UR3/CB3 | 3 kg | 500 mm | 11 kg | N01 p.90 |
| UR5/CB3 | 5 kg | 850 mm | 18,4 kg | N06 p.90 |
| UR10/CB3 | 10 kg | 1300 mm | 28,9 kg | N02 p.90 |
| UR3e | 3 kg | 500 mm | 11,1 kg | N38 p.15 |
| UR5e | 5 kg | 850 mm | 20,7 kg | N37 p.15 |
| UR7e | 7,5 kg | 850 mm | 20,7 kg | N36 p.15 |
| UR10e | 10 ou12,5 kg: conferir etiqueta | 1300 mm | 33,3 kg | N35 p.15,94 |
| UR12e | 12,5 kg | 1300 mm | 33,3 kg | N33 p.15 |
| UR16e | 16 kg | 900 mm | 33,1 kg | N34 p.15 |
| UR8Long | 8 kg; até10 kg condicionado | 1750 mm | 44,7 kg | N32 p.14,96–98 |
| UR15 | 17,5 kg | 1300 mm | 40,7 kg | N31 p.14 |
| UR18 | 18 kg | 950 mm | 39,2 kg | N29 p.14 |
| UR20 | 20 kg; até25 kg condicionado | 1750 mm | 64 kg | N30 p.14,97–99 |
| UR30 | 30 kg; até35 kg condicionado | 1300 mm | 63,5 kg | N28 p.14,94–96 |

Esses números orientam identificação e pré-seleção, sem comprovar viabilidade do ciclo. A carga inclui ferramenta, suporte, peça, mangueiras e demais elementos relevantes. O alcance nominal não é garantia de qualquer orientação, força ou ausência de singularidade.

## Carga expandida é uma região de operação

N28 p.96, N30 p.99 e N32 p.98 condicionam a expansão à ferramenta orientada verticalmente para baixo, CoG dentro do alcance horizontal nominal e offset horizontalXY dentro da curva expandida. A montagem do braço pode ter outra orientação; a condição física da carga permanece. Offsets emZ eXY têm papéis diferentes; não usar apenas a norma3D para substituir a leitura do gráfico.

Curvas N28 p.95, N30 p.98 e N32 p.97 distinguem desempenho completo de carga expandida. Foram conferidas visualmente, mas não digitalizadas para interpolação numérica. Não entregar função linear ou interpolação certificada a partir da miniatura. Carga ampliada pode reduzir velocidade/aceleração e limitar movimentos; verificar a trajetória inteira, CoG e inércia.

UR10e N35 p.94 pede confirmar capacidade na etiqueta. O texto vincula cargas acima10 kg à geometria/condição descrita e à orientação para baixo; não converter a presença de12,5 kg no manual em autorização para atualizar qualquer UR10e10 kg.

## Outras diferenças úteis

UR3/UR3e têm rotação ilimitada da flange no escopo das tabelas; isso não autoriza enrolar cabos da ferramenta. Limites do cotovelo de versões novas diferem dos manuais antigos. Fontes N28–N32 distinguem IP do braço, caixa e pendant; classificação de um componente não se transfere aos demais. Corrente de ferramenta também muda: conferir pinos e alimentação por modelo, não só12/24V.

**Método:** identidade → carga por estado/curva → orientação e alcance → dinâmica → instalação/interfaces → ensaio da aplicação. Entregar parâmetros com fonte e pendências, sem substituir projeto pelo maior número de carga disponível.
