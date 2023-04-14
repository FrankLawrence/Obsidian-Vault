---
Tags: 
Created: 2023-04-14 11:58:13
---
(Links:: [[Computer Networks]])
# Guided Transmission Media
- physical layer transports bits from one machine to another
- **guided transmission media**: transmission media that rely on physical cable
- Bandwidth: carry capacity of a medium measured in **Hz**
## Persistent Storage
- write data onto magnetic or solid-state storage and physically transport to destination machine
- bandwidth is higher and cost lower than over cable
- best solution for moving *very* large amounts of data
## Twisted Pairs
- two insulated copper wires 1mm thick
- twisted together in helical form -> waves from different twists cancel out -> less radiation
- signal transmitted as difference between two wires
- no need for amplification over several kilometers
- hundreds of megabits/sec bandwidth
- **full-duplex** links: can be used in both directions simultaneously
- **half-duplex** links: used in both directions but only one way at a time
- **simplex** links: only one direction
- more twists results in less crosstalk and better quality signals
- *Category 3*: supports 100-Mbps and 1Gbps
- *Category 6*: 10-Gbps (Unshielded Twisted Pair)
- *Category 7*: shielding on individual twisted pairs
- *Category 8*: operates at shorter distances but higher speeds -> data centers
## Coaxial Cable
- better shielding and greater bandwidth
- 50-Ohm cable: digital transmission
- 75-Ohm cable: analog transmission and cable television
- stiff copper wire as core, surrounded by insulating material, encased by cylindrical conductor (woven braided mesh), covered by plastic sheath
- 6 Ghz
## Power Lines
- data signal is superimposed on low-frequency power signal
- electrical signals sent at 50-60Hz, high-rate data sent in Mhz
- data must be sent over unlicensed frequencies
## Fiber Optics
- bandwidth of fiver technology is technically 50 Tbps, with conversion between electrical and optical signals limiting the bandwidth to 100 Gbps
- optical transimission system has thee components: light source, transmission medium and detector
- pulse of light indicates 1 bit, absence of light indicates 0 bit
- simplext data transmission
- for angles of incidence above certain critical value, light is refracted back when travelling through silica
- **multimode fiber**: many different rays will be bouncing around at different angles
- **single-mode fiber**: fiber's diameter is a few wavelengths of light large (10 microns) -> guides light in straight line
	- 50 times further distance than multimode fibers
### Transmission of Light Through Fiber
- Attenuation of light: ratio of input to output signal power in a medium
- infrared part of the spectrum used to transmit data
- **chromatic dispersion**: light pulses in fiber spread out in length as the propagate
- pulses are called **solitons**
### Fiber Cables
- no braiding -> smaller diameter
- core surrounded by glass cladding with lower index of refraction than the core
- splices of two cables result in 10% light loss
- two pieces can be fused (melted) together
- response time of photodiode at receiver limits data rates to 100 Gbps
# Wireless Transmission
- **Frequency Hopping Spread Spectrum**: transmitter hops from frequency to frequency hundreds of times per second
	- resistance to jamming and fading
- **Direct Sequence Spread Spectrum**: uses code sequence to spread data signal over wider frequency band
	- multiple signals share same frequency band
- ![[Spread spetrum and ultra-wideband communication.png|450]]
- **Ultra-Wideband Communication**: sends series of low-energy rapid pulses, with varying carrier frequencies
	- can tolerate substantial amount of strong interference from other narrowband signals
	- doesn't cause harmful interference to other narrowband signals
# Using the Spectrum for Transmission
## Radio Transmission
- Radio waves: can travel long distances and penetrate buildings
- **path loss**: power falls with distance ($1/r^2$ in air)
- AM radio uses MF band -> pass through buildings, 1000 km distance, low bandwidth
- HF and VHF are absorbed by ground, refract on ionosphere
	- used for long distance radio
## Microwave Transmission
- 100 Mhz waves can be focused in small beam (parabolic antenna) -> transmitter and receiver must be aligned
- microwaves are **directional**
- **multipath fading**: delayed waves arrive out of phase with direct wave and cancel the signal
## Infrared Transmission
- do not pass through solid objects
- infrared systems do not interfere in different rooms
- no government license needed to operate infrared system
## Light Transmission
- uniderectional: each end needs laser and photodetector
- cannot penetrate rain or thick fog
# From Waveforms to Bits
## The Theoretical Basis for Data Communication
- information transmitted by varying voltage or current
### Fourier Analysis
- any reasonable behaved periodic function, $g(t)$ with period $T$ can be constructed as the sum of number of sines and cosines: $$g(t)=\frac12c+\sum_{n=1}^\infty a_n\sin(2\pi nft)+\sum_{n=1}^\infty b_n\cos(2\pi nft)$$ where $f=1/T$ is the fundamental frequency, $a_n$ and $b_n$ are the sine and cosine amplitudes of the $n$th harmonics, and $c$ is a constant that determines the mean value of the function
### Bandwidth-Limited Signals
- width of the frequency range transmitted without being strongly attenuated is called the **bandwidth**
- signals that run from 0 up to a maximum frequency are called **baseband**
- signals that are shifted to occupy a higher range of frequencies are called **passband** signals
- for digital transmission, you only need to receive a signal with just enough fidelity to reconstruct the sequence of bits sent
- ![[A binary signal and its root-mean-square Fourier amplitudes, and successive approximations to the original signal.png|500]]
- for a bit rate $b$ bits/sec, the first harmonic of the signal is $b/8$ Hz
- number of highest harmonic that can pass described by $3000/(b/8)$
	- ordinary telephone line has artificial cutoff frequency of 3000 Hz
## The Maximum Data Rate of a Channel
> [!info] Nyquist's theorem
> $$\text{Maximum data rate} = 2B\log_2 V\text{ bits/sec}$$

- **Signal-to-Noise Ratio**(**SNR**): signal power $S$ and noise power $N$ -> $10\log_{10}S/N$ expressed in **decibels** (**dB**)

---
References: