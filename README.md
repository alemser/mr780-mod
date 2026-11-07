# MR 780 mod

As mesmas melhorias do SP103 (repositório `sp103-display-proxy`), agora no receiver híbrido **Magnat MR 780**:
um microcontrolador (provavelmente outro Raspberry Pi Pico 2 W) dentro do amp, que lê o que o amp mostra no display,
publica o estado na rede e comanda o amp pelo Wi-Fi.

O nome não é mais "display proxy" porque o projeto faz bem mais do que trocar textos no display: estado, comandos,
VU meter e telas de informação.

## Situação (2026-11-07)

Início, na **fase 1: só ler o estado** (o que vai para o display, ligado ou standby), sem cortar nada. O display é um
**VFD de matriz de pontos, 2 linhas × 16 caracteres**, com o controlador dentro do vidro, ligado à placa principal
por um cabo flat de 21 vias e passo de 1,0 mm (`docs/pesquisa-display.md`). Em 2026-11-07, com o multímetro e o amp
fora da tomada, foram mapeados no flat os GND, todos os botões, o VOLUME, o SOURCE e o sinal do controle remoto
(`docs/medicoes.md`). Próximo passo: as tensões com o amp ligado, e depois o analisador lógico (`docs/plano.md`).

## O que vem do SP103

| No SP103 | No MR 780 | Observação |
|---|---|---|
| Nomes próprios nas entradas ("TUNER" vira "Rega Planar 2") | Sim | O MR 780 tem 15 fontes: PHONO, CD, DVD, AUX 1/2, TAPE, LINE IN, FM, DAB, OPT 1/2, COAX 1/2, USB, BT |
| Estado do amp na rede (`/state`, `/device`) | Sim | Mesmo contrato da app Amp Link (repositório `amp-link`) |
| Comandar pelo Wi-Fi: códigos do controle remoto direto na linha do IR | Sim | Por fio, sem Broadlink. Mais os botões do painel (DIRECT, DIM), que o controle remoto não tem |
| Ligar o amp a partir do standby | Sim | No MR 780 o amp aquece as válvulas por ~30 s ao ligar |
| VU meter no display enquanto toca | Provável | 2×16 como no SP103; depende de o controlador do VFD aceitar caracteres desenhados |
| Telas de informação (agulha, qualidade do Bluetooth) | Sim | Mesmo tamanho de tela do SP103 |
| Ajustar grave e agudo pelo Wi-Fi | Difícil | Botões giratórios, provavelmente analógicos. O DIRECT (liga e desliga o tom) é viável |
| Alimentação pelo próprio amp, também em standby | A medir | O Pico pelo `D5V`. Novo: um Pi Zero 2 WH dentro, por um conversor DC-DC na linha `VCC` (sem transformador) |

Tudo isto está detalhado em `docs/plano.md`.

## Documentos

| Documento | Conteúdo |
|---|---|
| [`docs/plano.md`](docs/plano.md) | O plano, passo a passo, e as perguntas que as primeiras medições precisam responder |
| [`docs/pesquisa-display.md`](docs/pesquisa-display.md) | O que se sabe do MR 780 pela web, pelo manual e pelas fotos: display, placas, botões, controle remoto |
| [`docs/medicoes.md`](docs/medicoes.md) | Todas as leituras do multímetro, com a tabela das 21 vias do cabo flat |
| [`docs/historico/brief-2026-10.md`](docs/historico/brief-2026-10.md) | O brief de 2026-10-28 a 30: primeiras fotos, medidas do cabo flat e as decisões das fases. Não descreve o estado atual |

O manual oficial fica só na cópia local, em `docs/manual/` (tem direitos autorais, não vai para o GitHub).

## Pastas

- `firmware/`: o programa do Pico (vazio por enquanto).
- `tools/`: testes no computador e ferramentas.
- `captures/`: capturas do analisador lógico.
- `docs/img/`: fotos da placa.
