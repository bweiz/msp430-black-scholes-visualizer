# LED Bar Slave Firmware (MSP430FR2311)

## Responsibilities
- Configure eUSCI_B0 as an I2C **slave**
- Receive 1 byte from the master
- Drive 8 GPIO pins connected to an LED bar (bitmask -> LEDs)
- Optional "status/idle" LED behavior

## I2C
- Slave address: `0x40`
- On RX interrupt:
  - read byte from RX buffer
  - apply to LED GPIO pins

## GPIO Mapping
The LED bar driver updates:
- P1.0, P1.1, P1.4, P1.5, P1.6, P1.7
- P2.0, P2.6

(Each corresponds to one LED bit in the incoming byte.)

## Key files
- `ledbar.c` — maps bitmask to output pins
- `ledbar_i2c_slave.c` — I2C slave config + RX ISR
- `main.c` — init + idle timer ISR (optional)

