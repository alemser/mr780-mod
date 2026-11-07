# Medições

Todas as leituras, na ordem em que foram feitas. Quem mede é o dono; o Claude anota.

## Regra da numeração do cabo flat

- 21 vias, passo de 1,0 mm.
- A numeração é a do **conector na placa do display** (`MR780-DISPLAY-D.DDB`).
- Olhando a placa do display por trás, o **pino 1 é o da ponta direita**, a do triângulo (2026-11-07). Ainda falta
  confirmar com uma marca branca na placa ou pelo lado do CN8, na placa principal.
- O flat pode ser invertido: o pino 1 daqui pode ser o 21 no CN8. Conferir com continuidade antes de usar o CN8.

## Tabela do flat

| Pino | GND? (bip) | kΩ até GND | Ligado a | Standby (V) | Ligado (V) | Observação |
|---|---|---|---|---|---|---|
| 1 | não | 13,53 | | | | |
| 2 | não | 8,62 | | | | |
| 3 | não | 8,62 | | | | |
| 4 | não | 6,98 | | | | |
| 5 | **sim** | 0 | | | | |
| 6 | não | 4,5 | | | | |
| 7 | não | 3,00 | | | | |
| 8 | não | 6,0 | | | | |
| 9 | não | 10,45 | | | | |
| 10 | não | 5,5 | | | | |
| 11 | não | 5,5 | | | | Cursor do VOLUME (varia 5–10 kΩ ao girar) |
| 12 | não | 8,74 | | | | |
| 13 | não | 0,1 | | | | |
| 14 | não | 8,74 | | | | |
| 15 | não | 10,42 | | | | SOURCE (muda ao girar) |
| 16 | não | 8,74 | | | | |
| 17 | **sim** | 0 | | | | |
| 18 | não | 3,66 | | | | |
| 19 | **sim** | 0 | | | | |
| 20 | **sim** | 0 | | | | |
| 21 | não | 8,74 | | | | |

## Registro

### 2026-11-07, amp fora da tomada, continuidade

- Ponta preta no terminal **PHONO GROUND** da traseira (que é o chassi). Bipam com ele: **pinos 5, 17, 19 e 20**.
- Os outros 17 não bipam.
- O bip dos 4 é contínuo: são GND de verdade, não capacitor carregando.
- Os pinos 5, 17, 19 e 20 bipam entre si (esperado: são todos GND).
- Entre os outros 17 pinos (1–4, 6–16, 18, 21), nenhum par bipa. Então o filamento do VFD não passa pelo flat: deve vir direto pelos fios amarelos do transformador para a placa do display.

### 2026-11-07, amp fora da tomada, resistência até o GND

Ponta preta no PHONO GROUND, multímetro em Ω (valores em kΩ, a confirmar). Valores na tabela acima. São medidas com
tudo ligado na placa (passam pelos chips), então servem para agrupar pinos, não como valor exato.

- Pino 6: a primeira leitura deu 0 (engano); medido de novo, 4,5.
- Pino 13: 0,1 kΩ (100 Ω), bem mais baixo que os outros: candidato a alimentação ou a um LED.
- Pinos 7 (3,00) e 18 (3,66): mais baixos que os de sinal: candidatos a alimentação (5 V, 3,3 V).
- Grupos de valores iguais (circuitos parecidos):
  - 2 e 3: 8,62
  - 10 e 11: 5,5 (par: pode ser um encoder, A e B)
  - 9 e 15: 10,4
  - 12, 14, 16 e 21: 8,74 (quatro iguais: botões ou dados)
- Sozinhos: 1 (13,53), 4 (6,98), 6 (4,5), 8 (6,0).

### 2026-11-07, amp fora da tomada, girando os botões (Ω até o GND)

- **VOLUME**: o **pino 11** varia de ~10 até ~5 kΩ, passando pelos valores do meio, enquanto se gira. Mudança
  gradual, não saltos de clique: parece um **potenciômetro** lido pelo microcontrolador (o pino 11 é o cursor), e
  não um encoder. Pode ser um potenciômetro motorizado (que o controle remoto gira): a conferir.
- **SOURCE**: os pinos 2 e 3 não mudam ao girar o SOURCE (agora deram acima de 12 kΩ, parados). O SOURCE não está
  nesses pinos; pode estar em outro pino ou vir por outro cabo (as plaquinhas dos botões giratórios têm fios
  próprios até a placa principal).
- O botão de VOLUME gira sem fim, sem cliques e sem batente. Com o VOL ▲▼ do controle remoto ele **não gira**: só o
  display muda os dB. Então não é motorizado: é um **potenciômetro sem fim** (tipo usado em sintetizadores), lido
  pelo microcontrolador, e o volume de verdade é digital (o microcontrolador ajusta um chip de volume). Esse tipo de
  potenciômetro costuma ter **dois cursores** defasados, para saber o sentido: o **pino 10** (mesmo valor do 11, 5,5
  kΩ) é o candidato ao segundo. Conferir girando com a ponta no pino 10.
- Para o Pico: comandar o volume pelo código do controle remoto por fio é o caminho simples. Imitar o potenciômetro
  sem fim (duas tensões analógicas) é possível, mas desnecessário.
- **SOURCE**: o **pino 15** muda de valor ao girar o SOURCE. O **pino 9** tinha o mesmo valor do 15 (10,4 kΩ): é o
  candidato ao segundo contato. Falta saber se o SOURCE tem cliques (encoder ou chave) ou gira liso
  (potenciômetro sem fim, como o VOLUME).
- O SOURCE **tem cliques**, e o **pino 9 não muda** ao girá-lo: o SOURCE usa só o pino 15. Hipótese: um encoder
  cujos dois contatos passam por resistores diferentes até o mesmo pino, e o microcontrolador lê o valor (cada
  combinação dá uma resistência). Nesse caso, os valores se repetem num ciclo de ~4 cliques. A conferir anotando o
  valor de cada clique. O pino 9 fica sem função conhecida.
