# Interface do assento das tampas — medida no STEP

Coordenadas do STEP (piso em Z 127,4). Medido face a face nos 4 perfis do topo,
não em desenho. É a superfície onde o vidro e a chapa de alumínio se apoiam hoje
e onde o riser vai se fixar.

## Seção do assento — idêntica nos 4 lados

| Elemento | Z | Largura |
|---|---|---|
| Aba externa (rim) | **892,77** | 7,1 a 7,5 mm |
| Canaleta na aba (vedação/estética) | 891,27 | 4,1 mm × 1,5 fundo |
| **Piso do rebaixo — onde a tampa apoia** | **888,77** | **10,1 mm** |

**Degrau = 892,77 − 888,77 = 4,00 mm exatos** → a tampa tem **4 mm** de espessura
e fica rente à aba externa. Isso vale pro vidro e pra chapa.

## Vão e envelope das tampas

| | de | até | medida |
|---|---|---|---|
| Vão interno (borda interna do rebaixo) X | 5,08 | 678,48 | **673,40** |
| Vão interno Y | 84,66 | 746,06 | **661,40** |
| Envelope da tampa (entre faces internas das abas) X | −5,42 | 688,98 | **694,40** |
| Envelope da tampa Y | 74,36 | 756,56 | **682,20** |

Ou seja: as tampas somadas cobrem ~694 × 682, apoiadas numa borda de 10,1 mm
em todo o perímetro.

## Furação existente — 21 furos M3

Todos **Ø2,5 mm, 1,5 mm de profundidade** (roscados M3), no piso do rebaixo em
**Z 888,77**. São os pontos de fixação do riser — **não precisa furar nada**.

| Lado | Coordenada fixa | Posições |
|---|---|---|
| Frente | y = 77,86 | x = 49,23 · 234,33 · 434,33 · 634,33 |
| Trás | y = 749,61 | x = 49,23 · 341,78 · 634,33 |
| Trás | y = 751,36 | x = 74,48 · 184,48 · 499,08 · 609,08 |
| Esquerda | x = −0,97 | y = 128,81 · 378,81 |
| Esquerda | x = −0,72 | y = 400,16 · 425,16 |
| Esquerda | x = 1,53 | y = 501,91 · 701,91 |
| Direita | x = 684,53 | y = 128,81 · 378,81 |
| Direita | x = 682,03 | y = 501,91 · 701,91 |

> ⚠ **A furação não é colinear.** Na esquerda o x varia de −0,97 a 1,53 (2,5 mm
> de desvio) e na traseira o y varia de 749,61 a 751,36. Os furos da metade da
> frente e da metade de trás têm offsets diferentes. **Os furos passantes das
> peças impressas têm que ser rasgos** (Ø3,4 × 3 mm de rasgo), não furos redondos,
> senão não fecha.

## Arquitetura proposta

O riser vira uma moldura de 150 mm com 4 famílias de peças, todas em ASA:

| Peça | Qtd | Função |
|---|---|---|
| **Canto** | 4 | Peça em L. Pé que encaixa no rebaixo de 4 mm (auto-alinha sozinho) + coluna + assento superior reproduzindo o degrau de 4 mm × 10,1 mm |
| **Trilho** | 8 (2 por lado) | Segmento de ~300 mm com o mesmo perfil do canto, emenda macho-fêmea no meio do lado, onde fica o montante central |
| **Painel** | 8 (2 por lado) | ~315 × 150 × 3 mm, encaixa em canaleta nos trilhos |
| **Tampa de topo** | — | Reaproveita o vidro e a chapa de alumínio originais, sem modificação |

O pé de cada peça copia a tampa: cai 4 mm dentro do rebaixo e encosta na aba
externa. Ele se posiciona sozinho, sem gabarito.

O topo de cada peça reproduz o degrau original — piso a +150 mm do 888,77 e aba
4 mm acima — para o vidro e a chapa assentarem exatamente como assentam hoje.

## A confirmar na máquina

- Onde termina o vidro e começa a chapa de alumínio (medir do batente traseiro).
- Se existe fita de vedação no rebaixo hoje (a canaleta em 891,27 sugere que sim).
- Posição do furo oblongo de saída do PTFE na chapa de alumínio.


## Guia de filamento — ocupa a lateral esquerda

Os dois furos fora do padrão na esquerda (**y = 400,16 e y = 425,16**) não são da
tampa: são da **guia de filamento** (`导向件`), que no STEP ocupa
**x −33,3…28,3 · y 392,6…432,7 · Z 884,5…979,9**. Ela é parafusada no rebaixo do
perfil esquerdo e é o "puxadinho" de fábrica por onde o PTFE sai.

**Consequência:** o montante central do trilho esquerdo **não pode** cair no meio
do vão (y ≈ 415) — bate na guia. Tem que usar os furos de y = 378,81 ou 501,91.

## Layout de montantes derivado dos furos reais

Cada poste cai em cima de um furo M3 que já existe:

| Trilho | Cantos | Montantes intermediários |
|---|---|---|
| Frente | x 49,23 e 634,33 | **x 234,33 · 434,33** (dois) |
| Trás | x 49,23 e 634,33 | **x 341,78** (um, no centro) |
| Esquerda | y 128,81 e 701,91 | **y 378,81 · 501,91** (desviando da guia) |
| Direita | y 128,81 e 701,91 | **y 378,81 · 501,91** |

Vão livre máximo resultante: ~200 mm entre apoios. Confortável para painel
impresso de 3 mm.

## Emenda vidro / chapa de alumínio

Leitura de foto com trena: **~360 mm da borda traseira externa** (y ≈ 404 no
sistema do STEP). ⚠ Não confirmado com instrumento — conferir antes de cortar
geometria dos painéis. Não afeta os montantes (o mais próximo fica em y 378,81).

Furo oblongo de saída do PTFE na chapa: ~45–50 mm de comprimento, na metade
direita da chapa. A medir com precisão se formos refazer a chapa.
