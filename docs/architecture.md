# Architecture

## System overview
Two MSP430 boards split responsibilities:
- FR2355: UI + compute + I2C master
- FR2311: LED output peripheral over I2C

## Data flow
User input (keypad/encoder)
  -> parameter state
  -> Black-Scholes compute (call price)
  -> compare against market price (% diff)
  -> LCD text output + LED bar visualization

## I2C message
- 1 byte payload (LED mask)
- address 0x40

