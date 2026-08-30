# Geometria real medida no STEP (Fusion) — SV08 Max

Medições no documento Fusion `Sovol-SV08MAX`, importado de
`STEP/MP4802-0000 ... V8 3d printer.STEP`. Cotas em mm, no sistema do próprio
STEP (piso, base dos pés, em **Z = 127,4**).

> São *bounding boxes*, não colisão real. Servem pra dimensionar e decidir rumo;
> a folga final se confere com os sólidos.

## Cotas gerais

| | Z mín | Z máx |
|---|---|---|
| Máquina inteira (com a guia de filamento do topo) | 127,4 | 983,3 |
| Estrutura (até o topo dos perfis do topo) | 127,4 | 892,8 |
| **Altura estrutural** | | **765,4** |

Footprint do quadro: X −12,7 … 696,3 (**709,0**) · Y 66,8 … 763,9 (**697,1**).
Bate com o catálogo (700 × 710 × 750).

## Subconjuntos (nível 1 da árvore)

| Conjunto | Z | Observação |
|---|---|---|
| `01000` base kit | 127,4 – 492,5 | base, fonte, eletrônica |
| `02000` hot bed kit | 214,1 – **266,6** | **266,6 = topo da superfície de impressão** |
| `03000` z axis kit ×4 | 141,8 – 886,8 | as 4 colunas verticais dos cantos |
| `04000` ab axis kit | 407,7 – 624,2 | o gantry |
| `05000` **top kit** | **858,2** – 983,3 | o "chapéu" — 50 peças, é o que vai subir |
| `06000` extruder kit | 425,0 – **810,0** | topo = chicote + PTFE |
| `10000` filament holder | 380,6 – 603,0 | |
| `11000` auxiliary feeder | 544,5 – 761,3 | traseira esquerda, fora do quadro |

## Quem realmente limita a altura: a umbilical, não a estrutura

Na pose do STEP o bico está em **Z ≈ 425,0**. O que sobe junto com ele:

| Peça que acompanha o cabeçote | Z máx | Folga até 858,2 |
|---|---|---|
| Gantry (estrutura, `04000`) | 624,2 | 234,0 |
| Tubo de PTFE Ø4/2 mm | 797,3 | 60,9 |
| **Chicote do cabeçote** (`喷头线束2`) | **810,0** | **48,2** |

Ou seja: **o gargalo é o arco do PTFE + chicote batendo na tampa**, não colisão
de estrutura. Por isso a solução da comunidade funciona — basta subir a tampa.

## O que sobe

O conjunto `05000` inteiro, ~150 mm:

- 4 perfis do topo `05101` / `05102` / `05105` / `05106` (624,6 mm cada, Z 858,2–892,8)
- 4 cantos `05103` / `05104` (58 × 58, Z 866,0–892,8)
- 2 barras de LED (Z 881,2–887,3, presas sob os perfis)
- **Guia de filamento** `导向件` — Z 884,5–979,9, em X −33,3…28,3 / Y 392,6…432,7
  (lado esquerdo, meio da profundidade). Já hoje é um "puxadinho" de fábrica que
  sai por cima do quadro.
- As tampas de vidro e de alumínio — **não estão no STEP** (peças compradas).

## Consequências

1. Tubo de PTFE e chicote do cabeçote precisam de **+150 mm de sobra**.
2. Mangueira do alimentador auxiliar até a guia de filamento: **+150 mm**.
3. Fiação das barras de LED do topo: **+150 mm**.
4. Fechar a faixa nova no perímetro: **~625 mm × 150 mm por lado**.
5. `printer.cfg`: `position_max` do Z já é **505** — o firmware nunca foi o
   limite. Reconferir o `homing_override` depois.
6. Eixo Z **não muda**: colunas, correias, polias e tensionadores ficam onde estão.

## A conferir antes de fechar o modelo

- Altura útil real da máquina hoje (subir o Z no manual e medir da chapa ao bico).
- Como a tampa de vidro e a de alumínio se apoiam no perfil do topo — não está
  no STEP, precisa medir na máquina ou fotografar o encaixe.
