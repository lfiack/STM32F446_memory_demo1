# STM32F446ZCT6 external SDRAM, SD Card and audio CODEC demo
The goal of this project is just to familiarise with stuff I never really play with :
* SDRAM
* SD Card
* Audio CODEC
* QFN package soldering

And also, more ambitious routing.

## Main components
I inspired from the STM32F429I-DISCO board.
* MCU is a STM32F446ZCT6 (only one available that allows SDRAM)
	* [Farnell link](https://fr.farnell.com/stmicroelectronics/stm32f446zct6/mcu-32bits-cortex-m4-180mhz-lqfp/dp/2488314?ost=stm32f446zct)
* SDRAM is the IS42S16400J-6TLI from ISSI (the same as the DISCO board)
	* [Farnell link](https://fr.farnell.com/integrated-silicon-solution-issi/is42s16400j-6tli/sdram-64mbit-166mhz-tsop-ii-54/dp/2901167?st=is42s16400j)
* SD-Card slot is the first one I found on Farnell
	* [Farnell link](https://fr.farnell.com/amphenol-icc-fci/10067847-001rlf/carte-memoire-connecteur-11voies/dp/2135990?st=embase%20carte%20sd)
* The CODEC is the SGTL5000XNLA3 from NXP (also one of the firsts on Farnell)
	* [Farnell link](https://fr.farnell.com/nxp/sgtl5000xnla3/codec-stereo-headphone-amp-20qfn/dp/2308050)

### The SDRAM : IS42S16400J-6TLI from ISSI
Let's start by checking the configuration in CubeIDE (and let's see if I can justify everything from the datasheet).
* Mode
	* Clock and chip enable : SDCKE1+SDNE1 -> It's just how it's wired on the STM32
	* Internal bank number : 4 banks -> "1 Meg Bits x 16 Bits x 4 Banks"
	* Address : 12 bits -> Schematics page 2 ; Table page 1 (Row addresses/Column addresses)
	* Data : 16 bits -> Quite obvious
	* 16-bit byte enable : Yes -> LDQM, UDQM, page 6
* SDRAM control
	* Bank : SDRAM bank 2 -> No choice
	* Number of column address bits : 8 bits -> Table page 1 (Row addresses/Column addresses)
	* Number of row address bits : 12 bits -> Table page 1 (Row addresses/Column addresses)
	* CAS latency : 3 memory clock cycles (Programmable CAS latency (2, 3 clocks))
	* Write protection : Disabled -> This one will be hard to justify
	* SDRAM common clock : 2 HCLK clock cycles
	* SDRAM common burst read : Disabled
	* SDRAM common read pipe delay : 1 HCLK clock cycle
* SDRAM timing in memory clock cycles
	* Load mode register to active delay : 2
	* Exit self-refresh delay : 7
	* Self-refresh time : 4
	* SDRAM common row cycle delay : 7
	* Write recovery time : 3
	* SDRAM commomn row precharge delay : 2
	* Row to column delay : 2
