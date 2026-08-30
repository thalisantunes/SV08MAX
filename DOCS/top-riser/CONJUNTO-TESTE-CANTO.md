# Conjunto de teste do canto traseiro-esquerdo

Três peças que se montam no canto real da máquina. Substitui o gabarito de 60 mm
(que só testava o perfil) e valida **tudo que tem risco de projeto**.

| Arquivo | Peça | Volume maciço | Extensão |
|---|---|---|---|
| `RISER-01-CANTO-TRAS-ESQ.stl` | Canto em L com as duas linguetas | 75,9 cm³ | x −12,52…53,48 · y 697,66…763,66 |
| `RISER-02-STUB-ESQ.stl` | Trecho do trilho esquerdo | 40,3 cm³ | y 661,66…721,66 (60 mm) |
| `RISER-03-STUB-TRAS.stl` | Trecho do trilho traseiro | 40,0 cm³ | x 29,48…89,48 (60 mm) |

Total ~156 cm³ maciços → **cerca de 96 g de ASA** com 4 paredes e 25%.
Todos em `_work/`.

## Furos de fixação usados (todos já existem na máquina)

| Peça | Furo M3 real |
|---|---|
| Stub esquerdo | y = 701,91 (x 1,53) |
| Stub traseiro | x = 49,23 (y 749,61) · x = 74,48 (y 751,36) |

Rasgos de 3,4 × 3 mm com rebaixo Ø6,4 × 4 → **reaproveitam os M3×6 originais**.

## Emendas

Lap joint: a lingueta do canto entra no vazio do trilho e encosta por dentro na
pele. Dois furos Ø3,4 passantes por emenda, em z 930 e z 1000 — **M3×12 com
porca** se quiser travar (o teste de encaixe funciona sem).

## Verificação (feita antes de exportar)

| | Contra a máquina | Entre as peças |
|---|---|---|
| Canto | **0** | — |
| Stub esquerdo | 3,672 mm³ * | — |
| Stub traseiro | **0** | — |
| Os três juntos | — | **0** |

\* 3,672 mm³ espalhados por 60 × 17,6 mm de contato = **0,0035 mm** de espessura
média. Ruído numérico de face coincidente, não interferência.

### O que a verificação pegou e foi corrigido

**Colisão real de 187,7 mm³ entre o canto e o trilho traseiro** — as nervuras do
trilho, em x = 36, batiam na lingueta de emenda do canto (x 25,48…53,48).
Corrigido movendo as nervuras para x = 60 e 82.

> **Regra que sai daí:** nenhum trilho pode ter nervura nos 25 mm de cada ponta,
> que é onde moram as linguetas. Vale para os 8 trilhos definitivos.

## Impressão

H2D, bico **0.4**, **ASA branco** (contraste contra a moldura escura deixa a
linha de contato visível). Deitados sobre a face externa lisa, sem suporte.
4 paredes, 25%, camada 0,25.

## O que testar com as três na mão

1. Cada peça isolada cai no rebaixo e assenta sem balançar?
2. Os M3×6 originais rosqueiam nos três furos?
3. As linguetas entram no vazio dos trilhos sem forçar?
4. Com as três montadas, o **degrau do assento superior fica contínuo** ao virar
   o canto? (passar uma régua ou uma quina de vidro por cima)
5. A pele externa fica rente à aba nos três, sem degrau entre peça e peça?
6. O canto assenta no assento 58 × 58 sem cavalgar nos parafusos escareados?
