---
Tags: lecture
Created: 2026-06-15 23:28:09
---
(Links:: [[Advanced Computer Architecture]])
# Physical Covert-Channels and Side-Channel Attacks
Side-channel attacks are **most worrying for embedded devices** where physical access is possible, such as smart cards or [[IoT]] devices. In the case of *untrusted datacenters*, physical access is to be assumed possible ([[DRAM]] encryption and integrity problem discussed before). Sometimes however, physical presence is not even needed, such is the case with [[FPGA]]s in [[SoC]]s and in datacenters.

> [!definition] Invasive vs. Noninvase
> Invasive attacks usually involve disassembling the victim device, creating new connections (using laser cutters, probing stations, etc. ) and tampering with layers. This however is very expensive and only applies to large threat models.
> Noninvasive attacks on the other-hand only attack through an exisiting interface, leaving no traces, and usually using inexpensive equipment.

![[Side Channel Attacks#SCA classification by Technique]]

![[Side Channel Attacks#SCA classification by Leakage Source]]

> [!definition] Hardware Trojan
> - Malicious stealthy circuit, unintended part of an otherwise genuine computer
> 	- Added through an IP/component purchased from a disreputable source
> 	- Added by modification of the manufacturing data by a disreputable foundry
> 	- **Supply-chain attacks**: large-scale global outsourcing in design and manufacturing
> - Typically inactive most of the time, until it receives some activation signal and then performs some rogue action
> 	- **Activation** may be a thinned wire which creates an open after accelerated aging, an FSM brought into a hidden state, a sensor receiving a particular physical signal, etc.
> 	- **Action** could be a damage to the computer, a critical change in the specification, a corruption of the data being processed, the leakage of secrets through a covert channel, etc.

# Differential Power Analysis
The basic idea is to find out *statistical correlation between hypotheses on the secret and actual measurements* instead of trying to infer the secret from looking at the traces and interpreting them ad-hoc. Almost only one assumption is made: what is the cryptographic algorithm being run.
The first step involves collecting *many measurements to cancel noise* (thermal noise, environment noise, data-independent consumption, consumption variations due to non0secret data, etc.). By
1. Recording traces with very many different plain-texts, and...
	1. It is assumed we can trigger or observer many encryptions with known plaintext, for instance
	2. Traces will be very different and *will tell nothing individually*
2. Making a hypothesis on the key (or part of it) 

we can then *compute some internal signal* (decision or selection function) as a function of the plaintext and of the guess. Use the *divide-and-conquer* technique to choose a decision function which depends only on a few bits of the key, so that we have less guesses to make.
By splitting the measurements into *two groups based on decision function*, we can make a clear distinction:

If the *guess is wrong*, the decision function computed is uncorrelated with the real internal signal and thus with any point of the power traces -> the *average of the two sets will be identical* (for a sufficiently large number of measurements). 
If the *guess is right*, the decision function is equal to a real internal signal and thus correlated to something in the power traces (part of the consumption in some instants) -> the *average of the two sets will be different in those instants* and the difference will correspond to the effect of the internal and thus nonnull. 

![[Pasted image 20260616001617.png|700]]

- A typical decision function for DES could be $$d=MSB(SBOX_{0...3}(K_{0..5} \oplus P_{0..5}))$$ where
	- $K_{0..5}$ is a sub-key that derives directly and reversibly from key $K$
	- $P_{0..5}$ is obtained from the known plaintext
	- $SBOX_{0..3}()$ is the 4-bit substitution box function
	- $MSB()$ indicates the most significant bit
- **S-Box substitution** is a common component of most cryptographic algorithms, where input values are translated through a table, in order to obscure the relationship between key and ciphertext
- The decision function is powerful, because every *successful attack results in disclosing 6 bits of the key* at once

![[Differential Traces of DES.png|600]]

We can adapt the decision function to *attack multiple bits at once* by instead using the multivalued decision: $$D_{0..3}=SBOX_{0..3}(K_{0..5} \oplus P_{0..5})$$
The idea is that, with the same hypotheses on the key, one can improve the signal to noise ratio by *increasing the number of known bits* influencing power consumption (more signal) and *decreasing those unknown* and thus contributing to the noise (less noise). This is morderately effective but not really efficient for DPA; very efficient for a Correlation Power Attack.

So far we have been using a simple leakage model (hamming weight & distance), as we assume that there is *correlation between the value* of one signal (0 or 1) and *the power consumption* ($d$ has been used to split traces into buckets).
A more faithful representation acknowledges that the CMOS *transitions* consume power, and the decision function should instead be the change in a particular signal during the computation.
With more information on the implementation and the technology, one could develop more precise models (but it is *generally unnecessary* for a successful attack).

To analyse the traces we only pick *relevant information using simple power analysis* or some statistical processing: Identify where a particular operation takes place and use as a measurement only the maximum power consumption during the execution of the program.

## Correlation Power Attacks
- Use a linear correlation factor instead of differences -> improves the statistical quality of the decision
- **DPA splits measurements in bins based on the decision function** and checks if the averages of the measurements in the bins differ significantly
- **CPA measures the linear correlation factor** between the **leakage** model (applied to the bits of the decision function) and the **measurements**
	- If the correlation factor is 0 when the number of measurements grows, the key guess is wrong

There are many more improvements possible that allow for more powerful attacks (most protections can be circumvented at a cost):
- Improve measurements and preprocess traces
- Adaptively select relevant samples
- Building better statistics, such as higher-order attacks (combining instantaneous power consumption at different times), profiling or template attacks (building knowledge on the device leakage prior to the attack), etc.
- Better statistical tests

# Active Side-Channel Attacks (Fault Injection)
> [!definition]
> - Semi-invasive attacks which use some mean of **creating a disturbance** on a device to get it **to reveal secrets**
> - Somehow attacks based on [[Rowhammer]] are fault attacks, but more typical fault injection attacks use physical means to create malfunctions
> 	- Disturbance to the clock signal or to the power supply
> - Usually require a detailed knowledge of the device
> 	- Which, when and where to attack?
> 	- What is the probable effect of the attack?
> - Secrets are often revealed by *comparing runs with and without faults*
> 	- In trivial ways, e.g. by forcing to 0 or 1 a bit of the key and comparing output
> 	- In very subtle ways, exploiting the structure of the cryptographic algorithms

Some fault injection methods include:
- Varying the **supply voltage** to create a critical path delay, exceeding the clock period
- Varying the **clock frequency** to create clock period too short for the critical path
- **Heating** the device to affect the critical path
- Opening the VLSI circuit and **shining light** to flip bits in registers

Fault injection attacks are relatively rare, but harder to protect against. Countermeasure techniques are similar to methods used in *fault-tolerant computing*:
- Replication: Repeat computation multiple times and compare (expensive)
- Error detection: Parity checks, [[ECC]] and shadow registers (cheaper but easier to fool)
# Remote Side-Channel and Fault Attacks
Side-channel attacks are usually assumed to require physical presence in order to measure power consumption etc., however datacenters now make use of [[FPGA]]s, for which an attacker can program their own circuitry directly.

> [!question] How can an attacker measure power?
> - Supply voltage on transistors depends on the current absorbed, because of the finite resistance of the power supply and of the resistance of the power distribution network (cabling) 
> $$\Delta v(t) \sim -i (t)$$
> - Combinational delay of gates is approximately inversely proportional to supply voltage
>   $$\delta \sim \frac{1}{v(t)}$$
> 
> Thus the attacker only needs to **measure the delay**!

An attacker can program an FPGA into a delay-line sensor: Insert a transition in a series of inverters and see, within a fixed time, how far the transition manages to "travel". 

![[Delay-Line Sensor.svg|400]]

The smaller the output, the larger the instantaneous power consumed (on-chip digital oscilloscope).

Yet another approach involves creating a ring oscillator (odd number of inverters in a ring) and measuring the frequency by counting transitions and comparing to the transitions in a reference clock.

![[Ring Oscillator Sensors.svg|600]]

Again, there are plenty of practical issues to handle properly, but essentially one gets an on-chip digital "oscilloscope". 
Additionally, if drawing current lowers the supply voltage (and thus slows the logic circuits connected), drawing a lot of current may create **faults** in neighbouring circuits! This can be used to force true random number generators to be biased!
# Countermeasures to Physical Side-Channel Attacks
There are generally two approaches to fighting physical side-channel attacks: You either hide the physical traces left behind better, or change the algorithm to leak less data. 
Hiding the data follows a more engineering approach, where the same computation is performed, but we *reduce the correlation* between physical emanations and the secret.
Algorithm masking is a mathematical approach where we *randomize intermediate values* computed in the device.
## Physical hiding
There are three basic ideas: 
1. make the power consumption *random*
2. make the power consumption *constant*, or
3. increase the noise (*additional power constant*)

They can be applied at *many levels* (but some can only make power consumption random or constant): Software, architectural level hardware, logic gate level hardware, circuit-transistor level hardware.

Most algorithms can be written in different ways, and by using a *random instruction sequence*, it becomes harder to identify what trace is linked to what instruction. We add random dummy operations in random number during the computation or shuffle the order of operations while preserving semantic equivalence.
We could also *choose among equivalent implementations* of elementary operations, but this is *algorithm specific*. Additionally, these approaches need *good random numbers*. They are mostly useful against SPA or simple forms of [[#Differential Power Analysis|DPA]].

Adding dummy operations (such as NOP instructions) worsens the visibility of the correlation, but *does not fully disappear*, and attacks can be improved to mitigate this.
The *instruction shuffler* approach prepares the code to know *which instructions can be safely shuffled* and have a shuffler unit **supply instructions in random order** to the processor. The processor has no clue that instructions are being shuffled and the shuffler knows nothing of the semantic (the algorithm) of the instructions it is shuffling.

![[Instruction Shuffler.svg|300]]

The dependency analysis can be done at *compile time* where mutually independent blocks of dependent instructions are identified, which can be rearranged arbitrarily. A `shuffle` instructions indicates the size and amount of blocks that will follow for the shuffler unit to move at runtime.

### Randomising the Clock
By adding *randomly jittered clocks* to the chip design, finding a correlation becomes even harder, as it is harder to distinguish if what has changed was based on a slight shift in the registers circuit, or from the runtime data. The side-effect is that the critical path has to have "space" for the worst-case jitter (i.e. the circuit is *slower*).

![[Pasted image 20260617171739.png|600]]

Jitter exists naturally, so *electronic design tools* already support creating design that account for this variation. Jitter introduced in this way is *on a different scale*–but qualitatively it is the same. The impact is that performance worsens; targeted attacks may still break it!

![[Pasted image 20260617172635.png|600]]

### Data-Independent Power Consumption
The leakage model relies on transitions within the CMOS to tell apart a 0 and 1 data bit. At the logic-gate level we can use a **dual rail** system for communication ($x$ and $\bar x$), so a transition from 0->1 and 1->0 consumes the same amount of power. By adding a **precharge** to 0 or 1 we have a constant number of switching events, even when no logic transition takes place (0->0 or 1->1).

![[Data-Independent Power Consumption.svg|200]]

Note that to create the *complementary circuits*, the parasitic capacitances of the opposite circuit must be as close as possible which creates larger circuits and more energy consumption.
### Power-Gated MOS Current Mode Logic
At the transistor level, the same idea applies, but instead we use current-mode *differential logic*. The problem this brings is an increase in energy consumption due to *static power consumption*; an easy fix uses a gating transistor to briefly turn the circuit on.
## Algorithm masking
Attacks are based on the correlation between an attacker hypothesis and the corresponding value of an internal signal (responsible for a physical emanation). By changing the computation of the cryptographic algorithm to **make all internal signals** random, without changing the computation's result, the hope is to obscure the correlation:
- **Apply a mask to the inputs** $p'_{i} = f(p, m_{i})$ where $m_i$ is a random variable generated internally, $f$ an appropriate function, and $p$ may represent the plaintext and/or the key.
- Internal signals are now $s' = g(s, m_{0}, ..., m_{n})$ and, due to the random masks $m_{0},\dots, m_{n}$ **cannot be computed, for a given hypothesis, by the attacker**
- **Remove masks from the outputs** $c_{j}=h(c_{j}', m_{0},\dots, m_{n})$ where $h$ is an appropriate function

The challenge is to keep track of how the masks propagate through the algorithm and to be able to "remove" them from the final result.

1. Suppose that a cryptographic algorithm is linear (which of course is not— or at least not completely):
   $$F(x\oplus m, k) = F(x,k) \oplus F(m, k)$$
2. One could implement *boolean masking*: 
	- Mask input with random mask $m$: $x'=x\oplus m$
	- Compute $F(x',k)$ which cannot be attacked because $x'$ is unknown
	- Compute $F(m,k)$ which cannot be attacked because $m$ is unknown
	- Produce the output $F(x,k)$ as $F(x', k) \oplus F(m,k)$
3. For linear operations make sure that the *mask does not cancel out*:
	- Suppose that $a'=a\oplus m, b' = b\oplus m$ and the algorithm computes $x = a' \oplus b'$
	- Then $x = a' \oplus b' = a \oplus m \oplus b \oplus m = a \oplus b$ is *not masked* and leaks information
	- One should have used *two masks* $a'' = a \oplus m_{1}, b'' = b \oplus m_{2}$
	- Then $x = a'' \oplus b'' = a \oplus b \oplus m_{1} \oplus m_{2}$ which is still masked
4. [[AES]] S-Box is nonlinear and requires either 
	- pre-computed *masked tables* $T_{m}(x \oplus m) = T(x) \oplus m$ or 
	- implementation through the multiplicative inverse of a finite-field element since *arithmetic masking* is possible: $(a\times m)^{-1} = a^{-1}\times m^{-1}$

> [!warning] Second-order [[#Correlation Power Attacks|correlation attack]] on masked AES
> 1. Choose two internal values $u$ and $v$ protected as $u_{m} = u \oplus m$ and $v_{m}=v\oplus m$
> 2. Extracting the power samples $p(u_{m})$ and $p(v_{m})$ of the traces correlated to $HW(u_{m})$ and $HW(v_{m})$, where $HW()$ is the Hamming weight, is useless for DPA, because we do not know $u_{m}$ and $v_{m}$
> 3. Instead, we attack an hypothetical internal variable $w_{m}=u_{m}\oplus v_{m}=u \oplus v$ which is unprotected
> 4. Since $w_{m}$ does not exist, no point of the trace is directly correlated to $HW(w_{m})$
> 5. Yet, one can show that $|HW(a)-HW(b)|$ correlates well to $HW(a\oplus b)$
> 6. We use this to manufacture an artificial power sample $p(w_{m})=|p(u_{m})-p(v_{m})|$ from the samples $p(u_{m})$ and $p(v_{m})$ targeting $u_{m}$ and $v_{m}$
> 7. Samples $p(w_{m})$ can be used to mount an attack on $w_{m}=u_{m}\oplus v_{m}=u\oplus v$ which we can compute

One can implement **$N^{th}$-order masking** where each intermediate value is masked with **$N$ random variables**, but **$(N+1)^{th}$-order DPA attacks** are theoretically effective against **$N^{th}$-order masking**

> [!summary] Conclusion
> - Physical side-channel attacks are probably the **most elusive form of security threat** for cryptographic devices
> - There is a **variety of countermeasure** (both from a mathematical and from an engineering perspective)
> - Countermeasures are all **quite expensive** and none removes the possibility of an attack, they *only mitigate the security threats*
> - It looks like in practice what works best is the *implementation of many simple countermeasures* at ounce
> - Today, side-channel attacks are a *key threat* to some particular embedded products (e.g. *smart-cards*) and not yet of classic computing systems (e.g. datacenters, laptops, smartphones), but this may change


---
References:
- [[3. Hardware Security -- Physical Side-Channel Attacks.pdf]]
- [[Power analysis attacks on the AES-128 S-box using differential power analysis DPA and correlation power analysis CPA .pdf]]