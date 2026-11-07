# Plano

O caminho é o mesmo do SP103: primeiro olhar e medir, depois capturar, só então soldar. As decisões de 2026-10-28 a
2026-10-30 estão em `docs/historico/brief-2026-10.md`; este documento é o plano em vigor.

## Segurança

- O MR 780 é ligado direto na tomada (230 V) e tem válvulas: **dentro há tensões altas**, também alguns minutos depois
  de desligar (capacitores). Sempre: chave traseira em OFF, cabo fora da tomada, esperar uns 5 minutos.
- Não encostar na fonte (`MR780-SMPS-D`) nem na placa das válvulas.
- A placa do display tem a fonte do VFD (Q1, DW1, D2–D4, eletrolíticos de 50 V): **dezenas de volts**. Ligar fios só
  nas vias de lógica do cabo flat, e só depois de medir cada uma.
- O GPIO do Pico é de 3,3 V. Se o barramento do VFD for de 5 V, cada fio passa por um divisor ou conversor de nível.
- Fios só com o amp fora da tomada.

## Objetivos (dono, 2026-11-07)

1. **Saber o estado do amp**: ligado ou standby, entrada, volume, DIRECT, texto do DAB/FM.
2. **Mudar o que ele escreve no display.**
3. **Comandar sem depender do controle remoto pelo ar** (sem Broadlink).
4. **Um Raspberry Pi Zero 2 WH dentro do amp**, além do Pico, **alimentado pelo próprio amp**, sem transformador novo
   (no SP103 foi preciso pôr um).
5. Bônus: **grave e agudo** pelo Wi-Fi, e **ligar ou desligar o DIRECT**.

O que as fotos já dizem sobre cada um:

| Objetivo | Como | Situação |
|---|---|---|
| 1. Estado | O Pico escuta as vias de dados do VFD no cabo flat e remonta as telas, como no SP103. Ligado ou standby por um sinal da placa principal (`APD`, `SYS` ou `POW/OF`) | Viável. Falta medir as vias |
| 2. Mudar o display | O Pico no meio das vias de dados, entre duas extensões do flat | Viável, depende do protocolo (capturas) |
| 3. Comandos sem IR pelo ar | (a) Mandar os códigos do controle remoto **por fio**, direto na saída do receptor de IR, como no SP103 (não passa pelo ar). (b) Imitar os botões do painel (ON/STBY, DIM, DIRECT, ◄◄, ►►, MONO). (c) Imitar o giro dos botões SOURCE e VOLUME, se forem encoders | (a) é o caminho que já funciona no SP103. (b) e (c) dependem de como os botões chegam à placa principal: quase certo que pelo mesmo cabo flat |
| 4. Pi Zero alimentado pelo amp | Um conversor DC-DC pequeno (não é transformador) puxando da linha `VCC` da placa principal. Ver abaixo | Promissor. Falta medir a tensão e a folga |
| 5a. DIRECT | O controle remoto não tem DIRECT, então é imitar o botão do painel (3b). O estado já aparece no display ("DIRECT ON") | Provável |
| 5b. Grave, médio, agudo | Ver abaixo | Difícil |

### Alimentar o Pi Zero sem transformador

- A fonte de standby (`MR780-SMPS-D`, PN8370) é pequena: não serve.
- A placa principal recebe do transformador grande, pelo conector de força, as linhas
  `D5V VCC VCC DGND DGND -12V AGND +12V ACC` (`docs/img/principal-conector-forca.jpg`).
- Ela tem um regulador chaveado **XL2596S-ADJ** (até 3 A, aceita até 40 V na entrada), que faz a tensão da parte
  digital (`docs/img/principal-regulador-xl2596.jpg`). Provavelmente ele é alimentado pela linha `VCC`.
- Plano: **um módulo DC-DC próprio para o Pi Zero** (5 V, 2 a 3 A; ligado à linha `VCC`, depois do fusível), sem
  carregar o regulador do Magnat. Não usar o `+12V` e o `-12V`: são da parte de áudio, e o Pi Zero mete ruído neles.
- **A medir antes:** a tensão do `VCC` (com o amp ligado e tocando alto) e quanto o transformador aguenta a mais.
  O Pi Zero 2 W gasta uns 300 a 500 mA em 5 V (uns 2,5 W).
- **Consequência:** o transformador grande só liga quando o amp sai do standby (é o relé da fonte que liga). Então o Pi
  Zero **só fica ligado com o amp ligado**, e perde a energia de repente quando o amp vai para standby. O sistema dele
  precisa ser só leitura (como a imagem enxuta do Zero 2 W do OceanO) para o cartão não estragar. O Pico continua
  ligado em standby, pelo `D5V`, e é ele que liga o amp.
- Coincidência boa: o Pi Zero leva uns 20 a 30 s para iniciar, o mesmo tempo do aquecimento das válvulas.

### Grave, médio e agudo

Os botões BASS, MID, TREBLE e BALANCE estão numa placa comprida no painel, cheia de capacitores de áudio. Isso indica
um controle de tom analógico, com os potenciômetros direto no sinal (diferente do volume, que é lido pelo
microcontrolador). Se for isso, comandar pelo Wi-Fi exige trocar os potenciômetros por potenciômetros digitais de
áudio que aguentem ±12 V: dá, mas mexe no caminho do som e é um projeto à parte. Confirmar com fotos dessa placa.

O DIRECT desliga o controle de tom. Então "ligar e desligar o tom" pelo Wi-Fi já vem com o DIRECT.

## Fases (decisão do dono, 2026-10-30)

**Fase 1: ler o estado.** Saber o que vai para o display e se o amp está ligado ou em standby. Um Pico 2 W dentro do
amp, só escutando o cabo flat. Nada cortado.

**Fase 2: escrever no display e comandar.** O Pico no meio das vias de dados do VFD: lê o que a placa principal manda
e escreve a sua versão no display. As outras vias passam direto. Como o VFD tem fonte própria e (pelas fotos)
controlador próprio, dá para escrever qualquer texto. Junto: os comandos (IR por fio e os botões do painel).

**Fase 3: Pi Zero 2 WH dentro do amp**, alimentado pelo Magnat por um conversor DC-DC (acima), recebendo o REC OUT
por dentro (precisa de um ADC I2S, como o PCM1808).

## O cabo flat (medido em 2026-10-30)

- 21 vias, passo de 1,0 mm, ~22 mm de largura.
- Material escolhido: uma **extensão FFC 22P de 1,0 mm** (existe pronta), com o flat de 21 encostado no pino 1 e a
  folga do pino 22 calçada com plástico ou kapton. **Testar a continuidade via a via antes de ligar.** Flats de 21P,
  1,0 mm, ~10 cm, dos dois tipos (A e B), porque não dá para ver de que lado estão os contatos.
- Para a fase 2: duas extensões, com o Pico entre elas.
- Os fios do Pico vão soldados na extensão, nunca na placa do Magnat.

## Passo 1: fotos

**Feito** (2026-10-30 e 2026-11-07, `docs/img/`, resumo em `pesquisa-display.md`).

**Ainda falta, de perto e com luz:**

- A marca no vidro do VFD (canto direito, parece "CN20-..." ou "CN23"): pode dar o modelo e a pinagem.
- As legendas dos pinos do flat, dos dois lados (placa do display e a placa principal).
- O texto do microcontrolador U17 (64 pernas) e do U15 (ao lado do flat).
- A placa dos botões BASS, MID, TREBLE e BALANCE, dos dois lados (para saber se o tom é analógico).
- Os botões SOURCE e VOLUME por trás: encoder (gira sem fim, com cliques, 3 pernas e às vezes mais 2) ou
  potenciômetro.

## Passo 2: multímetro

**Fora da tomada: feito em 2026-11-07** (`docs/medicoes.md`): itens 1 e 6. Do item 4 já se sabe o pino (o sinal do
receptor vai ao pino 21); falta a tensão. Os itens 2 a 5 são com o amp ligado.

1. Amp fora da tomada: continuidade de cada via do flat com o GND.
2. Amp **em standby** e depois **ligado**: tensão de cada via do flat contra o GND. Anotar numa tabela de 21 linhas.
   Isso separa as vias de lógica (0 V, 3,3 V ou 5 V) das de tensão alta.
3. Os fios `D+5V`, `APD`, `SYS` e `POW/OF`: qual tem 5 V em standby (para alimentar o Pico) e qual muda ao ligar
   (para o Pico saber se o amp está ligado).
4. A saída do receptor do controle remoto (para comandar o amp pelo Wi-Fi, como no SP103).
5. O conector de força da placa principal: a tensão do `VCC` e do `D5V`, em standby, ligado e ligado tocando alto
   (para o conversor do Pi Zero).
6. Os botões do painel: em quais vias do flat eles aparecem, e se cada botão tem o seu fio ou se vários dividem um fio
   com resistores diferentes (escada de resistores lida pelo microcontrolador). Muda o jeito de imitá-los.

## Passo 3: analisador lógico

O mesmo Pico com o LogicAnalyzer usado no SP103, nas vias de lógica:

- Telas: ligar (contagem do aquecimento), volume, cada entrada, DIM, DIRECT, INFO no DAB e no FM, Bluetooth.
- O controle remoto: cada botão, para o protocolo e os códigos.

Decodificar e documentar em `docs/descobertas-protocolo.md`, com as capturas em `captures/`.

## Passo 4: o programa (fase 1)

Aproveitar do `sp103-display-proxy` o que não depende do display:

- Wi-Fi e atualização pelo Wi-Fi com volta automática (`ota.py`, `tools/enviar.py`).
- O estado e o contrato da app Amp Link (`/state`, `/device`, `/names`), com campos novos: fonte DAB/BT/USB, estação,
  texto do DAB/RDS.
- Os testes no Mac, usando as capturas reais.

Novo: o sniffer do VFD serial (PIO) e o leitor das telas do MR 780.

Lições do SP103 que já valem aqui:

- Sem página web no Pico: a app Android faz esse papel.
- Evitar conexões longas (`/events`) e respostas grandes: elas travam o Wi-Fi do Pico (lwIP sem memória).
- Copiar cada parte do SP103 só quando ela for usada, para não carregar código morto.

## O que a app Amp Link precisa ganhar

O `GET /device` já descreve o aparelho. O MR 780 vai ser um segundo aparelho na mesma app, com a sua lista de entradas
e sem ajuste de grave, médio, agudo e balanço (são botões giratórios no MR 780).

## Perguntas em aberto

- O que mostrar no display na fase 2: os mesmos objetivos do SP103 (nomes, VU, agulha, qualidade do Bluetooth) ou
  outros?
