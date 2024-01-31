---
Tags: 
Created: 2023-04-14 23:04:47
---
(Links:: [[Computer Networks]])
# 2.6 `fas:CheckCircle`
$f=\frac c\lambda=\frac{299.792.458}{0,01m}\approx 29,98 Ghz$
$f=\frac c\lambda=\frac{299.792.458}{1m}\approx 299,8 Mhz$
This freqeuncy range represents roughly $3*10^8-3*10^{10} Hz$ which lies in the UHF and SHF frequency bands.
# 2.12 `fas:CheckCircle`
Is the Nyquist theorem true for high-quality single-mode optical fiber or only for copper wire?

The Nyquist theorem only works for a noiseless channel, and as such would only work for high-quality optical fiber.
# 2.18 `fas:CheckCircle`
A modem constellation diagram similar to Fig. 2-17 has data points at (0, 1) and (0, 2). Does the modem use phase modulation or amplitude modulation?

Since both data points have the same angle to the x-axis it must use the distance from the origin to distinguish the symbols. It uses Amplitude modulation.
# 2.24 `fas:CheckCircle`
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
# 2.30 `fas:CheckCircle`
What is the maximum bit rate achievable in a V.32 standard modem if the baud rate is 9600 and no error correction is used?

From Figure 2-27 we can see that with V.32 there are 4 Bits/symbol
$9.600*4=38.400$ Bps
# 2.36 `fas:CheckCircle`
If a T1 carrier system slips and loses track of where it is, it tries to resynchronize using the first bit in each frame. How many frames will have to be inspected on average to resynchronize with a probability of 0.001 of being wrong?

$0.001=\frac1{1000}$ -> every 1000 frames have to be resynchronized.
# 2.42 `fas:CheckCircle`
What is the difference, if any, between the demodulator part of a modem and the coder part of a codec? (After all, both convert analog signals to digital ones.)

A modem implements OFDM (Orthoganol Frequency Divison Multiplexing), while codecs use the PCM (Pulse Code Modulation) technique.
# 2.48 `fas:CheckCircle`
Make a rough estimate of the number of PCS microcells 100 m in diameter it would take to cover San Francisco (120 square km).

$r=100/2=50m$
The volume for a circle is $V=\pi r^2$
One microcells has a volume of $V=\pi 50^2=7854m^2$
$120\,000\,000m^2/7854m^2=15\,279$ microcells
# 2.54 `fas:CheckCircle`
The 66 low-orbit satellites in the Iridium project are divided into six necklaces around the earth. At the altitude they are using, the period is 90 minutes. What is the average interval for handoffs for a stationary transmitter?

With 66 low-orbit satelites, a stationary transmitter will always have sight to at least one of them. The handoff depends on when a satellite is not anymore visible (beyond the horizon) and when the next satellite can be seen. Since the period is 90 minutes the next satellite will be in sight after at most 90 minutes. So the average handoff interval is 90 minutes.



---
References: