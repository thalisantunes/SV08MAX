# O "marreco" e a questão do multicolor

## Conclusão que derruba a compra pretendida

O **BigTreeTech Panda Bamboo Feeder não é trocador de cores.** É um *buffer /
alimentador auto-refill motorizado* — **a mesma categoria de coisa que o
alimentador auxiliar da Sovol**. O FAQ oficial do produto diz textualmente
*"The Panda Bamboo Feeder does not support multi-color printing"*
([West3D](https://west3d.com/products/panda-bamboo-feeder-esp32-smart-filament-loader-by-bigtreetech)).

O "auto-refill" dele é **sequencial**: acabou uma bobina, emenda na próxima
(2 por unidade, até 4 encadeando 3 unidades). Não é troca comandada por G-code.
O "Filament Hub" que vem no pack é peça **passiva** — junta as saídas numa linha
PTFE só. Não tem motor, não seleciona cor.

Comprar o Starter Pack por R$ 455–575 daria **um segundo marreco**, provavelmente
mais silencioso. Não daria multicolor.

> Cuidado com o nome: a BTT pendurou "Panda" em ~15 produtos sem relação
> (Panda Touch = tela; Panda Sense = sensor; Panda Hub = splitter USB;
> Panda Branch = placa de energia; Panda Bifrost = suporte passivo de bobina).
> Quase toda a linha é acessório para impressora **Bambu Lab**.

## O barulho — achado no nosso próprio config

`home/sovol/printer_data/config/buffer_stepper.cfg`, como veio de fábrica:

```ini
[buffer_stepper filament_buffer]
velocity: 150
accel: 5000
push_length: 25

[tmc2209 buffer_stepper filament_buffer]
uart_pin: buffer_mcu: PA12
interpolate: True
run_current: 0.9
hold_current: 0.4
sense_resistor: 0.150
uart_address: 3
```

**O bloco `[tmc2209]` do buffer não tem `stealthchop_threshold`.** Sem esse
parâmetro o TMC2209 roda em **spreadCycle**, que é o modo audível.

E não é descuido de projeto da placa — é descuido **só nesse motor**. No mesmo
`printer.cfg`, os quatro motores Z estão em `stealthchop_threshold: 999999`
(linhas 217, 233, 249, 265), ou seja, stealthchop sempre ligado. A Sovol
silenciou os Z e esqueceu o alimentador.

Some a isso `accel: 5000` — aceleração absurda para um motor de alimentação.

### Patch proposto (NÃO aplicado)

```ini
[buffer_stepper filament_buffer]
velocity: 50          # era 150
accel: 1000           # era 5000
push_length: 21       # era 25 — o braço bate na carcaça no fim do curso

[tmc2209 buffer_stepper filament_buffer]
stealthchop_threshold: 999999   # ACRESCENTAR — faltava
run_current: 0.6                # era 0.9
```

`velocity`, `accel` e `push_length` vêm de relatos do fórum Sovol
([#8785](https://forum.sovol3d.com/t/does-anyone-elses-aux-feeder-sound-like-crap/8785)),
onde a mesma metáfora aparece: *"sounds like a duck quacking every time it feeds"*.
**O `stealthchop_threshold` é dedução nossa a partir do config** — é o que eu
testaria primeiro, e é grátis.

Outro relato do fórum: trocar a graxa das engrenagens internas por lubrificante
de cera elimina o ruído de engrenagem.

> ⚠ Esses arquivos no repo são **cópia**. Mudar aqui não muda a impressora —
> tem que editar via Mainsail/SSH na máquina.

## Se for multicolor de verdade

Nenhuma dessas é o Panda. Todas **substituem o alimentador auxiliar por design**,
porque um MMU precisa empurrar filamento pelo bowden longo sozinho:

| Sistema | Filamentos | Custo | Ponto forte | Ponto fraco |
|---|---|---|---|---|
| **BoxTurtle** (Armored Turtle) | 4 | ~US$ 300 | Mais maduro. Cada pista com motor próprio. TurtleNeck buffer **feito para bowden longo** | Ninguém fez na SV08 Max. Sem secador |
| **BTT ViViD** | 4, expansível | US$ 269–399 | **Secador integrado com print-while-drying** — forte para ASA. Feito para Klipper | Produto novo, comunidade pequena |
| **Co Print KCM** | 4 (20 com ECM) | ~US$ 300 | **Única com guia oficial para SV08** | Conferir se exige o ChromaHead deles |
| **Anycubic ACE Pro** + [driver](https://github.com/szkrisz/ACEPROSV08) | 4 | ~US$ 150–200 | Repo específico para SV08 | README declara *"work-in-progress driver"* |

Ecossistema: quase tudo roda sob [Happy Hare](https://github.com/moggieuk/Happy-Hare);
BoxTurtle tem stack própria (AFC-Klipper-Add-On).

**Ninguém documentou BoxTurtle ou ERCF numa SV08 Max.** A thread
[ERCF 2.0 no fórum Sovol](https://forum.sovol3d.com/t/ercf-2-0/5737) termina sem
build. Existe um [toolhead multicolor CN3D para SV08 Max](https://www.printables.com/model/1515435-sv08-max-sv-zero-multicolor-toolhead-upgrade-by-cn)
com duplo sensor e **cortador integrado** — que é o pré-requisito de hardware.

### Dois problemas específicos da nossa máquina

1. **Bowden de 1,3 m.** A config custom da comunidade usa
   `filament_length_ptfe: 1300.0`. Está no limite superior do que MMU encara.
   Cada troca = descarrega, corta, carrega 1,3 m, purga. Em peça com centenas de
   trocas isso domina o tempo de impressão.
2. **Bico de 1.0 mm briga com multicolor.** Purga escala com o volume da zona de
   fusão. Em 0.4 mm uma peça de 40 g já gera ~60 g de purga; em 1.0 mm, muito
   mais. Multicolor e bico grosso são objetivos opostos — escolher um.

E purga de ASA em câmara aquecida não esfria rápido como PLA: fica mole e
pegajoso, com risco de grudar no bico ou na peça, e a própria torre de purga
vira candidata a warping.

## Recomendação

**Separar os dois problemas.**

1. **O marreco:** aplicar o patch acima. Custo zero, reversível, e o
   `stealthchop_threshold` é o candidato mais provável.
2. **Multicolor:** decidir antes se convive com o bico de 1.0 mm. Se sim, o
   **ViViD** ganha do BoxTurtle no nosso caso pelo secador integrado (ASA), e o
   **Co Print KCM** ganha em documentação. Se a resposta for "quero os dois ao
   mesmo tempo", a conta não fecha e não vale investir agora.

Para o riser: **o alimentador vai sair da lateral de qualquer jeito** — seja pelo
patch + realocação, seja por MMU. Então a passagem de PTFE no assento superior
deve ser projetada **genérica**: um furo com raio de curvatura ≥ 50 mm e
furação-padrão, sem casar com um sistema específico.

---

# Análise do vídeo do ruído (29/08)

Arquivo: `Referências/Buffer/document_4943150175330764605.mp4` — 2,90 s, 480×852,
~29 fps reais (o container declara 120), com áudio 48 kHz.

## Áudio

| | |
|---|---|
| Envelope | **Contínuo e plano**, ~−29 dBFS de 0,22 s até o fim. Pico −26,4 |
| Transientes impulsivos | **Nenhum** |
| Frequência dominante | **273 Hz** (harmônica em 546) |
| Sub-picos | ~99 Hz e ~137 Hz |

## Vídeo

Diferença média entre quadros consecutivos: 0,65 a 5,5 (escala 0–255), suave e
sem eventos discretos — compatível com tremor de câmera na mão. **Nenhum curso
mecânico visível** nos 2,9 s.

## O que isso descarta

1. **Não é impacto mecânico.** Impacto do braço na carcaça apareceria como
   transiente afiado no envelope. O envelope é plano. → **o `push_length: 25→21`
   perde prioridade** (continua válido, mas não é a causa).
2. **Não é o curso de empurrar.** 25 mm a 150 mm/s = **0,167 s**. O tom dura
   2,7 s contínuos. Não bate.

## Hipótese principal

Motor **energizado e parado, zumbindo** — `hold_current: 0.4` mantém corrente, e
sem `stealthchop_threshold` o TMC2209 fica em **spreadCycle**, que chopa corrente
mesmo em standstill. Isso produz exatamente um tom contínuo de frequência fixa.

Reforça o achado do config: os 4 motores Z estão em `stealthchop_threshold: 999999`
e só o buffer ficou de fora.

⚠ **Ressalva honesta:** as peças móveis do alimentador são internas, então o vídeo
não prova ausência de movimento — prova ausência de movimento *visível*. E não dá
pra descartar 100% que o tom venha de outra fonte próxima (ventoinha).

## Teste que decide, antes de mexer no config

Com a impressora **ociosa**, no console do Mainsail:

1. `DUMP_TMC STEPPER="buffer_stepper filament_buffer"` — ler o GCONF e conferir
   o bit `en_spreadcycle`. Read-only, seguro.
2. Ouvir. Mandar **`M84`** (desliga todos os steppers) e ouvir de novo.
   - Ruído **para** → é motor segurando posição → **stealthchop é a correção**.
   - Ruído **continua** → não é o buffer; procurar ventoinha.

Só depois aplicar o patch.

---

# CONFIRMADO no driver ao vivo (29/08, 17:50)

`DUMP_TMC STEPPER=filament_buffer` na máquina rodando:

```
GCONF:      000001c4 en_spreadcycle=1 pdn_disable=1 mstep_reg_select=1 multistep_filt=1
IHOLD_IRUN: 00081509 ihold=9 irun=21 iholddelay=8
TPWMTHRS:   000fffff tpwmthrs=1048575
TPOWERDOWN: 00000014 tpowerdown=20
```

**`en_spreadcycle=1`** — o driver do buffer está em spreadCycle. Os quatro motores
Z, no mesmo `configfile.settings` lido do Klipper, têm `stealthchop_threshold: 999999`
(→ `en_spreadcycle=0`). O bloco do buffer **não tem a chave**.

> ⚠ `DUMP_TMC STEPPER="buffer_stepper filament_buffer"` é **rejeitado**.
> O nome que funciona é **`STEPPER=filament_buffer`**.

## Teste A/B feito ao vivo, sem reiniciar

```
SET_TMC_FIELD STEPPER=filament_buffer FIELD=en_spreadcycle VALUE=0
```

GCONF passou de `000001c4` → **`000001c0`**. StealthChop ativo.
Reverter: mesma linha com `VALUE=1`, ou qualquer `FIRMWARE_RESTART`
(a mudança **não é persistente**).

## Patch revisado

O `push_length: 25→21` **saiu** — a análise do vídeo mostrou envelope de áudio
plano, sem transiente, então não é impacto de braço na carcaça.

```ini
[tmc2209 buffer_stepper filament_buffer]
stealthchop_threshold: 999999   # ACRESCENTAR — é a correção principal
run_current: 0.6                # era 0.9

[buffer_stepper filament_buffer]
velocity: 50          # era 150
accel: 1000           # era 5000
```

`velocity`/`accel` continuam valendo: reduzem o ruído do curso em si e o impacto
no fim. Mas o tom contínuo de 273 Hz é o spreadCycle.

---

# RESULTADO MEDIDO — antes × depois (29/08)

Vídeos em `Referências/Buffer/`. O "depois" foi gravado com **apenas** o
`en_spreadcycle=0` ao vivo — `velocity`, `accel` e `run_current` ainda não valiam.

| | Antes (`...605.mp4`, 2,9 s) | Depois (`...610.mp4`, 22,5 s) |
|---|---|---|
| Piso de ruído (mediana RMS) | **−29,0 dBFS** | **−42,8 dBFS** |
| Caráter | tom **contínuo**, 100% do tempo | silêncio + **eventos curtos** isolados |
| Frequência dominante | **275 Hz** | 688 Hz |
| Banda 250–300 Hz | é o pico (0 dB) | **−18,7 dB** abaixo do pico |
| Duty cycle do ruído | ~100% | ~7% (≈1,5 s em 22,5 s) |

**−13,8 dB no piso** ≈ perceptualmente cerca de um quarto do volume. O zumbido
permanente de 275 Hz — assinatura do spreadCycle — **desapareceu**.

Eventos remanescentes em 0,9 · 2,2 · 5,5–5,8 · 8,7 · 11,2 · 12,1 · 13,5–13,7 ·
20,9 s, chegando a −29 dB. **São os pushes de filamento em si**, e é neles que
`velocity` e `accel` atuam — que só passaram a valer depois.

## Aplicado e persistido

Gravado em `buffer_stepper.cfg` via API do Moonraker, `FIRMWARE_RESTART` feito,
Klipper voltou **ready**. Confirmado no driver: `GCONF: 000001c0` (en_spreadcycle
ausente = 0) e `TPWMTHRS: 00000000` — StealthChop sempre, igual aos motores Z.

```ini
[buffer_stepper filament_buffer]
velocity: 50          # era 150
accel: 1000           # era 5000
push_length: 25       # inalterado — o vídeo descartou impacto mecânico

[tmc2209 buffer_stepper filament_buffer]
interpolate: True
stealthchop_threshold: 999999   # ACRESCENTADO
run_current: 0.6                # era 0.9
```

**Backup:** o arquivo original está em `config/buffer_stepper.cfg.ORIGINAL.bak`
na própria impressora (6168 bytes). Não é incluído pelo `printer.cfg`, então é
inerte — para reverter, basta renomear por cima.

## Próximo passo

Gravar um terceiro vídeo agora, com velocity/accel valendo, para medir quanto
caiu o ruído dos pushes.

---

# Terceiro vídeo — com velocity/accel valendo (29/08, 21:33)

`...613.mp4`, 28,1 s. **Tem voz/risada de fundo** — separei por planicidade
espectral (voz é banda larga, planicidade > 0,6; o motor é tonal, < 0,55).
Os vídeos 2 e 3 têm voz; o piso foi medido por **percentil 10**, que ignora
transiente.

## Progressão do piso e da banda do spreadCycle

| | Antes | Só stealthchop | Completo |
|---|---|---|---|
| Piso (p10) | −30,6 | −43,9 | **−46,1 dBFS** |
| Banda **250–300 Hz** (janelas quietas) | **+23,5** | −7,2 | **−15,7 dB** |

O tom de 275 Hz caiu **39 dB no total**. Essa é a parte sólida do resultado.

## Eventos de push — o trade-off

Detectados acima do piso + 8 dB, classificados por planicidade:

| | Vídeo 2 (vel 150 / acc 5000) | Vídeo 3 (vel 50 / acc 1000) |
|---|---|---|
| Eventos de máquina | 3 | 3 |
| **Duração média** | **0,22 s** | **1,35 s** |
| **Pico médio** | **−27,9 dB** | **−30,6 dB** |

> **Cada push ficou 2,7 dB mais baixo, mas ~6× mais longo.**

Era previsível: 25 mm a 50 mm/s leva **0,5 s** contra 0,167 s a 150 mm/s, e a
rampa mais lenta soma. Em energia acústica integrada o push até subiu — mas
ruído **tonal** incomoda muito mais que banda larga no mesmo nível, e o drone
contínuo é que sumiu. O Thalis reportou percepção de **ruído mais baixo**.

## Ajuste fino disponível

Se durante uma impressão real o push arrastado incomodar, o botão é o `velocity`:

| velocity | duração do push de 25 mm |
|---|---|
| 150 (original) | 0,167 s |
| **100** | 0,25 s |
| 50 (atual) | 0,50 s |

`velocity: 100` corta a duração pela metade e mantém quase todo o ganho, já que
o grosso veio do stealthchop, não da velocidade.

⚠ Ressalva: distância do microfone e nível de gravação variam entre os vídeos,
então **níveis absolutos não são comparáveis entre arquivos**. Duração e a
relação banda/piso dentro do mesmo arquivo são.
