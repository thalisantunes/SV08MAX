# SV08 MAX — o que tem neste repositório

Fork de `Sovol3d/SV08MAX` em `thalisantunes/SV08MAX`. O upstream não tem
documentação nenhuma (README = uma linha), então este arquivo mapeia o conteúdo.

## Árvore

| Pasta | Tamanho | Conteúdo |
|---|---|---|
| `STEP/` | 41 MB (zip) | **Um único** `.STEP` de 175 MB com a máquina inteira montada (`MP4802-0000 500尺寸3D打印机 V8`). Descompactado em `_work/` (fora do git). |
| `PDF/` | 138 MB | 136 desenhos técnicos 2D, um por peça, cotados e com material. Nomes em chinês, prefixo `JXHMP4802-<grupo><item>-<rev>`. |
| `Motherboard/` | 740 KB | `Mcu_Pin_definition.pdf` e `Extra_Pin_definition.pdf` — pinagem da placa. |
| `home/` | 452 MB | Cópia do rootfs do usuário `sovol` da impressora: Klipper, Moonraker, KlipperScreen, mainsail, crowsnest, moonraker-obico, timelapse, kiauh e o firmware proprietário `zhongchuang`. |
| `Referências/` | — | Material de pesquisa nosso (não vem do upstream). |
| `DOCS/` | — | Documentação nossa. |
| `_work/` | 175 MB | Área de trabalho (STEP descompactado). Não versionar. |

## Grupos de desenho no `PDF/`

| Prefixo | Grupo |
|---|---|
| `01xxx` | Base / estrutura inferior, caixa da fonte, chapas de proteção |
| `02xxx` | Mesa aquecida (placa de alumínio, PEI, chapa magnética), perfis do eixo Z |
| `03xxx` | Perfis frontal/traseiro/lateral, cable chain, suportes de fiação |
| `05xxx` | **Perfis do topo (`顶部型材1..4`) e guias** — é aqui que mexe o riser |
| `06xxx` | Duto de ar / chapas do sistema de refrigeração |
| `08xxx` | Chicotes elétricos (motores, fim de curso, LED, câmera, mesa) |
| `09xxx` | Embalagem, espumas, etiquetas, manual do usuário (EN) |
| `10xxx` | Suporte de filamento |
| `11xxx` | Alimentador auxiliar (engrenagens, tubos, sensor de filamento) |
| `12xxx` | Carcaça da tela |

`PDF/JXHMP4802-09010-h User manual_SV08MAX_V1.3_EN.pdf` é o manual em inglês.

## `home/sovol/printer_data/config/`

`printer.cfg`, `Macro.cfg`, `buffer_stepper.cfg` (alimentador auxiliar),
`chamber_hot.cfg` (aquecedor de câmara), `plr.cfg` (power loss recovery),
`moonraker.conf`, `crowsnest.conf`.

## Fontes externas que estamos considerando

- **Top riser** — https://cults3d.com/en/3d-model/tool/sovol-sv08-max-top-riser-3d-printed-corners-laser-cut-acrylic-panels
- **DEMON-SV08MAX** — https://github.com/3DPrintDemon/DEMON-SV08MAX
  (substitui `buffer_stepper.cfg/.py`, `filament_switch_sensor.py`, `chamber_hot.cfg`,
  `plr.cfg/.sh`, `z_offset_calibration.py` para melhorar o alimentador automático;
  instruções em `Demon_Klipper_Essentials_Unified` →
  `Documentation/INSTALL_INSTRUCTIONS/SOVOL_SV08_MAX_SETUP/SV08_MAX_EXTRA_INSTRUCTIONS.md`)
