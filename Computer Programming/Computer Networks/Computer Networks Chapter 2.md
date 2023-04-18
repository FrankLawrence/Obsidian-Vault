---
Tags: 
Created: 2023-04-14 23:04:47
---
(Links:: [[Computer Networks]])
# 2.6 `ris:CheckboxCircle`
$f=\frac c\lambda=\frac{299.792.458}{0,01m}\approx 29,98 Ghz$
$f=\frac c\lambda=\frac{299.792.458}{1m}\approx 299,8 Mhz$
This freqeuncy range represents roughly $3*10^8-3*10^{10} Hz$ which lies in the UHF and SHF frequency bands.
# 2.12 `ris:CheckboxCircle`
Is the Nyquist theorem true for high-quality single-mode optical fiber or only for copper wire?
The Nyquist theorem only works for a noiseless channel, and as such would only work for high-quality optical fiber.
# 2.18 `ris:CheckboxCircle`
A modem constellation diagram similar to Fig. 2-17 has data points at (0, 1) and (0, 2). Does the modem use phase modulation or amplitude modulation?
Since both data points have the same angle to the x-axis it must use the distance from the origin to distinguish the symbols. It uses Amplitude modulation.
# 2.24 `ris:CheckboxCircle`
A CDMA receiver gets the following chips: (-1 +1 -3 +1 -1 -3 +1 +1). Assuming the chip sequences defined in Fig. 2-22(a), which stations transmitted, and which bits did each one send?
$S=(-1 +1 -3 +1 -1 -3 +1 +1)$
$S\cdot A=(+1-1+3+1-1+3+1+1)\to(+1-1+3+1-1+3+1+1)/8=1$
$S\cdot B=(+1-1-3-1-1-3+1-1)\to(+1-1-3-1-1-3+1-1)/8=-1$
$S\cdot C=(+1+1+3+1-1-3-1-1)\to(+1+1+3+1-1-3-1-1)/8=0$
$S\cdot D=(+1+1+3-1+1+3+1-1)\to(+1+1+3-1+1+3+1-1)/8=1$
From this we can conclude that:
- station A sent a 1 bit
- station B sent a 0 bit
- station C sent nothing
- station D sent a 1 bit
# 2.30 `ris:CheckboxCircle`
What is the maximum bit rate achievable in a V.32 standard modem if the baud rate is 9600 and no error correction is used?
From Figure 2-27 we can see that with V.32 there are 4 Bits/symbol
$9.600*4=38.400$ Bps
# 2.36
If a T1 carrier system slips and loses track of where it is, it tries to resynchronize using the first bit in each frame. How many frames will have to be inspected on average to resynchronize with a probability of 0.001 of being wrong?
# 2.42
What is the difference, if any, between the demodulator part of a modem and the coder part of a codec? (After all, both convert analog signals to digital ones.)
# 2.48
Make a rough estimate of the number of PCS microcells 100 m in diameter it would take to cover San Francisco (120 square km).
# 2.54
The 66 low-orbit satellites in the Iridium project are divided into six necklaces around the earth. At the altitude they are using, the period is 90 minutes. What is the average interval for handoffs for a stationary transmitter?
# 2.60
Multiplexing STS-1 multiple data streams, called tributaries, plays an important role in SONET. A 3:1 multiplexer multiplexes three input STS-1 tributaries onto one output STS-3 stream. This multiplexing is done byte for byte. That is, the first three output bytes are the first bytes of tributaries 1, 2, and 3, respectively. The next three output bytes are the second bytes of tributaries 1, 2, and 3, respectively, and so on. Write a program that simulates this 3:1 multiplexer. Your program should consist of five proc- esses. The main process creates four processes, one each for the three STS-1 tributaries and one for the multiplexer. Each tributary process reads in an STS-1 frame from an input file as a sequence of 810 bytes. They send their frames (byte by byte) to the mul- tiplexer process. The multiplexer process receives these bytes and outputs an STS-3 frame (byte by byte) by writing it to standard output. Use pipes for communication among processes.


---
References: