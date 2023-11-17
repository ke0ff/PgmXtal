# PgmXtal
ATtiny-85 Project to control an si598 oscillator (MFR P/N: 598ACA000121DG was used for software verification).

The code is for the Atmel Studio 7 (now Microchip Studio 7) SW design environment.  The I2C/UART portion
(i.e., everything NOT in main.c) was plaigurized from the internet - unfortunately,
I didn't keep notes on where I found the code, and the author didn't identify themselves in all of the source file comments.  SO...
my bad.  Where the author is so identified, I agree to honor their license (as should anyone who choses to use and/or redistribute and code found within this repo - see https://github.com/ke0ff/PgmXtal/blob/main/LICENSE).

The Atmel Studio 7 software project folder is simply zipped into the SW subfolder.  Updated files may be added individually to that folder as the design matures.
So, to reconstruct the project, unzip the archive to your PC.  Then, if there are any .c or .h files inthe SW folder,
download them and place them into the folder structure for the project (you'll have to dig a bit to get down to them).
Here are the links for the part datasheets:

https://www.skyworksinc.com/-/media/SkyWorks/SL/documents/public/data-sheets/si598-99.pdf

https://www.mouser.com/datasheet/2/268/Atmel_2586_AVR_8_bit_Microcontroller_ATtiny25_ATti-1315542.pdf

ATTINY85 = 8pin package (SOIC or DIP):</br>
Pin1: reset (n/c)</br>
Pin2: OE (si598 pin 2)</br>
Pin3: CHSEL input - selects freq 0 or freq 1</br>
Pin4: GND</br>
Pin5: SDA (si598 pin 7)</br>
Pin6: SOUT (19200 baud, N81 debug/status serial output)</br>
Pin7: SCL (si598 pin 8, 133 Kbps)</br>
Pin8: Vdd (3.3V)</br>

Schematics and other hardware details will be added to the HW subfolder when they become available.

The SW puts the part to sleep after the si598 is intialized. CHSEL now initiates wake from sleep so that a reset is not
needed to induce a channel change.  <i>At least 1us of RC delay (in hardware) should be applied to the CHSEL signal.  More is better (to a point)</i>.

<i>Note: A 3.3V TVS on the 3.3V power connection is highly recommended to protect the oscillator and MCU from power transients.</i>

The SkyWorks part numbering system is rather difficult to decypher.  The datasheet implies that the I2C address
is depicted in the PN (the "000121" portion of the above part).  However, what appears to be happening is that
Skyworks assigns a pseudo-random 6-digit code to identify the starting frequency and I2C address.  They maintain
a database that holds the info.  Without that database, it is impossible to decypher the address and start frequency.
However, there is an app on the SkyWorks web page that allows a reverse loop-up:

https://tools.skyworksinc.com/TimingUtility/timing-part-number-search-results.aspx

This allows one to confirm the I2C address (and start-up frequency) for a given part.  0x55 (becomes 0xAA when shifted to
make room for the R/W bit) seems to be the address that many of the parts use.

