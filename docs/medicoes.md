# Medições

Todas as leituras, na ordem em que foram feitas. Quem mede é o dono; o Claude anota.

## Regra da numeração do cabo flat

- 21 vias, passo de 1,0 mm.
- A numeração é a do **conector na placa do display** (`MR780-DISPLAY-D.DDB`).
- Olhando a placa do display por trás, o **pino 1 é o da ponta direita**, a do triângulo (2026-11-07). Ainda falta
  confirmar com uma marca branca na placa ou pelo lado do CN8, na placa principal.
- O flat pode ser invertido: o pino 1 daqui pode ser o 21 no CN8. Conferir com continuidade antes de usar o CN8.

## Tabela do flat

| Pino | GND? (amp fora da tomada) | Ligado a | Standby (V) | Ligado (V) | Observação |
|---|---|---|---|---|---|
| 1 | não | | | | |
| 2 | não | | | | |
| 3 | não | | | | |
| 4 | não | | | | |
| 5 | **sim** | | | | |
| 6 | não | | | | |
| 7 | não | | | | |
| 8 | não | | | | |
| 9 | não | | | | |
| 10 | não | | | | |
| 11 | não | | | | |
| 12 | não | | | | |
| 13 | não | | | | |
| 14 | não | | | | |
| 15 | não | | | | |
| 16 | não | | | | |
| 17 | **sim** | | | | |
| 18 | não | | | | |
| 19 | **sim** | | | | |
| 20 | **sim** | | | | |
| 21 | não | | | | |

## Registro

### 2026-11-07, amp fora da tomada, continuidade

- Ponta preta no terminal **PHONO GROUND** da traseira (que é o chassi). Bipam com ele: **pinos 5, 17, 19 e 20**.
- Os outros 17 não bipam.
