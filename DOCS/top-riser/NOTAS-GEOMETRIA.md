# Top riser SV08 Max — levantamento de geometria

Objetivo: recuperar os ~150 mm de altura de impressão que a estrutura do topo
rouba, elevando o conjunto superior (perfis + painéis) da máquina.

## Perfis do topo — medidas tiradas dos desenhos oficiais

Fonte: `PDF/JXHMP4802-051xx-a 顶部型材N.pdf`. Material de todos: **6063-T5**,
anodizado jateado cinza PANTONE 430C. Furação toda em **M3**.

| Peça | Desenho | Comprimento | Seção | Observações |
|---|---|---|---|---|
| 顶部型材 1 | `JXHMP4802-05101-a` | **624,6 ±0,1** | 34,5 alt. × 9 base, aba de 16,5 / 21,5 | Face com laser "SOVOL". Furos: 4×M3 passante + 2×M3 ⌀2,5 cegos; passo 110 / 314,6 / 110 |
| 顶部型材 2 | `JXHMP4802-05102-a` | **624,6** (612,6 entre extremos de furo) | 34,5 × 18 (aba 9) | 4×M3 passante + 4×M3 ⌀2,5; passo 184,8 / 200 / 184,8 |
| 顶部型材 3 | `JXHMP4802-05105-a` | **624,6 ±0,4** | 34,5 × 9 (aba 21,1 / 16,5) | 4×M3 ⌀3,5 + 4×M3 passante; passo 195 / 200 / 195 |
| 顶部型材 4 | `JXHMP4802-05106-a` | 624,6 | espelhado do 3 | — |
| Guias 1 e 2 | `JXHMP4802-05108/05109` | — | — | 导向件 (peças de guia dos painéis) |

**Consequência de projeto:** o quadro do topo é um quadrado de ~624,6 mm de
lado e o perfil **não é 2020/3030 padrão** — é extrusão proprietária de 34,5 mm
de altura com aba/canal para encaixe do painel. Qualquer riser precisa
reproduzir esse encaixe ou fazer um adaptador para perfil comercial.

## Referência do Cults3D (cantos impressos + acrílico laser)

- Ganho: recupera os ~150 mm perdidos.
- Peças impressas (~1 kg em ABS/ASA/PETG): 4 cantos + 4 "bridges" (ligação no
  topo dos painéis) + 4 apoios centrais frame↔painel.
- Acrílico 3 mm: 4 painéis frontais/traseiros e laterais (~625 × 150 e 613 × 154).
  Arquivos em DXF, SVG e Lightburn.
- Ferragem: 15× M3×10, 32× M3×6, 42× insertos térmicos M3×4,5 + parafusos originais.
- Impressão sugerida: bico 0,6 mm, 20–25% preenchimento, 3–5 paredes, 5–6 camadas
  topo/base; material tem que aguentar 60 °C+.
- **Compensar contração nos "bridges":** PETG 100,5% · ASA 100,7% · ABS 101%.
  Imprimir 1 bridge de teste antes dos 4.
- Variantes com "connector nubs" e receptáculos nos cantos/bridges, e Z3 com
  furo + plug (ver `Referências/top-riser-geo/2025-09-29-09_43_41-f.png`).

## Decisões em aberto

1. Copiar a abordagem canto impresso + acrílico, ou usar extrusão de alumínio
   comercial (mais rígido, mas precisa de adaptador para o perfil proprietário)?
2. Altura do riser: exatos 150 mm ou parametrizar (ex.: 100 / 150 / 200)?
3. O que fazer com a fiação/cable chain do eixo Z e com o duto/exaustor do topo
   ao subir o conjunto.
4. Impacto no `printer.cfg`: `position_max` do Z e `safe_z_home`.
