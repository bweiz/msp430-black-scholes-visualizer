# MSP430 Black-Scholes Option Price Visualizer

A two-MCU embedded demo that computes a Black-Scholes **call option** price on an MSP430 and **visualizes the deviation** between market price and theoretical price using an 8-LED bar over I2C.

- **Master (MSP430FR2355):** keypad + rotary encoder + 16x2 LCD UI, Black-Scholes math, I2C master
- **Slave (MSP430FR2311):** I2C slave that drives an 8-LED bar (1 byte = 8 LEDs)

## What it does
1. Choose a parameter on the keypad (Stock, Strike, Time, Volatility, Risk-free rate, Market Price).
2. Use the rotary encoder to adjust the value (press `*` to change step size).
3. Press `#` to compute the Black-Scholes call price.
4. LCD shows:
   - theoretical call price
   - market price
   - percent difference
5. LED bar lights based on the magnitude of the percent difference.

## UI Controls (Master)
- `1`..`6`: select parameter to edit  
- `*`: cycle encoder step size (x10, x1, 0.1, 0.01)
- `C`: confirm/store the edited value and return to menu
- `#`: compute and show results

## LED Bar Visualization
The master sends a single byte to the slave at I2C address `0x40`. Each bit maps to one LED (MSB = leftmost bar). The number of bars is derived from the percent difference magnitude (capped to 8).

## Repo Structure
- `firmware/master_fr2355/` — UI + compute + I2C master
- `firmware/ledbar_slave_fr2311/` — I2C LED bar slave
- `docs/` — architecture + demo notes
- `hardware/` — pin maps + wiring notes
- `assets/` — photos + demo GIF

## Build / Flash
This project was developed for TI MSP430 using Code Composer Studio.
- Open the master project under `firmware/master_fr2355/`
- Open the slave project under `firmware/ledbar_slave_fr2311/`
- Build + flash each target board
- Power both boards and connect I2C + GND between them

## Notes
- Floating-point math is used for Black-Scholes (log/sqrt/exp/erf).
- The system is designed as a clean example of UI + math + peripheral integration (GPIO, timers, interrupts, I2C).

## Demo
Add `assets/demo.gif` and a couple photos in `assets/photos/` once you record it.

