# MR 780 mod

As mesmas melhorias do SP103 (repositório `sp103-display-proxy`), agora no receiver híbrido **Magnat MR 780**:
um microcontrolador (provavelmente outro Raspberry Pi Pico 2 W) dentro do amp, que lê o que o amp mostra no display,
publica o estado na rede e comanda o amp pelo Wi-Fi.

O nome não é mais "display proxy" porque o projeto faz bem mais do que trocar textos no display: estado, comandos,
VU meter e telas de informação.

## Situação (2026-11-07)

Início. Primeiras fotos feitas (`docs/img/`): o display é um **VFD de matriz de pontos, 2 linhas × 16 caracteres**,
com o controlador dentro do vidro, ligado à placa principal por um cabo flat (`docs/pesquisa-display.md`). Nada
medido ainda. Próximo passo: fotos de perto do cabo flat e dos chips, depois medir e capturar (`docs/plano.md`).

## O que vem do SP103

| No SP103 | No MR 780 | Observação |
|---|---|---|
| Nomes próprios nas entradas ("TUNER" vira "Rega Planar 2") | Sim | O MR 780 tem 15 fontes: PHONO, CD, DVD, AUX 1/2, TAPE, LINE IN, FM, DAB, OPT 1/2, COAX 1/2, USB, BT |
| Estado do amp na rede (`/state`, `/device`) | Sim | Mesmo contrato da app Amp Link (repositório `amp-link`) |
| Comandar pelo Wi-Fi: códigos do controle remoto direto na linha do IR | Sim | Falta descobrir o protocolo do controle remoto |
| Ligar o amp a partir do standby | Sim | No MR 780 o amp aquece as válvulas por ~30 s ao ligar |
| VU meter no display enquanto toca | Provável | 2×16 como no SP103; depende de o controlador do VFD aceitar caracteres desenhados |
| Telas de informação (agulha, qualidade do Bluetooth) | Sim | Mesmo tamanho de tela do SP103 |
| Ajustar grave, agudo e balanço pelo Wi-Fi | Não | No MR 780 são botões giratórios |
| Alimentação pelo próprio amp, também em standby | A medir | Procurar um 5 V ou 3,3 V que fique ligado em standby |

Tudo isto está detalhado em `docs/plano.md`.

## Documentos

| Documento | Conteúdo |
|---|---|
| [`docs/plano.md`](docs/plano.md) | O plano, passo a passo, e as perguntas que as primeiras medições precisam responder |
| [`docs/pesquisa-display.md`](docs/pesquisa-display.md) | O que se sabe do MR 780 pela web e pelo manual: display, botões, controle remoto, atualização |

O manual oficial fica só na cópia local, em `docs/manual/` (tem direitos autorais, não vai para o GitHub).

## Pastas

- `firmware/`: o programa do Pico (vazio por enquanto).
- `tools/`: testes no computador e ferramentas.
- `captures/`: capturas do analisador lógico.
- `docs/img/`: fotos da placa.
