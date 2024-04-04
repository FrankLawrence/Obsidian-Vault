---
Tags: 
Created: 2023-04-14 11:58:13
---
(Links:: [[Computer Networks]])
Electrical, timing and other interfaces by which bits are set as signals over channels
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
- **full-duplex** links: can be used in both directions simultaneously ^ce1a72
- **half-duplex** links: used in both directions but only one way at a time ^c0e815
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
- bandwidth of fiber technology is technically 50 Tbps, with conversion between electrical and optical signals limiting the bandwidth to 100 Gbps
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
- **chromatic dispersion**: light pulses in fiber spread out in length as they propagate
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
- unidirectional: each end needs laser and photodetector
- cannot penetrate rain or thick fog
# From Waveforms to Bits
## The Maximum Data Rate of a Channel
> [!info] Nyquist's theorem
> $$\text{Maximum data rate} = 2B\log_2 V\text{ bits/sec}$$
> $B$: Bandwidth
> $V$: Number of discrete signal levels

- **Signal-to-Noise Ratio**(**SNR**): signal power $S$ and noise power $N$ -> $10\log_{10}S/N$ expressed in **decibels** (**dB**)
	- a ratio of 1000 is 30 dB

> [!info] Shannon's capacity
> $$\text{Maximum data rate} = B\log_2 (1+S/N)\text{ bits/sec}$$

## Digital Modulation
- process of converting between bits and signals that represent them
- ![[SCR-20230415-mzqp.png|500]]
- **baseband transmission**: directly convert bits into a signal
	- signal occupies frequencies from zero up to a maximum that depends on the signaling rate
- **passband transmission**: regulate amplitude, phase, or frequency of a carrier signal to convery bits
	- signal occupies a band of frequencies around the frequency of the carrier signal
- **multiplexing**: carray several signals over a single wire
### Baseband Transmission
- positive voltage represent 1 bit and negative voltage represents 0 bit -> **Non-Return-to-Zero**
- receiver maps signal samples to closes symbols
### Bandwidth Efficiency
- we need a bandwidth of at least $B/2$ Hz when the bit rate is $B$ bits/sec
- use more than two signaling levels
- use four voltages to send 2 bits at once as a single **symbol**
- signal must be sufficiently strong to be distinguished correctly
- **symbol rate**: rate at which the signal changes
- **bit rate**: symbol rate multiplied by the number of bits per symbol
### Clock Recovery
- **Manchester encoding**
	- XOR data signal with clock signal
	- when XORed with 0 level, it makes low-to-high transition (the clock )
	- when XORed with 1 level it is inverted and makes high-to-low transition
	- requires twice as much bandwidth as NRZ due to clock
- **Non-Return-to-Zero Inverted**: code 1 as a transition and 0 as no transition
- sender cannot transmit too many 0s -> break up runs of 0s 
- **[[4B-5B]]**: Every 4 bits is mapped into a 5-bit pattern
- some output combinations are being used -> nondata codes represent physical layer
- **Scrambler**: XOR data with pseudorandom sequence -> data is as random as the pseudorandom sequence
	- adds no bandwidth or time overhead
	- can still cause long runs
### Balanced Signals 
- signals that have as much positive as negative voltage -> no DC electric component
- **capacitive coupling**: passes only AC portion of a signal
- if we send a signal whose average is not zero, we waste energy as the DC component will be filtered out
### Passband Transmission
- arbitrary band of frequencies is used to pass the signal
- take **baseband** signal (0 to $B$ Hz) and shift it up by $S$ to occupy different passband ($S$ to $S+B$ Hz)
- **ASK** (**Amplitude Shift Keying**): two or more different amplitudes are used to represent symbol
- **FSK** (**Frequency Shift Keying**): two or more different tones are used
- **PSK** (**Phase Shift Keying**): carrier wave is shifted 0 or 180 degrees at each symbol period
	- **QPSK** (**Quadrate Phase Shift Keying**): four shifts represent 2 bits of information per symbol
- **Constellation Diagram** 
  ![[(a) QPSK (b) QAM-16 (c) QAM-64.png|500]] 
	- phase is indicated by angle to the origin with positive x-axis
	- amplitude is the distance from the origin
	- **QAM-16**: Quadrature Amplidute Modulation
	- **Gray code**: adjacent symbols differ in only 1 bit position -> single bit error 
## Multiplexing
### Frequency Division Multiplexing
- divide spectrum into frequency bands
- each user has exclusive possession of some band
- **guard band**: excess bandwidth

- ![[Frequency division multiplexing. (a) The original bandwidths. (b) The bandwidths raised in frequency. (c) The multiplexed channel..png|500]]
	- Frequency division multiplexing. (a) The original bandwidths. (b) The bandwidths raised in frequency. (c) The multiplexed channel.
	- overlap between adjacent channels -> strong spike in one channel will be felt in other channel as noise
- **OFDM** (**Orthogonal Frequency Division Multiplexing**): channel bandwidth is divided into many subcarriers that independtly send data
	- subcarriers extend into adjacent ones
	- subcarriers is zero at center of adjacent subcarrier -> able to sample at center frequency
	- **guard time**: repeat portion of symbol signals
### Time Division Multiplexing
- users take turns periodically getting entire bandwidth for certain time interval
- bits from each input stream are taken in a fixed **time slot**
- guard time account for timing variations
### Code Division Multiplexing
- narrowband signal is spread out over a wider frequency band
- each bit time is subdivided into $m$ short intervals called **chips** -> multiplied with original data
- typically 64 or 128 chips per bit
- **chip sequence**: each station is assigned a unique $m$-bit code (written as a sequence of -1 and +1)
- transmit chip sequence on 1 bit, and negation of chip sequence on 0 bit
- bandwidth needed is greater by factor of $m$
- chip sequences are pairwise *orthogonal* -> normalized inner product of two chip sequences $S$ and $T$ is 0 
- normalized inner product or any chip sequence with itself is 1 $S\cdot S=1$; $S\cdot \bar S=-1$ 

> [!example]- Chip sequences
> $A=(-1-1-1+1+1-1+1+1)$
> $B=(-1-1+1-1+1+1+1-1)$
> $C=(-1+1-1+1+1+1-1-1)$
> $D=(-1+1-1-1-1-1+1-1)$
> 
> $\begin{array}{ll}
> S_1=C &=(-1+1-1+1+1+1-1-1) \\
> S_2=B+C &= (-2\quad\;0\quad\;0\quad\,0+2+2\quad\,0-2) \\
> S_3=A+\bar B &= (\;\;\;0\quad\,0-2+2\quad\;0-2\quad\,0+2) \\
> S_4=A+B+\bar C+D &=(-2-2\quad\,0-2\quad\;0-2+4\quad\;0)
> \end{array}$
> 
> $S_1\cdot C=[1+1+1+1+1+1+1+1]/8=1$
> $S_2\cdot C=[2+0+0+0+2+2+0+2]/8=1$
> $S_3\cdot C=[0+0+2+2+0-2+0-2]/8=0$
> $S_4\cdot C=[2-2+0-2+0-2-4+0]/8=-1$

- all stations are synchornized -> chip sequences begin at the same instant
- compute normalized inner product of received chip sequence and chip sequence of station
- $$S=A+\bar B + C$$ $$S\cdot C=(A+\bar B + C)\cdot C=A\cdot C+\bar B \cdot C+C\cdot C=0+0+1=1$$

> [!example]- Analogy
> Airport lounge with many pairs of people conversing:
> - **TDM**: pairs of people taking turns speaking
> - **FDM**: pairs of people speaking at different pitches (frequencies)
> - **CDMA**: pairs of people speak in different language -> extract desired signal while rejecting everything else as random noise

### Wavelength Division Multiplexing
- multiple signals are multiplexed onto an optical fiber using different wavelengths of light
- $n$ fibers combined onto single shared fiber and split at receiver end into $n$ fibers
	- output fibers filter all but one wavelength
- ![[Wavelength division multiplexing.png|500]]
- diffraction grating is completely passive and reliable compared to electrical [[#Frequency Division Multiplexing|FDM]]
- bandwidth: 25,000 Ghz on single fiber band -> 2500 10-Gbps channels with 1 bit/Hz data rate
- **Dense WDM**: small spacing between channels increases transmission capacity -> maintain stable wavelengths and frequencies
- interferometers at output filters can be controlled by a computer to change to a different wavelength
# The Public Switched Telephone Network
## Structure of the Telephone System
- telephone owners needed $n$ wires strung up to all $n$ houses they wanted to talk with
- switching office connected to each customer's house -> jumper cable manually connected between two households
- switching offices needed switching offices
- each telephone is connected to the nearest **end office** (1 to 10km away)
- **local loop**: two-wire copper connection to end office 
- end office autmatically connects local loops for entire call
- end offices are connected to **toll offices** with **toll connecting trunks**
- toll offices connected via high-bandwidth **intertoll trunks**
- analog system: voice signal transmitted as an electrical voltage
- local loops are essential and simultaniously the weakest link
## The Local Loop: Telephone Modems, ADSL, and Fiber
### Telephone Modems
- old local loops transport digital data from customer and end office into the Internet -> relatively narrow bandwidth, attenuation and distortion of signals (electrical noise)
- digital bits must be converted to analog signals through **modem** ($mo$dulator $dem$odulator)
	- cable or DSL modem: hardware between phsical line coming in and rest of the network
- telephone modems are limited to 3100 Hz from the telephone line
- modem sents at rate of 2400 symbols/sec (2400 baud)
	- use 0 volts for logical 0 and 1 volt for logical 1 -> 1 bit per symbol -> 2400-bps modem
	- use four different symbols -> 4800 bps
- Shannon limit of telephone system is about 35 kpbs based on average quality (two local loops)
- ISPs have high-quality signal -> 70 kbps
- telephone channel is 4000 Hz wide with guard bits
- number of samples per second to reconstruct it is 8000
- North America uses 8 samples per seconds with one for control -> 56 kbps
### Digital Subscriber Lines (DSL)
- filter cuts out frequencies below 300 Hz and above 3400 Hz (gradual -> 4000 Hz)
- new customers don't have filter -> 1 MHz physical restraint instead of 3100 Hz artificial bandwidth through filter
- 1.1-Mhz spectrum split into 256 channels of 4312.5 Hz
- **DMT** (**Discrete Multitone)**: ![[Operation of ADSL using discrete multitone modulation.png|500]]
	- Channel 0 used for **Plain Old Telephone Service**
	- Channels 1-5 used for separation between voice and data signals
	- 2 channels used for upstream (user to ISP) and downstream (ISP to user) control
	- 248 channels for user data
- 80-90% bandwidth for downstream (more downloads than uploads) -> *Asymmetrical* DSL (**ADSL**)
- **G.dmt** standard supported speeds of 8 Mpbs downstream and 1 Mbps upstream
- **VDSL** pushed data rate over 52 Mbps downstream and 3 Mbps upstream
- ![[Typical ADSL equipment configuration.png|500]]
	- **NID** (**Network Interface Device**): marks end of telephone company's property and start of customer property
	- **splitter**: analog filter separating POTS from data
	- **Digital Subscriber Line Access Multiplexer**: converts signal to bits and sends packets to the ISPs data network
	- deployment easy -> buy DSLAM and splitter and attach to ADSL subscribers splitter
- **bonding**: use two or more physical DSL connections
### Fiber To The X (FTTX)
- deploy fiber as far they can go to the home -> less constrained by slow copper cables over long distances
- **Fiber to the Home**: directly to home itself
- **Fiber to the Node** (Neighborhood): fiber terminated in cabinet on street miles away from customer home
- **Fiber to the Distribution Point**: bring fiber to distribution point of several hundred subscribers meters away from premises
- **Fiber to the Curb**: Between FTTN and FTTdp
- **PON** (**Passive Optical Network**): upstream data merges signals from everyone into one, downstream data divided by optical splitter to all houses (encryption used aswell)
- **Gigabit-capable PON**s provide 2.4 Gbps downstream and 1.2 or 2.4 Gbps upstream
- equiptment requests and is granted time slots by end office
## Trunks and Multiplexing
- end office converts to digital form for transmission over longhaul trunks
### Digitizing Voice Signals
- 12 calls (60 kHz-to-108 kHz band) are one **group** -> five groups are **supergroup** 
- FDM is used over copper wires, TDM can be handled by digital electronics
- **codec** (*co*der-*dec*oder): digitizes analog signals using **Pulse Code Modulation** (**PCM**)
	- 8000 samples per second (enough for 4-kHz telephone channel bandwidth (Nyquist))
	- uncompressed data rate for telephone call: 8 bits every 125 $\micro$sec (64 kbps)
- samples of amplitude are quantized to an 8-bit number
- errors are proportional to signal amplitudes (amount of bits)
### T-Carrier: Multiplexing Digital Signals on the Phone Network
- **T-Carrier**: specification for transmitting multiple TDM channels over a single circuit
- ![[T1 carrier (1.544 Mbps).png|500]]
	- 24 voice channels multiplexed together
	- each channels inserts 8 bits into the output stream
	- 193 bits every 125 $\micro$sec
## Switching
- ![[Circuit and Packet switching.png|500]]
- [[Comparison of circuit-switched and packet-switched networks]]
### Circuit Switching
- telephone system switches path from telephone to receiver's telephone for enter call duration
- **Strowger gear**: circuit-switching equiptment
- hunting of path can take a couple seconds
- once a call has been set up, a dedicated path between both ends exists and will continue to exist until the call is finished
- set up an end-to-end path *before* any data can be sent
- data cannot arrive out of order
- congestion occurs at the setup time
- bandwidth wasted when there is no traffic for a user
- if a switch goes dow, all of the circuits using it are terminted and no more traffic can be sent
### Packet Switching
- packets sent as soon as they are available
- no need to set up dedicated path in advance
- routers use store-and-forward transmission to send packet on its own
- packets may have to wait to be forwarded -> *queuing delay*
- congestion occurs when packets are sent
- packets can be routed around dead switches
 
---
References: Computer-Networks-Global-Edition