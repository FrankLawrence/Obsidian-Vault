---
Tags: 
Created: 2023-03-13 23:37:44
---
(Links:: [[Computer Organization]])

# Bus Structure
- ![[I-O interface for an input device.png]]
- address placed on address line from processor is examined by all address decoders
- processor uses control lines to request READ and WRITE operation
- requested data transferred over data lines
- any machine instruction that can access memory can be used to transfer data to or from an I/O device (device input register has address)
# Bus Operation
- *bus protocol*: governs how the bus is used
	- when information may be placed on or data loaded off of bus from device/register
	- control signals indicate actions
		- control line $R/\overline W$ specifies read and write
		- data size specified by other control lines
		- control lines carry timing information
- one device play role of *master* in any data transfer: issues read or write commands to bus
## Synchronous Bus
- devices derive timing information from control line: *bus clock*
- some lines are high, some low depending on the particular address or data values being transmitted
- signals half-way between low and high signal should be ignored

> [!example] Input (Read) operation
> - master places device address on address line and command on control lines at $t_0$
> - address decoders must be fast enough before next clock pulse $t_1$
> - master loads data on data lines into one register at end of clock cycle $t_2$

- ![[A detailed timing diagram for the input transfer.png]]
	- master sends address and command signals on rising edge $t_0$ -> delay appearing on bus at $t_{AM}$
	- slave sends requested data at time $t_1$ -> delayed appearance on bus at $t_{DM}$
	- $t_2-t_{DM}$ must be longer than setup time for regsiter
- time $t_2-t_0$ must accommodate the longest delays on the bus and slowest device interface -> devices operate at speed of slowest device
- buses incorporate control signal that represent a response from the device
- ![[An input transfer using multiple clock cycle.png]]
	- request data on active edge -> delay to long -> slave cannot respond immediately
	- data ready and placed during clock cycle 3 -> slave ready signal
	- master sends new address and command signals to start in cycle 4
## Asynchronous Bus
- *handshake protocol*: exchange of command and response signals between master and slave
- control line master-ready indicates ready for a data transfer
	- devices decode address from bus -> slave performs operation -> activates Slave-ready
	- master waits for Slave-ready before removing signals from bus
- ![[Handshake control of data transfer during an input operation.png]]
- Master-slave signal should not arrive at device ahead of the address command information -> $t_1-t_0$ is longer than maximum possible bus skew (lines have different propagation speeds)
- $t_2 − t_1$ depends on the distance between master and slave and on the delays introduced by the slave’s circuitry
- Master-ready signal dropped when data has been received
- device receives end of Master-ready signal, and removes data and Slave-ready signals from bus

> [!info]- Trade-off factors for bus protocol design
> - Simplicity of device interface
> - Ability to accommodate device interfaces that introduce different amounts of delay
> - Total time required for a bus transfer
> - Ability to detect error resulting from addressing a nonexistent device or from an interface malfunction

- ✅ asynchronous bus doesn't need clock signal to arrive at all devices at about the same time 
- ✅ delays from interface circuits or propagation are accomodated
- ❌ transfer requires two round-trip delays
- high speed buses use synchronous approach
## Electical Considerations
- *bus driver*: logic gate (*tri-state gate*) that places data on the bus
- control input enables driver, and passes through corresponding value of input signal
# Arbitration
- *arbiter circuit*: decides which device will have access to slave first
	- based on *request*, grant use of slave to device with higher priority
- some devices produce error on delay in gaining access to bus -> give high priority
# Interface Circuits
- *port*: side where data is transfered between interface and I/O device (parallel and serial port)
	- parallel port: multiple bits of data transfered simultaneously
 
> [!tip]- Functions of an I/O interface
> 1. Provides register for temporary storage
> 2. Status register contains status information accessed by processor
> 3. Control register holds information on behavior of interface
> 4. Address-decoding for when it is being addressed
> 5. Generates required timing signals
> 6. Format conversion on transfered data between processor and I/O device

## Parallel Interface
### Input Interface
- connection of a keyboard to a processor through two registers: data register `KBD_DATA`, and status register `KBD_STATUS`
- ![[Keyboard to processor connection.png]]
	- Output of encoder:
		- Data output represents one byte of an encoded character -> `KBD_DATA`
		- control signal is set to 1 -> `KBD_STATUS`
- [[An input interface circuit.png]]
### Output Interface
- two handshake signals: New-data and Ready

> [!example]+ Display
> [[Display to processor connection.png]]
> 1. Assert Ready signal -> `DOUT` in `DISP_STATUS` set to 1
> 2. Send character to `DISP_DATA`
> 	1. clear `DOUT` flag
> 	2. set new-data to 1
> 3. Diaplay sets Ready to 0 and reads data from `DISP_DATA`

## Serial Interface
- connect processor to I/O devices that transmit data one bit at a time
- parallel and serial format transformation: shift registers with parallel access capability
- [[A serial interface.png]]
- flags `SIN` and `SOUT` mantained by status and control block
- `SIN` set to 1 when new data loaded into `DATAIN` from shift register, and cleared when read by processor
- `SOUT` set to 0 when processor writes new data into `DATAOUT`, and set to 1 when data transferred from `DATAOUT` to output shift register
- double buffering used so the processor can read first character loaded into `DATAIN` from the shift registers before the second character from the serial transfer is loaded into the shift registers -> continuous stream of input data
- timing information from transmitter to reciever must be encoded in data transmitted: asynchronous and synchronous transmission:
	- **Asynchronous Transmission**: 
		- Each charater is transmitted as a Start bit followed by 8 data bits and 1 or 2 Stop bits
		- *start-stop transmission*: 1-to-0 transmission at start bit alerts receiver that data transmission is about to begin
		- stop bit ensures that start bit of next character will be recognized
		- line remains in 1 state until next character
		- local clock signal is much higher than transmission clock and uses modulo counter to make sure the signal is the same before loading bits into the register
	- **Synchronous Transmission**: 
		- receiver generates a clock that is synchronized to that of the transmitter by observing successive 1-to-0 and 0-to-1 transitions in received signal
		- adjust active edge to be in the center of bit position
		- data transmission can continue indefinitely
		- enables high data transfer rates
# Interconnection Standards


---
References: