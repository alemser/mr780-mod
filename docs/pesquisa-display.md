# MR 780: o display e o que se sabe do amp (2026-11-07)

Resumo: a web não diz qual é o display do MR 780 (não existe manual de serviço público). **As fotos do amp aberto
responderam:** é um **VFD de matriz de pontos, 2 linhas × 16 caracteres**, sem nenhum chip na placa do display.

## O que as fotos mostram (2026-11-07)

Fotos em `docs/img/`.

### O display (`placa-display-frente-vfd.jpg`)

- Um tubo de vidro VFD (display fluorescente a vácuo) com **2 linhas de 16 caracteres**. Cada caractere é uma
  matriz de pontos (5×7 ou parecido), não segmentos. Não aparecem ícones fixos.
- O vidro está soldado direto na placa do display, por uma fileira de pernas na borda de baixo.
- **Não há nenhum chip na placa do display**, nem na frente nem atrás (`placa-display-tras-1.jpg`): só transistores,
  diodos, capacitores e um regulador (Q1). Isso é a fonte do VFD (filamento e a tensão alta das grades).
- Um VFD de 2×16 em matriz de pontos sem chip por fora precisaria de mais de 100 fios para ser comandado. Como o
  vidro tem bem menos pernas, **o controlador está dentro do próprio vidro** ("chip-in-glass", CIG). Esses VFDs
  recebem **comandos e códigos de caracteres por uma linha serial de 3 ou 4 fios** (relógio, dados, seleção e, às
  vezes, reset). Exemplos do tipo: Futaba série "BT" com driver interno, ou módulos chineses parecidos.
- Consequência: provavelmente é um **display de caracteres**, como o do SP103, só que serial. O amp manda letras, não
  pontos. Muitos desses controladores também aceitam **caracteres desenhados** (CGRAM), o que permitiria o VU meter.
- Placa: `MR780-DISPLAY-D.DDB`, P/N `0021-0101-0002`, de 2020-09-24.

### A ligação do display com a placa principal

- Um **cabo flat (FFC)** vai da placa do display até a placa principal (conector CN5, perto do microcontrolador).
  É por ele que passam os fios do VFD e, provavelmente, os botões e o receptor do controle remoto.
- Um par de fios amarelos sai do transformador e vai até a placa do display: deve ser a alimentação do filamento
  ou da fonte do VFD (corrente alternada de baixa tensão). Não mexer.

### A placa principal (`placa-principal-1.jpg`, `placa-principal-2.jpg`)

- Placa `MR780_Main_PCB`, P/N `0021-0100-2000`, de 2021-12-21.
- **Microcontrolador principal**: um chip quadrado de 64 pernas (LQFP-64) perto do cabo flat, com um cristal de
  25 MHz ao lado. A marca não dá para ler nas fotos: precisa de foto de perto.
- Um chip de 20 pernas (U15) entre o microcontrolador e o cabo flat: pode ser um conversor de nível (3,3 V ↔ 5 V)
  para o display. Precisa de foto de perto.
- **DAC Wolfson WM8740** (confirma o boato do fórum).
- **XMOS** (a entrada USB de computador).
- **Módulo DAB/FM verde da Frontier Silicon** (o chip parece ser um Kino4), com a antena pelo cabo coaxial fino.
- **Módulo Bluetooth azul** (X-FTB01, com aptX).
- Um conector com os fios `D+5V`, `APD`, `SYS`, e outro com `POW/OF` e `DGND` indo para a fonte: é por aí que o
  microcontrolador liga e desliga o amp. O `D+5V` pode ser o 5 V que fica ligado em standby: **medir**.

### Fonte e outras placas

- Fonte chaveada `MR780-SMPS-D` (2018-03-10), com um relé: é a que fica ligada em standby e liga o resto.
- Pré de válvulas `TUBE PAMP PCB` (as duas ECC81 debaixo da chapa).
- Transformador toroidal `Magnat MT 900`.
- Placas pequenas no painel para os botões giratórios (SOURCE, BALANCE, BASS, MID, TREBLE).

## O display, pelo manual do usuário

Fonte: manual oficial (`docs/manual/MR-780_Manual_022025.pdf`, cópia local; texto em `.txt` ao lado).

- "Large two-line display" (display grande de duas linhas), segundo a Magnat.
- Botão **DIM**: 3 níveis de brilho.
- O que aparece no display (item 17 do painel):
  - aquecimento da válvula, por ~30 s depois de ligar (contagem regressiva, segundo um usuário do hifi-forum.de);
  - a entrada escolhida;
  - o volume;
  - estação e recepção em FM e DAB (botão INFO do controle remoto).
- Textos citados no manual, exatamente como aparecem: `DIRECT ON`, `STORED PRESET`, `APO MODE OFF`, `APO MODE ON`,
  `BT` (pisca ao parear, fixo quando conectado), `N/A`.
- No FM, o INFO mostra em sequência: PS (nome da estação), RT (texto), PTY (tipo), hora/data.
- No DAB, o INFO mostra: nome da estação, DLS (texto, muitas vezes artista e música), tipo de programa, multiplex e
  frequência, hora/data, taxa de bits, nome do multiplex.
- Em USB/Bluetooth: identificação do aparelho, taxa de dados e as tags ID3 (título, artista, álbum).
- Nas entradas digitais, mostra a taxa de amostragem por ~5 s.

## Painel frontal

1 LED de estado (verde ligado, vermelho standby) · 2 ON/STBY · 3 fone (desliga os alto-falantes) · 4/5 ◄◄ ►► ·
6 MONO · 7 SOURCE · 8 LINE IN · 9 BALANCE · 10/11/12 BASS/MID/TREBLE · 13 DIRECT · 14 sensor do controle remoto ·
15 VOLUME · 16 DIM · 17 display.

Diferença importante para o SP103: **BASS, MID, TREBLE e BALANCE são botões giratórios**, não ajustes no menu. Então o
Pico não vai conseguir mudar o grave pelo Wi-Fi como faz no SP103 (só se forem lidos por um microcontrolador, a ver).

## Painel traseiro

Entradas analógicas: PHONO (MM ou MC de saída alta), CD, DVD, TAPE, AUX 1/2, e REC OUT. Digitais: 2 ópticas, 2
coaxiais, USB tipo B (placa de som para o computador). Antena DAB/FM. Uma entrada **UPDATE** (USB) e uma chave
**NORMAL/SERVICE** (só para atualização de firmware).

## Controle remoto

Botões do amp: AMP (liga/standby), SOURCE, TUNING ▲▼, AUTO (busca DAB), INFO, VOLUME ▲▼, MUTE, números 0–9 e +10
(presets), ◄◄ ►► e PLAY/PAUSE (USB/Bluetooth), Disconnect (Bluetooth). Os outros botões são do CD player Magnat
MCD 750. O protocolo (NEC ou outro) não aparece em lugar nenhum: vamos capturar.

## Firmware

Segundo o fórum audio.com.pl (2020): o arquivo `MR-780.bin` vai na raiz de um pendrive FAT32; chave traseira em
SERVICE; desliga, coloca o pendrive na entrada UPDATE, liga; o amp atualiza e vai para standby; chave de volta em
NORMAL. DIM + DIRECT por 3 s mostra a versão (o mesmo post avisa que isso pode apagar os presets).

Isto indica que existe um microcontrolador principal com firmware próprio. Não vamos mexer no firmware da Magnat.

## Outras coisas ditas em fóruns (sem confirmação)

- Pré com duas válvulas ECC81/12AT7 (unidades novas com JJ); potência transistorizada.
- DAC Wolfson (as fotos confirmam: WM8740).
- O tuner seria da Grundig (boato).
- Ao ligar: 30 s de aquecimento no display, depois um relé liga o resto.
- O volume "pulava" antes de uma atualização de firmware: indica que o botão de volume é lido pelo microcontrolador
  (encoder ou potenciômetro lido por ADC), e não um potenciômetro direto no sinal. Bom para nós: dá para comandar.

## Os três casos possíveis (antes das fotos)

Esta tabela foi escrita antes das fotos. As fotos indicam o **segundo caso** (VFD de caracteres com controlador
próprio), só que com o controlador dentro do vidro em vez de numa placa à parte:

| Tipo | Como reconhecer na foto | Como o controlador fala com ele | Para nós |
|---|---|---|---|
| Módulo de caracteres paralelo (HD44780 ou compatível) | Conector de 14 a 16 pinos, como no SP103 | 8 ou 4 fios de dados + RS, E | Igual ao SP103. Reaproveita quase tudo |
| Módulo VFD de caracteres (Noritake, Futaba) | Placa com um tubo de vidro e chip próprio; conector com poucos fios | Serial (SPI ou UART) ou paralelo | Médio: outro protocolo, mesma ideia |
| VFD "nu" com chip driver na placa do amp (PT6311, PT6312, HT16515 e parecidos) | Tubo de vidro soldado na placa frontal, um chip de 44 a 52 pernas perto | Serial de 3 fios (DIN, CLK, STB), mandando segmentos, não letras | Maior: o display pode ter ícones e segmentos fixos, e o que dá para desenhar depende da grade do vidro |

## Fontes

- [Manual oficial do MR 780 (PDF)](https://magnat.com/wp-content/uploads/2025/09/MR-780_Manual_022025.pdf)
- [Página do MR 780 na Magnat](https://www.magnat.de/en/electronics/mr-780/)
- [son-video.com, descrição do MR 780](https://www.son-video.com/article/amplis-connectes/magnat/mr-780)
- [Fórum audio.com.pl, tópico do MR 780, página 61 (atualização de firmware)](https://forum.audio.com.pl/topic/61267-magnat-mr780/page/61/)
- [hifi-forum.de, tópico do MR 780](http://www.hifi-forum.de/viewthread-111-5374.html)
- [RepData, MR 780 com ruído no canal direito](https://www.repdata.de/wbb2/forum/index.php?thread%2F83330-nagelneuer-hifi-r%C3%B6hren-receiver-magnat-mr-780-kratzt-und-bummst-im-rechten-lauts%2F=)
