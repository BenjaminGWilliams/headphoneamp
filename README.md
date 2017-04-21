# headphoneamp

## How it works?


### AC Power Supply

-J1		Input 13.5 - 29 VAC (no load). Edge of letting ripple through worst case. 12 VAC tx or lower AC line voltage caused ps to fall out of reg and let AC ripple through regs. On semi regs reach drop out voltage at 11.5 VAC loaded. If driving low impedance at high current levbels need at least 12 VAC loaded

-D3		1/2 wave rec for +ve ps
-D4		1/2 wave rec for -ve ps

-C2,C3,C4,C5	filters rough 1/2 DC into reasonably constant DC
			as long as bottom of AC ripple is above dropout voltage of regs acts like full-wave without requiring center tapped or dual winding transformer

		4x caps instead of 2 to further reduce ripple, lower ESR and higher riple current capability

-U5 and U6	7812 and 7912 regulate filtered DC to +/- 12V removes 99.9% of the ripple

-C6 and C7	Improve transient response of regs by decreasing output imped at high freq (regs reject more noise)

-D1 and D5	Block battery voltage from feeding back into regs (drain battery if amp off) Schottky so 0.25-0.4V drop

-DC idle current	20 mA - 24 mA. AC current higheras 2x supplies and peak charing current higher in 1/2 supply. Quiscent currents of regs, lossers in filter caps and battery charger

-D2 and D6	Diode isolation of battery so dont see full 12 V supply railks. Schottky to max operating voltages when running on battery.

-R1 and R2	1W power resistors to limit charge current to under 1/20 of bat cap when batts fully charged (~8mA with 220 ohm) - overcharging
		When batteries at 0V dissipate under 1W each and lim max current to around 50 mA.
		Lower (150 ohm)	faster charging. Higher (270 ogm) for AC to extend batt life on constant charge

-S1		On/Off. Two pole switch (as two batteries).

-LED		Normal forward current is arround 20mA (same as rest of amp!). High efficiency sufficiently visible with 20 mA. Powered symmeterically from rails (18V to 24V)
		This is to make batteries drain at same rate

 

###Power Management


To shutdown amp with 9V bates drop to around 6-7V. Circuit prevents potentially damaging headphones with DC.

-U2		low power comparator.

A section	compares total ps voltage to LED voltage.LED has constant drop as battierse discharge so is free voltage rference without any added power consumption or other components
		Its accurate enough

-C1		removes noise from battery voltage and provides slight turn on delay



-Q1		Power MOSFET. When voltage of U2 @ pin 2 > pin 3 the comparator output pulls to negative rail turnging on this mosfet.
U2B		Also when voltage of U2 @ pin 2 > pin 3 output of U2B as an inverter to go turning on MOSFET Q2		

-C16 and C21 	provide a controlled turn on reducing transient click at power on

If battery falls too low or on battery d/ced pin 3 > pin 2 and Q1 and Q2 switch off shutting down amp.

-R25		provides hysteriss to help prevent amp from turning right back on again as now unloaded battery voltage rises.
		Power consumption in shutdown < 1mA

Turn on transient	800 mV peak (560 mV RMS) into 600 ogms at about 1.5 mS (= 1/2 of one cycle at 330 Hz)
Turn off transient	350 mV peak (230 mV RMS) at 20 mS 1/2 cycle at 25 Hz

-C8 and C9	low ESR and additional ps filtering as well as local low impedance power reserve for musical peaks
		Form star ground for the amp

-C17 and C18	Ground reference 0.22uF decoupling (bypass) caps for the output stage

-C10		input stage decoupling (bypass) caps and is not ground referenced. Ease of ground routing

###Main board


-J2		Input jack used to ground input when nothing is plugged in.
		Most sources have output impedance of <= 330 ohms. Limits input Johnson Noise to < 600 ohms.
		
-R14 and R20	With input open circuit, the Johnson Noise of these 10k input ressitance dominates noise performance 

-P1		3pin header for off board input. 

-J2		NOTE: if offboard connectors used its essential to cut small ground tracks from pin 3 and pin 4 of J2
		Otherwise inputs shorted to ground uness something is plugged into J2
		Ideally off-board jacks should be isolated from chassis and only grounded at P1 header using twisted pair kept away from PS and output

-R7 and R3	Provide series input current limiting protection for U1 when amp off or inputs overloaded. Enchance ESD capability of amp
		
-C11 and C12 	Along with R7 and R3 form RC filter to prevent significant RF from making it to op amp inputs
		where it could be demodulated and create excessive DC and noises.
		
-R14 and R20	Set input impedance to 10K which is optimal according to blogs

-U1		Non-inverting opamp for each channel. Xtalk is better than -110dB so no use in using single op amps

-R16 and R22	Chosen to keep impedances low to reduce Johnson Noise nut not excessivly load the amp increasing THC

-S2		Gain switch. Switches the ground resistors in gain stage feedback loop. If switch briefly open circuit the amp wont
		slam into supply rail from opening the feedback amp. Just dops to 1X gain. Routing kept as hosrt as possible
		By switching the ground resistors the parasitic values and crosstalk are less of an issue

-NJM2068		Apparently dead quiet, low distoration, and less power than lots ofother op amps. At moderate gains and typical headphone
		loads the output stage dominates the distortion not the NJM2068

-C19 and C20	work with the 2069 dominant pole compenstaion to optimize stability and transient response. 220pF as 1.5K value of R16 and R22

-VR1		Volume pot. Driven from U1 (it is 10K).

-C13 and C14	isolate input DC bias current of U3 and U4 from the volume control to remove rustling noise when adjusting volume
		2.2 uF for coupling
Volume Control location and gain structure	in the middle between two stages as improves noise performance

-R12 and R13	prove DC bias for output op amps. Large value to push low freq roll off from C13 and C14 down to DC.
		Wants to be small to minimize DC offset at headphones

Output stage	Parallel two devices in each IC and used dedicated IC for each channel: 2x peak current
		better channel isolation from high currents involed, lower distortion, 2x thermal dissipation





