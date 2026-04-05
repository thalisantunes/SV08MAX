# GUIA DE BOAS PRÁTICAS — PRIMEIRO USO SOVOL SV08 MAX

> Documento consolidado de boas práticas, armadilhas conhecidas e soluções para o primeiro uso da **Sovol SV08 MAX**, compilado a partir do repositório oficial [Sovol3d/SV08MAX](https://github.com/Sovol3d/SV08MAX), do [fórum oficial Sovol](https://forum.sovol3d.com), da [Wiki Sovol](https://wiki.sovol3d.com) e de relatos da comunidade (Reddit, Discord, Printables).

---

## Sumário

1. [Antes de Ligar (Inspeção Física)](#1-antes-de-ligar-inspeção-física)
2. [Primeiro Boot](#2-primeiro-boot)
3. [Tradução para Português](#3-tradução-para-português)
4. [Configurações Críticas do Klipper (`printer.cfg`)](#4-configurações-críticas-do-klipper-printercfg)
5. [Problemas Conhecidos e Soluções](#5-problemas-conhecidos-e-soluções)
6. [Temperaturas Recomendadas](#6-temperaturas-recomendadas)
7. [Manutenção](#7-manutenção)
8. [Upgrades Recomendados pela Comunidade](#8-upgrades-recomendados-pela-comunidade)
9. [Links Úteis](#9-links-úteis)

---

## 1. ANTES DE LIGAR (Inspeção Física)

A SV08 MAX chega quase pronta, mas **não ligue a impressora sem fazer uma inspeção completa**. Vários dos problemas mais recorrentes relatados no fórum são defeitos de fábrica ou peças que afrouxaram no transporte.

- [ ] **Verificar parafusos do *carrier* do extrusor.** Problema recorrente: *carrier* solto causa *layer shifts* (deslocamento de camadas). Reaperte antes do primeiro *homing*.
- [ ] **Inspecionar cabos USB do *toolhead* sob a cola de fábrica.** Há vários relatos de cabos danificados ou com fios verdes visíveis escondidos sob a cola quente. Retire a cola com cuidado e inspecione; substitua o cabo se houver qualquer dano.
- [ ] **Verificar tensão das correias A/B.** Imprima o *medidor de tensão de correia* do Printables (modelo **#115460**) assim que tiver a primeira impressão funcionando, e ajuste ambas as correias para que tenham a **mesma** frequência/tensão.
- [ ] **Confirmar que os *fans* da *motherboard* e do *bed controller* estão funcionando.** Unidades com ventoinhas defeituosas de fábrica já foram reportadas. Abra a tampa inferior e confirme visualmente que ambos giram.
- [ ] **Verificar o conector de energia e o fusível.** Há relatos de falhas no fusível e mau contato no conector IEC de entrada. Confirme se o seletor de tensão (se houver) está correto para sua rede (110 V / 220 V).
- [ ] **Reapertar todos os parafusos estruturais do *frame*.** Qualquer oscilação na estrutura compromete a qualidade de impressão em altas velocidades.
- [ ] **Inspecionar a mesa** (chapa PEI e placa magnética) em busca de amassados ou sujeira de fábrica.

---

## 2. PRIMEIRO BOOT

- ⚠️ **ATENÇÃO NO PRIMEIRO *HOMING*.** Há relatos de colisão do cabeçote com o *gantry* durante a primeira calibração. **Fique com a mão sobre o botão de emergência / interruptor geral** durante o primeiro *home* e observe atentamente.
- A SV08 MAX **usa drivers diferentes do SV08** padrão. O `homing_override` no `printer.cfg` pode precisar de ajuste se você importar configurações do SV08. **Não copie o `printer.cfg` do SV08 cegamente.**
- O sistema operacional embarcado roda **Debian Bullseye** (EOL / desatualizado). **Não execute `apt upgrade` sem cuidado**, pois isso pode quebrar pacotes pinados pela Sovol e inutilizar a impressora. Prefira instalar pacotes específicos quando necessário.
- O **script de produção da Sovol** (herdado da linha de testes) causa **reset do Wi-Fi a cada ~30 segundos**, perdendo o IP. Esse script geralmente precisa ser **localizado e comentado** em `/etc/rc.local` ou em um *service* do systemd (procure por referências a `ttyS3` ou ao envio de IP para serial).
- **Credenciais SSH padrão:**
  - Usuário: `sovol`
  - Senha: `sovol`
  - **Troque a senha imediatamente** com `passwd` após o primeiro login, especialmente se a impressora estiver em rede não confiável.
- Execute a sequência recomendada de calibração:
  1. *Homing* dos eixos
  2. *Bed leveling* (Eddy Current Sensor, ~80 s)
  3. PID do *hotend* e do *bed*
  4. Calibração do *extruder* (E-steps / *rotation distance*)
  5. Z-offset fino com folha A4

---

## 3. TRADUÇÃO PARA PORTUGUÊS

Boa notícia: o **KlipperScreen já possui tradução pt_BR nativa**, não é necessário instalar nada.

- **Localização dos arquivos na impressora:**
  ```
  ~/KlipperScreen/ks_includes/locales/pt_BR/LC_MESSAGES/
  ```
- **Arquivos presentes:**
  - `KlipperScreen.po` — arquivo-fonte de tradução editável
  - `KlipperScreen.mo` — binário compilado usado pelo KlipperScreen
- **Como ativar na tela (KlipperScreen):**
  1. Abrir **Settings** (Configurações)
  2. Selecionar **Language** (Idioma)
  3. Escolher **Português (Brasil)**
  4. O KlipperScreen recarrega automaticamente
- **Nota:** O **Mainsail** (interface web) também suporta português. Entre em *Settings → Language* e selecione Português.
- Se encontrar erros de tradução, você pode contribuir via Weblate (link disponível no README oficial do [KlipperScreen](https://github.com/KlipperScreen/KlipperScreen)).

---

## 4. CONFIGURAÇÕES CRÍTICAS DO KLIPPER (`printer.cfg`)

> ⚠️ **SEMPRE faça backup do `printer.cfg` original** antes de qualquer alteração:
> ```bash
> cp ~/printer_data/config/printer.cfg ~/printer_data/config/printer.cfg.original
> ```

### 4.1. Feeder auxiliar (sensor de entupimento)

- Ajustar a **velocidade do *feeder* auxiliar para ~2/3 do padrão**. O design simplista gera falsos positivos de entupimento em velocidades nominais.
- Considerar **desativar a macro `NOZZLE_CLOG_CHECK`** ou ajustar o parâmetro `delta_extrusion` para um valor **maior que 50** (tornando a detecção menos sensível).
- **Bypass temporário durante troca manual de filamento** ou testes:
  ```
  SET_GCODE_VARIABLE MACRO=variables VARIABLE=is_manual_feeding VALUE=True
  ```

### 4.2. PLR (Power Loss Recovery)

- O script `plr.sh` grava as coordenadas da impressão **a cada movimento**, o que sobrecarrega o MCU e causa erro:
  ```
  MCU shutdown: Move queue overflow
  ```
- **Solução:** editar o *helper* Python (em `~/pyhelper/` ou referenciado pelo `plr.sh`) para **remover a escrita contínua**, mantendo apenas *snapshots* periódicos (ex.: a cada N camadas) ou desativando totalmente o PLR se não for usar.

### 4.3. Z-Offset

- A impressora possui **dois Z-offsets separados** (um no salvamento interno, outro aplicado em runtime), o que causa confusão.
- **Abordagem mais confiável:** adicionar o valor de Z-offset diretamente no macro `[gcode_macro START_PRINT]` (ou `Start_print`), por exemplo com `SET_GCODE_OFFSET Z=<valor>` após o *homing* e *bed mesh*.
- **Seção `z_offset_calibration`** no `printer.cfg`:
  ```
  [z_offset_calibration]
  endstop_xy_position: 250,251
  ```
  Essa coordenada corresponde ao ponto físico sobre o sensor de referência; **não altere** sem medir.

---

## 5. PROBLEMAS CONHECIDOS E SOLUÇÕES

| # | Problema | Solução |
|---|----------|---------|
| 1 | **Falso positivo "Nozzle Clog Detected"** ao usar Exclude Objects | Bug na macro `CHECK_NOZZLE_CLOG`. Desativar a verificação ou aumentar `delta_extrusion > 50`. |
| 2 | **Feeder auxiliar com falsos positivos de entupimento** | Design simplista: reduzir velocidade para ~2/3 do padrão. |
| 3 | **Ruído excessivo dos fans** da *motherboard* e *bed controller* | Substituir por **Noctua** 40 mm / 60 mm (atenção à tensão; usar conversor buck se necessário). |
| 4 | **Layer shifts** (camadas deslocadas) | Verificar parafusos do *carrier* do extrusor, tensão/igualdade das correias A/B e fixação do *frame*. |
| 5 | **Z-Banding / Z-Wobble** | Problema mecânico: verificar correias, rolamentos lineares, folgas em *leadscrews*/polias e planicidade da mesa. |
| 6 | **Wi-Fi da *mainboard* reseta o IP periodicamente** | Comentar o script de produção da Sovol (causa reset a cada ~30 s). |
| 7 | **Bed heater *overtemp* a 110 °C** | Mesa aquecida com **tensão de linha (AC)** — limite prático com *enclosure* é **~90 °C**; chave térmica de segurança abre em 130 °C. |
| 8 | **Hotend limitado a 300 °C** | *Upgrade* possível com o hotend **Creality K2 Plus Unicorn** (requer furação, rosqueamento e solda — ver seção 8). |
| 9 | **Câmera não aparece no *dashboard*** | Verificar status do serviço **Crowsnest**: `sudo systemctl status crowsnest`; conferir `crowsnest.conf`. |
| 10 | **Obico *self-hosted* não instala** | Debian Bullseye EOL: editar `/etc/apt/sources.list` trocando `deb.debian.org` por `archive.debian.org` e comentar `security.debian.org`. |
| 11 | **Firmware *update* trava na tela de Wi-Fi** | **Sempre manter um backup do eMMC** antes de atualizar; a recuperação via imagem é a saída segura. |

---

## 6. TEMPERATURAS RECOMENDADAS

> ⚠️ O *hotend* da SV08 MAX tende a **rodar 5–10 °C mais frio** do que o valor indicado no *firmware*. Ajuste para cima em relação às temperaturas usuais.

| Material | Nozzle | Bed | Observações |
|----------|--------|-----|-------------|
| **PLA** | **225–230 °C** | 55–60 °C | Mais alto que o usual devido ao desvio do *hotend*. |
| **PETG (rápido)** | **265 °C+** | 75–85 °C | Necessário para velocidades altas; aumentar *retraction* se houver *stringing*. |
| **ABS/ASA** | 245–260 °C | 100 °C | Usar *enclosure* fechado. |
| **PC / PC-blend** | 270–290 °C | 105–110 °C | *Enclosure* obrigatório. |
| **PET-CF / PET-GF** | **até 320 °C** | 90 °C | O nozzle de fábrica é **aço temperado** e aguenta fibras abrasivas em curto prazo; substituir por nozzle endurecido para uso contínuo. |

**Bed (mesa aquecida):**
- Aquecida via **tensão AC de linha** — muito rápida.
- **Limite prático com *enclosure*: 90 °C.**
- *Overtemp switch* (chave térmica) abre em **130 °C**.

---

## 7. MANUTENÇÃO

- **Lubrificar os trilhos lineares regularmente.** A impressora vem com **duas graxas** no kit:
  - **Graxa grande, transparente, metálica** — para trilhos lineares (LM guides).
  - **Graxa pequena, branca** — para *leadscrews* / fusos Z.
- **Verificar o pino do extrusor.** Problema **compartilhado com o SV08 e o SV08 Zero**: o pino pode se mover e soltar. Existe um *fix* impresso comunitário no Printables — modelo **#1393015**.
- **Verificar a tensão das correias periodicamente.** Use o medidor impresso (Printables **#115460**) e garanta que A e B estejam iguais.
- **Imãs de ferrite nos cabos USB** (cabeça de impressão e câmera) para aterramento / redução de EMI. Ajuda a reduzir falhas de comunicação USB do *toolhead*.
- Limpar o *hotend* e o bico antes de cada impressão grande.
- Armazenar o filamento em local seco (secadora ou caixa com sílica).

---

## 8. UPGRADES RECOMENDADOS PELA COMUNIDADE

- **Ventoinhas Noctua** para a *motherboard* e o *bed controller* — reduz drasticamente o ruído (principal reclamação da máquina).
- **Adaptador de duto de exaustão imprimível** — Printables **#1491911** (permite acoplar duto para exaustão dos voláteis do *enclosure*).
- **Top Hat / Riser imprimível** — aumenta a altura do *enclosure* para peças maiores que o volume nativo.
- **eMMC 32 GB** — é possível usar um módulo **genérico** com adaptador USB para fazer *backup* e *clone* da imagem original antes de mexer no sistema.
- **Hotend Creality K2 Plus Unicorn** — permite romper o teto dos 300 °C. **Requer:**
  - Furação do bloco / suporte
  - Rosqueamento
  - Soldagem do cartucho e do termistor
  - Ajustes no `printer.cfg` (PID, sensor type, max temp)
- **Extruder pin fix** — Printables **#1393015** (mesmo fix do SV08/Zero).
- **Medidor de tensão de correias** — Printables **#115460** (essencial desde o dia 1).

---

## 9. LINKS ÚTEIS

- **GitHub oficial (firmware/configs/CAD):** <https://github.com/Sovol3d/SV08MAX>
- **Fórum oficial Sovol (buscar por "SV08 MAX"):** <https://forum.sovol3d.com>
- **Wiki Sovol:** <https://wiki.sovol3d.com>
- **Ellis' Print Tuning Guide** (referência absoluta para calibração Klipper): <https://ellis3dp.com/Print-Tuning-Guide/>
- **KlipperScreen Traduções (Weblate):** ver link no README do projeto [KlipperScreen](https://github.com/KlipperScreen/KlipperScreen)

### Fontes consultadas para este guia

- [Step-by-Step Installation and Setup Guide for the Sovol SV08 MAX — Sovol](https://www.sovol3d.com/blogs/news/sovol-sv08-max-step-by-step-installation-setup-guide)
- [Sovol SV08 Max pro tips to maximize print quality — Sovol](https://www.sovol3d.com/blogs/news/sovol-sv08-max-print-quality-pro-tips-guide)
- [Boosting print speed and quality with Sovol SV08 Max — Sovol EU](https://sovol.eu/blogs/new/boost-print-speed-quality-sovol-sv08-max-advanced-tips)
- [Essential Tips for Perfect Prints with Sovol SV08 Max — Sovol](https://www.sovol3d.com/blogs/news/essential-tips-perfect-prints-sovol-sv08-max-3d-printing)
- [Top Tips for Getting the Best Prints with Sovol SV08 Max — Sovol](https://www.sovol3d.com/blogs/news/sovol-sv08-max-best-print-tips-setup-settings-maintenance)
- [SV08 Max | Sovol 3D Printer Wiki](https://wiki.sovol3d.com/en/SV08Max/SV08MAX)
- [Unboxing, presentation and tests — Sovol3d forum](https://forum.sovol3d.com/t/unboxing-presentation-and-tests/7982)
- [Gantry calibration — Sovol3d forum](https://forum.sovol3d.com/t/gantry-calibration/9479)
- [Mastering High-Speed Printing with the Sovol SV08 Max — Sovol](https://www.sovol3d.com/blogs/news/high-speed-precision-sovol-sv08-max)
- [Essential tips for flawless prints with Sovol SV08 Max — Sovol UK](https://sovol.uk/blogs/news/essential-tips-flawless-prints-sovol-sv08-max-3d-printer)
- [Large 3D Printer Installation and First Print Success — Sovol](https://www.sovol3d.com/blogs/news/large-3d-printer-setup-installation-calibration-first-print)
- [Sovol SV08 MAX CoreXY 3D Printer User Manual](https://manuals.plus/asin/B0FK9NR69S)

---

*Documento mantido no repositório `thalisantunes/sv08max` na branch `claude/analyze-sovol-sv08max-KQQ8w`.*
