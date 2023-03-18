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
- devices derive timing information from conrol line: *bus clock*
- some lines are high, some low depending on the particular address or data values being transmitted
- signals half-way between low and high signal should be ignored

> [!example] Input (Read) operation
> - master places device address on address line and command on control lines at $t_0$
> - address decoders must be fast enough before next clock pulse $t_1$
> - master loads data on data lines into one register at end of clock cycle $t_2$

- ![[A detailed timing diagram for the input transfer.png]]
	- master sends address and command signals on rising edge $t_0$ -> delay appearing on bus at $t_{AM}$
	- slave sends requested data at time $t_1$ -> delayed appearance on bus at $t_{DM}$
- time $t_2-t_0$ must accommodate the longest delays on the bus and slowest device interface -> devices operate at speed of slowest device
- 

---
References: