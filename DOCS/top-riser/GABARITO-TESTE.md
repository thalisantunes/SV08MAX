# Gabarito de teste — primeira peça a imprimir

Arquivo: `_work/GABARITO-TESTE-riser.stl` (17,6 × 60 × 154 mm, 40,4 cm³ maciço).

É uma **fatia de 60 mm do trilho esquerdo**, em y 110 → 170, contendo o furo real
de fixação em **y = 128,81**. Serve pra validar o encaixe inteiro antes de gastar
~1,2 kg de ASA nas 12 peças definitivas.

## Verificação de interferência (contra os 159 corpos vizinhos do STEP)

| Peça | Interferência |
|---|---|
| `CANTO_TRAS_ESQ` | **0** |
| `GABARITO_TESTE` | 3,6 mm³ contra `PERFIL_TOPO_4_esq` |

Os 3,6 mm³ estão espalhados por toda a área de contato (60 × 17,6 mm) → espessura
média de **0,003 mm**. É ruído numérico de face coincidente, não interferência real.

### O que o teste pegou

1. **O trilho A colidia com a guia de filamento** — 3.120 mm³, em y 392,66…415,36.
   O segmento A ia até y 415,36 e a guia ocupa y 392,6…432,7. Confirma que a guia
   **tem que subir** para o assento do riser antes de imprimir os trilhos definitivos.
2. O gabarito começava em y 100 e invadia o **assento do canto frontal-esquerdo**
   (`Corpo134`, y 66,86…124,86). O perfil esquerdo só começa em **y 109,06**.
3. Uma lasca contra o raio interno do degrau → aliviada com chanfro de 1,2 mm e
   folga aumentada de 0,2 para **0,4 mm**.

## Como imprimir

- **ASA** (a peça vive numa câmara a 60 °C)
- Deitado sobre a **face externa lisa** (a de 60 × 154). Altura fica 17,6 mm,
  sem suporte — todos os balanços ficam a 45° ou mais
- Bico **0,6 mm** (o rasgo tem 3,4 mm de largura; com 1,0 fica marginal)
- 4 paredes, 25% de preenchimento, camada 0,25
- ~21 g, cerca de 1 h

## O que conferir com a peça na mão

1. O pé cai no rebaixo e assenta **sem balançar**?
2. A pele externa encosta rente na aba, sem degrau visível por fora?
3. O **M3×6 original** passa pelo rasgo e rosqueia no furo de y 128,81?
4. O rasgo tem folga lateral suficiente pra absorver o desvio de furação?
5. O degrau do assento novo mede **4,0 mm** de profundidade × 10,1 de largura?
6. Uma quina do vidro assenta no degrau novo?

Se os seis passarem, os outros 11 componentes são o mesmo perfil espelhado.


---

## Decisão de máquina e material (29/08)

**Tudo vai na H2D com bico 0.4, em ASA.** Volume da H2D: 325 × 320 × 325 (bico
único) / 300 × 320 × 325 (duplo). As peças definitivas são 306,3 × 154 × 17,6
deitadas — **cabem nos dois modos**.

**O bico 1.0 da SV08 Max não serve para as peças definitivas.** O projeto tem
pele de 3,2 mm e nervuras de 2,4 mm: com 1.0 a pele vira 3 perímetros sem miolo
e as nervuras viram 2 linhas, perdendo a rigidez que deveriam dar. O rasgo de
3,4 mm também fica grosseiro. Bônus: imprimindo na H2D, a SV08 Max fica livre —
e ela é justamente a máquina que vai ficar desmontada na hora de instalar o riser.

**Cor: gabarito em BRANCO.** Não é estética — é instrumento de medição. Contra a
moldura anodizada escura, o branco deixa a linha de contato visível: dá pra ver
fresta, folga e assentamento a olho nu, e a foto volta legível para análise.
Preto contra preto esconde exatamente o que precisamos enxergar.

Para as peças definitivas a conversa é outra: **preto** integra com a máquina e
perdoa uma junta imperfeita; branco cria uma faixa de 150 mm bem visível em volta
do topo e denuncia qualquer fresta. Decidir depois que o gabarito passar.
