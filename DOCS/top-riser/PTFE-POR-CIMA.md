# Alimentação de filamento pelo topo — pesquisa

Objetivo do Thalis: tirar o filamento da lateral e alimentar por cima, com a
bobina na prateleira acima da máquina.

## Resumo honesto da cobertura

**Não existe solução consolidada e testada pra isso na SV08 Max.** O que há:

| Evidência | Status |
|---|---|
| [SV08 Max Auxilliary Feeder Top Mount](https://www.printables.com/model/1491617-sv08-max-auxilliary-feeder-top-mount) | Faz exatamente isso — move o alimentador auxiliar pro topo, reaproveitando os parafusos do acoplador PTFE superior. Recomenda **tubo PTFE único de 3 mm ID** do alimentador direto ao hotend. Precisa de extensão 4 vias Molex Micro-Fit 3.0 (~200 mm). ⚠ **1 make, 0 reviews** |
| Bobina no teto **sem PTFE nenhum**, curvas de ~500 mm de raio ([fórum Sovol](https://forum.sovol3d.com/t/external-feeder-assist-replacement/8713)) | Proposta de uma pessoa, **não construída** |
| [Building SV08 Enclosure, but what about the filament?](https://forum.sovol3d.com/t/building-sv08-enclosure-but-what-about-the-filament/9335) | Passagem no centro do topo + drybox acima. Descrito, **sem medidas nem resultado** |
| Threads de riser/top hat da SV08 Max | **Não tocam no assunto de filamento** |

Ninguém relatou "deu errado" porque quase ninguém fez. Provavelmente vamos ser
quem documenta.

## O número que manda no projeto: raio de curvatura

Estudo da Dyze Design, [The Adverse Effects of PTFE Restrictions](https://dyzedesign.com/2023/01/the-adverse-effects-of-ptfe-restrictions/) — tubo 2×4 mm, curva de 180°, força medida:

| Raio | Atrito |
|---|---|
| 50 mm | ~0,5 N |
| 30 mm | ~2 N |
| 10 mm | **extrusor patina** |

Mínimo absoluto citado: **15 mm**. O crescimento é **exponencial**, não linear —
de 50 para 30 mm a força quadruplica.

> **Regra de projeto: ≥ 50 mm de raio. Nunca abaixo de 30.**
> Os 150 mm do riser dão espaço de sobra; economizar espaço aqui custa caro.

PLA deu força maior que PETG em todos os raios — é o pior caso. ASA é tolerante.

## O risco real não é térmico

| | |
|---|---|
| Temperatura de serviço do PTFE | **260 °C** |
| Câmara aquecida da SV08 Max | ~50–60 °C |

Margem de 200 °C. **Não há risco térmico de amolecimento do tubo.** O risco
térmico do projeto são as **peças impressas** — tudo dentro ou no topo da câmara
tem que ser ABS/ASA/PETG/PLA-HT, nunca PLA comum.

## O risco real é o alimentador auxiliar

O buffer da SV08 Max **não** é sincronizado com o extrusor. É um sistema de
**posição por sensor** mola-gravidade: empurra `push_length: 25` mm, uma aba
bloqueia o IR do switch superior, o filamento é consumido, a aba desce até o
switch inferior, e ele empurra de novo.

Isso depende de a resistência a jusante ser **baixa e previsível**. Já é o
problema nº 1 relatado da máquina:

- [Auxiliary feeder issue](https://forum.sovol3d.com/t/auxiliary-feeder-issue/9677): erros de "blockage" **sem entupimento real**; setup problemático usava 80 cm de Capricorn
- [Filament feeder failures](https://forum.sovol3d.com/t/filament-feeder-failures/8708): resíduo acumulado dentro do tubo após **8 h** de impressão ("polvilhado de neve artificial")
- Boa parte da comunidade simplesmente **abandona ou substitui** o alimentador auxiliar

Alternativa com precedente: [SV08MAXCustomFeeder](https://github.com/Chineko-Koizumi/SV08MAXCustomFeeder) — substituto completo, corpo em ABS, modos ativo e passivo. ⚠ Desabilitar exige mexer na config; só desconectar o cabo faz a impressora recusar imprimir.

## Bobina acima — consenso e não-consenso

**Consenso:** peso de bobina apoiado no painel da tampa **degrada a qualidade por
vibração** ([fórum Voron](https://forum.vorondesign.com/threads/this-is-a-stupid-question-p-but-any-concern-with-placing-my-filament-box-on-top-of-my-2-4-350.1222/)). Prateleira independente resolve — e é justamente o caso dele.

**Consenso:** curva antes do extrusor é ponto de fratura de filamento quebradiço
([Bambu Lab, oficial](https://wiki.bambulab.com/en/x1/troubleshooting/filament-breaks-in-path)). Ironia: **secador externo deixa o filamento mais quebradiço** — o combo "secador + curva de entrada por cima" é o cenário de risco. PLA é o pior caso.

**Problema documentado da alimentação suspensa** ([fórum Prusa](https://forum.prusa3d.com/forum/original-prusa-i3-mmu2s-mmu2-general-discussion-announcements-and-releases/overhead-filament-feed/)): o peso do tubo PTFE + filamento cria força descendente constante que causa "pop-back" (o filamento volta alguns cm ao desengatar). **Solução que funcionou: suspender a ponta do tubo com barbante e clipes**, aliviando o peso em vez de pendurá-lo no acoplador.

**Sem dados publicados:** altura mínima, ângulo de entrada, e se polia de desvio
ganha de um raio generoso. Dado o estudo Dyze, raio grande e livre provavelmente
vence a complexidade da polia.

## O que isso implica pro nosso riser

1. Reservar espaço pra curva de **≥ 50 mm de raio** na saída superior.
2. Decidir cedo o destino do alimentador auxiliar — manter na traseira e ainda
   empilhar um caminho novo por cima soma arrasto num mecanismo já frágil.
3. Prever **alívio mecânico do peso** do trecho vertical de PTFE (gancho/suporte),
   não pendurar no acoplador.
4. Peça de passagem no topo em ASA, nunca PLA.
5. A bobina vai na prateleira, **nunca apoiada na tampa**.

## Nota

Já existe um [riser de 150 mm todo impresso pra SV08 Max](https://cults3d.com/en/3d-model/various/sovol-sv08-max-150mm-raiser-all-printed-no-acrylic) e um [top hat de 100/150 mm](https://forum.sovol3d.com/t/3d-printable-sv08-max-top-hat-riser-coming-to-printables-soon/8706) testado com Z em 500 mm. Vale olhar antes de terminar o nosso — nem que seja pra comparar soluções de encaixe.
