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
| 7 | não | 3,00 | | | | Botão ON/STBY (talvez escada de botões) |
| 8 | não | 6,0 | | | | Botão ►► |
| 9 | não | 10,45 | | | | Botão MONO |
| 10 | não | 5,5 | | | | Botão ◄◄ |
| 11 | não | 5,5 | | | | Botão DIM (variou com o VOLUME: eco? conferir) |
| 12 | não | 8,74 | | | | Botão DIRECT (vai a ~0 apertado) |
| 13 | não | 0,1 | | | | VOLUME (contato?) |
| 14 | não | 8,74 | | | | VOLUME (contato) |
| 15 | não | 10,42 | | | | SOURCE, contato A |
| 16 | não | 8,74 | | | | SOURCE, contato B |
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
- O SOURCE gira **sem fim**. Clique a clique no pino 15, os valores (kΩ) vieram mais ou menos assim: 10, 5, 4, 3, 2,
  5, 4, 3, 6, 8, 5, 4, 3, 2, 1… sem um ciclo claro e sem voltar ao 10. **Inconclusivo**: o multímetro é lento demais
  para um encoder, e o valor que ele mostra depende de quanto tempo se espera. Num encoder com cliques, parado no
  clique ele costuma dar sempre o mesmo valor; a mudança só acontece entre um clique e outro. Fica para o
  analisador lógico (ou para a tensão do pino 15 com o amp ligado).

### 2026-11-07, amp fora da tomada, apertando os botões (Ω até o GND)

- **Pino 12**: só o **DIRECT** (botão no extremo direito, acima do painel) muda o valor. Os outros botões não.
- O DIRECT leva o pino 12 para um valor **perto de 0**: o botão liga o pino ao GND. Para o Pico "apertar", basta
  puxar o pino 12 para o GND por um instante.
- **Pino 14**: muda **só ao girar o VOLUME**. Então o VOLUME usa os **pinos 11 e 14** (os dois cursores do
  potenciômetro sem fim); o pino 10 não é o segundo cursor, como se pensava.
- **Pino 16**: muda **só ao girar o SOURCE**. Então o SOURCE usa os **pinos 15 e 16**: um encoder comum, com os
  dois contatos (A e B) em pinos separados. O Pico consegue imitá-lo puxando os dois pinos para o GND na ordem certa.
- **Pino 21**: nenhum botão nem giro muda o valor.
- Outros botões (variações mais fracas que a do DIRECT, ambíguas):
  - pino 8: muda pouco, mas dá para perceber, com ◄◄ e com ►►;
  - pino 9: MONO;
  - pino 10: ◄◄ e MONO;
  - pino 11 (cursor do VOLUME): MONO e ►►;
  - pino 13: muda ao girar o VOLUME.
  Um botão mexendo em vários pinos, e pouco, indica que o multímetro está medindo "por tabela" através dos outros
  componentes da placa. Para saber o pino de cada botão com certeza: continuidade direta da perninha do botão até o
  pino do flat.
- Continuidade direta das perninhas dos botões até o flat: **só o lado GND do ON/STBY bipa (com o pino 5)**. O
  outro lado de cada botão não bipa com nenhum pino. Então os botões do painel (fora o DIRECT) **passam por resistores**
  antes do flat: provavelmente uma escada de resistores lida pelo microcontrolador, como sugerem os resistores R32–R35
  ao lado dos botões K2–K4 na placa do display.
- Em Ω, do lado de sinal do **ON/STBY** até o flat: o **pino 7** dá um valor bem baixo. O ON/STBY chega ao pino 7
  (por um resistor pequeno). O pino 7 deixa de ser candidato a alimentação: é a linha de pelo menos um botão (talvez a
  escada inteira).
- Em Ω, do lado de sinal do **◄◄** até o flat: o **pino 10** dá bem baixo. ◄◄ = pino 10.
- ON/STBY no 7 e ◄◄ no 10: **cada botão tem o seu pino** (com um resistor em série), não uma escada única.
- ►► e MONO: difíceis de alcançar (capa de borracha por cima).
- **Correção do dono:** onde se leu "MONO dá baixo no pino 11", era o **DIM**. O DIM tem **3 perninhas em triângulo**
  (não em fila). Então **DIM = pino 11**, pelo lado de sinal do botão.
- **Conflito a resolver:** o pino 11 também variou bastante ao girar o VOLUME. Hipótese nova: o VOLUME é um **encoder
  sem cliques** (gira liso, mas liga e desliga contatos), nos **pinos 13 e 14**: o 14 mudou só com o VOLUME, o 13
  também mudou com o VOLUME, e o 13 deu 0,1 kΩ parado (um contato do encoder que estava fechado naquela posição). A
  variação no 11 seria eco. Teste: bip no pino 13 (e depois no 14), girando o VOLUME devagar: se o bip liga e desliga,
  é contato de encoder.
- Bip girando o VOLUME devagar: **nunca bipa** (pinos 11, 13 e 14). Não desempata: se os contatos do VOLUME passam
  por resistores em série, como os dos botões, o bip (que só soa abaixo de ~50 Ω) não liga mesmo sendo encoder.
  Fica para a tensão com o amp ligado ou para o analisador lógico. O que se sabe: o VOLUME mexe nos pinos 13 e 14
  (e no 11, que é o DIM).
- Apertando pela frente, em Ω no pino 8: o **►►** leva o valor de **8 para 6 kΩ**. Provável ►► = pino 8 (queda pequena, a confirmar se nenhum outro pino cai mais;
  ou só oscila, porque antes o ◄◄ também mexia um pouco no 8).
- Apertando pela frente, em Ω no pino 9: o **MONO** leva o valor de **10 para 7 kΩ ou menos**. MONO = pino 9. A queda
  parecida com a do ►► no pino 8 reforça que esses botões têm um resistor em série, e ►► = pino 8.
- **Sobram os pinos 1, 2, 3, 4, 6, 18 e 21**: receptor do controle remoto, dados do VFD e alimentação.

### 2026-11-07, amp fora da tomada, receptor do controle remoto

- O receptor fica numa plaquinha separada, entre o VOLUME e o DIRECT, ligada por um cabinho à placa do display.
- **Perninha do meio = GND.** É a pinagem típica dos receptores de 38 kHz como o VS1838B (sinal, GND, alimentação):
  então uma das pontas é o sinal e a outra a alimentação.
