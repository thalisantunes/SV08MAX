# Peças do riser — estado do modelo

Documento Fusion: **SV08MAX-TOP-RISER** (projeto Ingresse).
Referência dimensional: componente `REF_TOPO_SV08MAX` (197 corpos do STEP original).

## Princípio de projeto

Cada peça **copia a tampa**: o pé cai os mesmos 4 mm dentro do rebaixo do perfil
do topo e encosta na aba externa, então se auto-alinha sem gabarito. O topo da
peça **reproduz o mesmo degrau** 150 mm acima, para vidro e chapa reassentarem
sem nenhuma modificação.

> **Regra:** o assento superior tem que reproduzir **os 21 furos M3** originais,
> não só os de fixação. Assim tudo que hoje é parafusado no topo — inclusive a
> **guia de filamento** (`导向件`, y 392,6…432,7 na lateral esquerda) — sobe junto
> e reparafusa igual. A guia é o caminho do PTFE; ela precisa acompanhar a tampa.

## TRILHO_ESQ_A — feito

Primeira peça modelada. Segmento traseiro da lateral esquerda.

| | |
|---|---|
| Extensão | y 109,06 → 415,36 (**306,3 mm**) |
| Seção | x −12,52 → 5,08 (**17,6 mm**) |
| Altura | z 888,77 → 1042,77 (**154 mm** = 150 de riser + 4 do degrau) |
| Volume sólido | 199,5 cm³ · ~213 g em ASA se fosse maciço |
| Pele externa | 3,2 mm |
| Pé e assento | 8,0 mm de espessura |

Seção, de fora para dentro:
- apoio na aba externa em Z 892,77, de x −12,52 a −5,62
- alívio de 1 mm em x −5,62…−4,82 para não brigar com o raio do degrau
- pé assentado no rebaixo em Z 888,77, de x −4,82 a 5,08 (0,2 de folga no lado externo)
- pele de 3,2 mm subindo em x −12,52…−9,32
- assento superior: piso em Z 1038,77 (x −5,02…5,08) e aba em Z 1042,77 (x −12,52…−5,02)

**Fixação:** rasgos de 3,4 mm de largura × 3 mm de alongamento em Y, nos furos
reais y = 128,81 e 378,81. Rebaixo de Ø6,4 × 4 mm para a cabeça. Sobra 4 mm de
material sob a cabeça + 1,5 de rosca = **reaproveita os M3×6 originais**.

O alongamento é o que absorve o desvio de até 2,5 mm da furação de fábrica.

## A fazer

1. Nervuras ligando pele → assento e pele → pé (a cada ~50 mm), contra fluência
   do ASA a 60 °C e para o assento não ceder com o peso do vidro.
2. Emenda macho-fêmea entre segmentos A e B.
3. Reproduzir os 21 furos no assento superior.
4. Peça de canto (em L) unindo dois trilhos.
5. Espelhar para direita, frente e trás.
6. Verificação de interferência contra o `REF_TOPO_SV08MAX` inteiro.
