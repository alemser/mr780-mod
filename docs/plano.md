# Plano

O caminho é o mesmo do SP103: primeiro olhar e medir, depois capturar, só então soldar.

## Segurança antes de abrir

- O MR 780 é ligado direto na tomada (230 V) e tem válvulas: **dentro há tensões altas**, também alguns minutos depois
  de desligar (capacitores). Sempre: chave traseira em OFF, cabo fora da tomada, esperar uns 5 minutos.
- Não encostar na placa da fonte nem na parte das válvulas. A placa frontal (display e botões) é baixa tensão.
- Abrir o amp tira a garantia (dito em fórum).

## Passo 1: fotos

**Primeira rodada feita em 2026-11-07** (`docs/img/`, resumo em `pesquisa-display.md`): o display é um VFD de matriz
de pontos 2×16 com o controlador dentro do vidro, ligado à placa principal por um cabo flat.

**Falta, de perto e com luz:**

- As pontas do vidro do VFD (esquerda e direita): costuma haver um número de peça pequeno impresso no vidro.
- O conector do cabo flat **dos dois lados** (placa do display e CN5 na placa principal), para ler o que está escrito
  ao lado dos pinos e contar os fios.
- O texto do microcontrolador principal (64 pernas, perto do cabo flat) e do chip U15 (20 pernas, ao lado).
- A parte direita da placa do display, onde ficam o receptor do controle remoto e o botão DIRECT.
- O botão de volume por trás.

A lista original, para referência:

1. A placa frontal inteira, por trás (o lado do display).
2. O display: o vidro, a borda e o que estiver escrito nele (números de peça, "Noritake", "Futaba", etc.).
3. Todo chip perto do display: o texto de cima de cada um (com lupa ou zoom).
4. O cabo ou conector que liga a placa frontal à placa principal: quantos fios, e o que está escrito ao lado de cada
   pino na placa (VCC, GND, DIN, CLK, STB, RS, E...).
5. O microcontrolador principal (o maior chip com muitas pernas).
6. O receptor do controle remoto (componente de 3 pernas atrás de uma janelinha).
7. O botão de volume por trás: se é um encoder (gira sem fim, com cliques) ou um potenciômetro.

Coloque as fotos em `docs/img/`. Com elas eu identifico o tipo de display (`pesquisa-display.md`, a tabela dos três
casos).

## Passo 2: medições com o multímetro (amp ligado em standby e ligado)

- As tensões no conector da placa frontal, pino por pino, em standby e ligado.
- Se existe um 5 V ou 3,3 V que fica ligado em standby (para alimentar o Pico, como no SP103).
- A tensão de saída do receptor do controle remoto (para saber se o Pico pode puxar a linha para 0, como no SP103).

## Passo 3: capturas com o analisador lógico

- Os fios de dados do display, em várias telas: ligar (aquecimento), trocar de entrada, volume, DIM, INFO no DAB.
- A linha do receptor do controle remoto, apertando cada botão do controle: dá o protocolo e os códigos.

## Passo 4: decidir a montagem

Depende do Passo 1:

- **VFD serial (o caso provável)**: o Pico escuta os 3 ou 4 fios (relógio, dados, seleção). Para trocar textos, o
  Pico fica no meio desses fios: o microcontrolador fala com o Pico, e o Pico fala com o VFD. Corta-se só essas
  trilhas (ou se usa um adaptador no cabo flat), sem dessoldar o vidro. Fica mais simples que no SP103: numa linha
  serial não existe a pergunta "ocupado?", então não precisa do `busy.py`.
- Se for paralelo (improvável pelas fotos): o mesmo esquema do SP103.

## Passo 5: o programa

Aproveitar do `sp103-display-proxy` o que não depende do display:

- Wi-Fi, atualização pelo Wi-Fi com volta automática (`ota.py`, `tools/enviar.py`).
- Servidor de estado e o contrato da app Amp Link (`/state`, `/device`, `/names`, `/control`).
- Injeção de IR (`ir.py`), com os códigos do controle do MR 780.
- Ponte do VU (`relay/`), se o display permitir.

Não copiar agora: copiar quando cada parte for usada, para não carregar código morto.

## O que a app Amp Link precisa ganhar

O `GET /device` já descreve o aparelho. O MR 780 vai ser um segundo aparelho na mesma app, com a sua lista de
entradas e sem os ajustes de grave e agudo pelo Wi-Fi (são botões giratórios no MR 780).
