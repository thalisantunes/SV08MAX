# DEMON-SV08MAX — o que muda, arquivo por arquivo

Origem: https://github.com/3DPrintDemon/DEMON-SV08MAX
Instruções completas em `SV08_MAX_EXTRA_INSTRUCTIONS.md` (cópia local nesta pasta).

Os arquivos originais da nossa impressora estão em `home/`. Os diffs completos
estão em `DEMON/diffs/*.diff`. Nada foi aplicado — isto é só análise.

## Resumo dos diffs

| Arquivo | Destino na impressora | Δ | O que muda |
|---|---|---|---|
| `buffer_stepper.py` | `~/klipper/klippy/extras/` | +6 / −5 | Em vez de dar `PAUSE` direto quando detecta enrolamento, chama a macro `_JAM_DETECTION` do Demon. O comportamento antigo fica comentado. |
| `filament_switch_sensor.py` | `~/klipper/klippy/extras/` | +8 / −3 | Move a lógica pra depois do debounce e **adiciona o caminho de filamento presente**: acende o LED verde, zera `stop_feeding`, chama `_AUTO_FEED_MESSAGE` e `FEED_LOOP`. É o coração da melhoria do alimentador. |
| `z_offset_calibration.py` | `~/klipper/klippy/extras/` | +7 / −3 | `z_hop` padrão 2 → 5 mm; tolerância de reprova 0,0125 → 0,02 mm; e **randomiza ±50 mm o ponto de sonda** (`generator()`) pra não gastar sempre o mesmo ponto da chapa. |
| `plr.sh` | `~/` | +6 / −2 | Lê as variáveis de `/home/sovol/demon_vars.cfg` em vez de `saved_variables.cfg`, e adiciona mensagens de status ao retomar. |
| `buffer_stepper.cfg` | `~/printer_data/config/` | +158 / −42 | Reescrita quase completa da config do alimentador auxiliar. |
| `chamber_hot.cfg` | `~/printer_data/config/` | +56 / −34 | Reescrita do controle do aquecedor de câmara. |
| `plr.cfg` | `~/printer_data/config/` | +47 / −19 | Reescrita do power loss recovery. |
| `NO_BUFFER/filament_switch_sensor.py` | alternativa | — | Variante pra rodar **sem** o buffer/alimentador da Sovol. |

## :warning: Dependências — não dá pra aplicar solto

Os arquivos assumem que o **Demon Klipper Essentials Unified** já está instalado.
Eles chamam coisas que não existem no firmware de fábrica:

- macros `_JAM_DETECTION`, `_AUTO_FEED_MESSAGE`, `FEED_LOOP`
- pinos de LED renomeados: `green_led` → `LED_Green`, `blue_led` → `LED_Blue`
- arquivo de variáveis `/home/sovol/demon_vars.cfg`

Aplicar só estes 7 arquivos por cima do estoque **quebra** o alimentador e o PLR.

## Ordem de instalação (segundo o autor)

1. General Setup For All Printers (Demon Klipper Essentials Unified) — obrigatório antes de tudo.
2. Atualizar o KIAUH e reinstalar a extensão de shell command.
3. `SV08 MAX CUSTOM EXPANSION FILE` — obrigatório.
4. Substituir estes arquivos por SSH (o script do autor faz backup com sufixo `.1`).
5. Editar o `printer.cfg` (includes, `[idle_timeout]`, `[save_variables]`, `[homing_override]`).
6. Modificar fisicamente a tira metálica de limpeza do bico ("Modify your Metal Cleaner Strip").

Opcionais que o guia cobre: consertar/atualizar a imagem eMMC da Sovol,
modificar o aquecedor de câmara e migrar pro Klipper mainline.
