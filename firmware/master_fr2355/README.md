# Master Firmware (MSP430FR2355)

## Responsibilities
- Keypad UI for parameter selection
- Rotary encoder for live value editing + step-size modes
- 16x2 LCD display (4-bit interface)
- Black-Scholes call price computation (float math)
- I2C master transmit to LED-bar slave

## Parameters
1. Stock price (S)
2. Strike price (K)
3. Time to expiration in years (T)
4. Volatility (sigma)
5. Risk-free rate (r)
6. Market price (MP)

## UI State Machine
- MODE_SELECT: choose param (1..6) or compute (#)
- INPUT_PARAM: edit selected value (encoder), `*` cycles step size, `C` confirms
- DISPLAY_RESULT: show call price + market price + % diff, drive LED bar until a keypress

## I2C Protocol (to slave)
- Slave address: `0x40`
- Payload: 1 byte, each bit controls one LED
- Update is called during result display to visualize % difference magnitude

## Implementation notes
- Encoder ISR decodes quadrature on Port 3 pins and accumulates `count` delta.
- LCD driver sends nibbles and pulses Enable; helper functions handle cursor and strings.
- Black-Scholes uses:
  - d1/d2 with `logf`, `sqrtf`, `expf`
  - normal CDF via `erff`

## Key files
- `main.c` — UI + compute + LED visualization loop
- `i2c_master.c` — single-byte writes to slave
- `lcd.c` — LCD driver (4-bit)
- `keypad.c` — keypad scan + debounce
- `rotary.c` — encoder ISR

