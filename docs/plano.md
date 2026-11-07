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

## Fases (decisão do dono, 2026-10-30)

**Fase 1: ler o estado.** Saber o que vai para o display e se o amp está ligado ou em standby. Um Pico 2 W dentro do
amp, só escutando o cabo flat. Nada cortado.

**Fase 2: escrever no display.** O Pico no meio das vias de dados do VFD: lê o que a placa principal manda e escreve
a sua versão no display. As outras vias passam direto. Como o VFD tem fonte própria e (pelas fotos) controlador
próprio, dá para escrever qualquer texto.

**Fase 3 (se der): Pi Zero 2W dentro do amp**, alimentado pelo Magnat, recebendo o REC OUT por dentro (precisa de um
ADC I2S, como o PCM1808). A investigar: se os 5 V da placa principal aguentam ~500 mA. A fonte de standby (PN8370) não
aguenta.

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

## Passo 2: multímetro

1. Amp fora da tomada: continuidade de cada via do flat com o GND.
2. Amp **em standby** e depois **ligado**: tensão de cada via do flat contra o GND. Anotar numa tabela de 21 linhas.
   Isso separa as vias de lógica (0 V, 3,3 V ou 5 V) das de tensão alta.
3. Os fios `D+5V`, `APD`, `SYS` e `POW/OF`: qual tem 5 V em standby (para alimentar o Pico) e qual muda ao ligar
   (para o Pico saber se o amp está ligado).
4. A saída do receptor do controle remoto (para comandar o amp pelo Wi-Fi, como no SP103).

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

- O MR 780 também entra no groove-rig (IR pelo Broadlink)?
- O que mostrar no display na fase 2: os mesmos objetivos do SP103 (nomes, VU, agulha, qualidade do Bluetooth) ou
  outros?
