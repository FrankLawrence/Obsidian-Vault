---
Tags: 
Created: 2023-03-12 20:22:14
---
(Links:: [[Computer Organization]])
# 7.3 M 

# 7.6 E
# 7.9 M
The asynchronous bus protocol in Figure 7.6 uses a full-handshake, in which the master maintains an asserted signal on Master-ready until it receives Slave-ready, the slave keeps Slave-ready asserted until Master-ready becomes inactive, and so on. Consider an alternative protocol in which each of these signals is a pulse of a fixed width of 4 ns. Devices take action only on the rising edge of the pulse. Using the same parameters as in Problem 7.7, what is the minimum and maximum time to complete one bus transfer?
# 7.12 D
The arbiter should have access to the master-ready signal of the higher priority device. To do this, the device should enable the Bus-request line to the arbiter. The arbiter understands this request as having higherpriority and deactivets the bus-grant line of the bus master. The bus master must convey to the device, that it should stop the data transfer and save the current state, so it can carry on afterward. The arbiter then activates the bus-grant line of the high priority device and switches it off after. This allows the previous device to carry forth with the operation.
# 7.15 E
Give the logic expression for an address decoder that recognizes the 16-bit hexadecimal address FA68.
# 7.18 M
Data are stored in a small memory in an input interface connected to a synchronous bus that uses the protocol of Figure 7.5. Read and Write operations on the bus are indicated by a Command line called R/W. The speed of the memory is such that two clock cycles are required to read data from the memory. Design a circuit to generate the Slave-ready response of this interface.


---
References: