# Medições

Quem mede é o dono; o Claude anota. O **resumo** abaixo é o que vale; o **registro** guarda as leituras na ordem em
que foram feitas, e algumas hipóteses dele foram corrigidas depois (o resumo diz quais).

## Resumo (fim de 2026-11-07)

Tudo medido com o amp **fora da tomada**, multímetro com a ponta preta no PHONO GROUND (chassi). Valores em kΩ.

| Pino | O que é | Certeza |
|---|---|---|
| 5, 17, 19, 20 | GND | Certo (bip contínuo, ligados entre si) |
| 7 | Botão ON/STBY | Certo (perninha do botão até o pino, em Ω) |
| 8 | Botão ►► | Provável (cai de 8 para 6 apertando) |
| 9 | Botão MONO | Provável (cai de 10 para 7 apertando) |
| 10 | Botão ◄◄ | Certo (perninha do botão até o pino) |
| 11 | Botão DIM | Certo (perninha do botão até o pino); também variou com o VOLUME, provavelmente eco |
| 12 | Botão DIRECT | Certo (vai a ~0 apertado: sem resistor em série) |
| 13, 14 | VOLUME | Os dois mudam ao girar. Potenciômetro sem fim ou encoder sem cliques: fica para o analisador |
| 15, 16 | SOURCE (encoder com cliques, contatos A e B) | Certo |
| 21 | Receptor do controle remoto, perninha 1: **sinal** | Provável (bip direto com a perninha) |
| 1, 2, 3, 4, 6, 18 | A descobrir: alimentação da lógica, alimentação do receptor e os dados do VFD | — |

O que isto quer dizer para o projeto:

- **Todos os botões e os dois botões giratórios passam pelo flat.** O Pico, ligado só nele, pode ler e "apertar"
  tudo, inclusive o DIRECT (que o controle remoto não tem).
- Os botões, fora o DIRECT, têm um **resistor em série** (centenas de Ω a dezenas de kΩ); **cada botão tem o seu
  pino** (não é uma escada única).
- O **filamento do VFD não passa pelo flat** (nenhum par de pinos bipa entre si): ele vem pelos fios amarelos do
  transformador direto para a placa do display.
- Os **dados do VFD** devem estar entre 1, 2, 3, 4, 6 e 18. Os pinos 2 e 3 têm o mesmo valor (8,62): podem ser um par
  (relógio e dados).

Em aberto:

1. **Perninha 3 do receptor** (a da direita), a alimentação dele: já se sabe que não vai para 1–3 nem 7–17; falta
   medir nos pinos 4, 6, 18 e 21. E falta anotar se o receptor foi olhado pela frente (bolinha virada para quem olha) ou
   por trás.
2. **Pino 1 do flat**: confirmar qual ponta é (marca branca na placa ou pelo CN8).
3. Confirmar que o multímetro estava em kΩ.
4. **Com o amp na tomada**: tensões dos pinos 1, 2, 3, 4, 6, 18 e 21, em standby e ligado. No flat, só com a extensão
   de 22P (passo de 1,0 mm é estreito demais para a ponta com o amp ligado). Antes disso, com segurança: o conector de
   força da placa principal (`D5V`, `VCC`) e os fios `APD`, `SYS` e `POW/OF`.

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
| 7 | não | 3,00 | | | | Botão ON/STBY |
| 8 | não | 6,0 | | | | Botão ►► (provável) |
| 9 | não | 10,45 | | | | Botão MONO (provável) |
| 10 | não | 5,5 | | | | Botão ◄◄ |
| 11 | não | 5,5 | | | | Botão DIM (também variou com o VOLUME: eco) |
| 12 | não | 8,74 | | | | Botão DIRECT (vai a ~0 apertado) |
| 13 | não | 0,1 | | | | VOLUME |
| 14 | não | 8,74 | | | | VOLUME |
| 15 | não | 10,42 | | | | SOURCE, contato A |
| 16 | não | 8,74 | | | | SOURCE, contato B |
| 17 | **sim** | 0 | | | | |
| 18 | não | 3,66 | | | | |
| 19 | **sim** | 0 | | | | |
| 20 | **sim** | 0 | | | | |
| 21 | não | 8,74 | | | | Receptor do controle remoto, perninha 1 (sinal, provável) |

## Registro

### 2026-11-07, amp fora da tomada, continuidade

- Ponta preta no terminal **PHONO GROUND** da traseira (que é o chassi). Bipam com ele: **pinos 5, 17, 19 e 20**.
- Os outros 17 não bipam.
- O bip dos 4 é contínuo: são GND de verdade, não capacitor carregando.
- Os pinos 5, 17, 19 e 20 bipam entre si (esperado: são todos GND).
- Entre os outros 17 pinos (1–4, 6–16, 18, 21), nenhum par bipa. Então o filamento do VFD não passa pelo flat: deve
  vir direto pelos fios amarelos do transformador para a placa do display.

### 2026-11-07, amp fora da tomada, resistência até o GND

Os palpites desta parte (alimentação no 7 e no 13, par 10/11) foram desmentidos depois: ver o resumo.

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

Superado depois: o VOLUME é nos pinos 13 e 14 (não 11 e 10) e o SOURCE nos 15 e 16 (não só no 15).

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
- Apertando pela frente, em Ω no pino 8: o **►►** leva o valor de **8 para 6 kΩ**. Provável ►► = pino 8 (queda pequena; a
  conferir se nenhum outro pino cai mais com o ►► apertado).
- Apertando pela frente, em Ω no pino 9: o **MONO** leva o valor de **10 para 7 kΩ ou menos**. MONO = pino 9. A queda
  parecida com a do ►► no pino 8 reforça que esses botões têm um resistor em série, e ►► = pino 8.
- **Sobram os pinos 1, 2, 3, 4, 6, 18 e 21**: receptor do controle remoto, dados do VFD e alimentação.

### 2026-11-07, amp fora da tomada, receptor do controle remoto

- O receptor fica numa plaquinha separada, entre o VOLUME e o DIRECT, ligada por um cabinho à placa do display.
- **Perninha do meio = GND.** É a pinagem típica dos receptores de 38 kHz como o VS1838B (sinal, GND, alimentação):
  então uma das pontas é o sinal e a outra a alimentação.
- **Perninha 1** (a da esquerda, contando da esquerda para a direita como o dono estava olhando) **bipa com o pino 21**.
  O pino 21 tinha 8,74 kΩ até o GND e não mudava com nenhum botão: combina com a saída (sinal) de um receptor de IR,
  que fica parada em nível alto. Candidato forte: **pino 21 = sinal do controle remoto**.
- **Perninha 3** (a da direita), em Ω: pinos **1 a 3** e **7 a 17**: sem valor (aberto). Faltam os pinos 4, 6, 18 e 21.
