# STM32L4 embedded labs

Two lab projects from ECSE 444 (Microprocessors) at McGill University, Fall
2026. They run on the B-L4S5I-IOT01A board (STM32L4S5, Cortex-M4F) and were
built with STM32CubeIDE.

The labs were done in pairs. These are the two parts I wrote.

## dac-tone-generator

Plays a sine tone on the DAC. A 32-sample table is built at startup, timer TIM2
sets the sample rate, and DMA feeds the DAC so the main loop does nothing. The
blue button raises an interrupt that switches between three notes (C6, E6, G6)
by changing the timer period. Setting `USE_DMA` to 0 writes each sample from
the timer interrupt instead.

## freertos-sensor-uart

Reads the board's humidity, pressure, acceleration and magnetic field sensors
over I2C and prints one of them on the UART, ten times a second. Three FreeRTOS
tasks share the work: one reads the sensors, one watches the button and changes
which sensor is printed, one prints. A mutex protects the shared readings.

## Building

Each folder has my code (`Core/`), the linker scripts and the CubeMX project
file (`.ioc`). The vendor code is not in the repository. Open the `.ioc` in
STM32CubeMX to generate the HAL drivers and FreeRTOS, then add:

- `dac-tone-generator`: CMSIS-DSP (`arm_math.h` and the Cortex-M4F library),
  for `arm_sin_f32`.
- `freertos-sensor-uart`: the B-L4S5I-IOT01 board support package from
  STM32CubeL4, for the sensor drivers.
