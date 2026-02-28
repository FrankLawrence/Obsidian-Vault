---
Tags: #lecture 
Created: 2025-10-02 13:37:30
---
(Links:: [[System Security]])

Side-Channel attacks exploit **physical or behavioral leakages** during execution, which can beat system that are implemented correctly.

|                                   | Examples                                | Type    | Target                        |
| --------------------------------- | --------------------------------------- | ------- | ----------------------------- |
| Classic Side-channels attacks     | Timing, Power, EM, Acoustic             | Passive | Crytpographic implementations |
| Microarchitectural SCAs           | Cache timing, [[Spectre]], [[Meltdown]] | Passive | CPU internal resources        |
| Fault Injection (physical)        | Laser glitches, voltage glitches        | Active  | Crypto chips, smartcards      |
| Fault/Disturbance (Architectural) | [[Rowhammer]], Plundervolt, CLKScrew    | Active  | Memory/CPU integrity          |

- Emission Security is a classic SCA which takes advantage of physical signals activated by some digital activity.
- The aim often is to bring the complexity of a problem (ex. from exponential) to linear time.

> [!example] 
> Guessing a password is exponential based on the number of digits. If a correct digit influences the system to give of a different signal (such as taking more time or power), we can guess the password each digit at a time -> linear

## SCA classification by Leakage Source
- **Timing Attacks**: Measure how long operations take
- **Power Analysis Attacks**: Measure fluctuations in power consumption
	- *Simple power analysis (SPA)*: Directly interpret power traces
	- *Differential power analysis (DPA)*: Use statistical analysis over many traces
- Electromagnetic (EM) Attacks: Capture EM radiation emitted by devices
- Acoustic/Optical Attacks: Use sound (keyboard clicks, CPU noise) or optical emanations (LEDs, reflections)
- Cache/Memory Attacks: Exploit shared hardware resources (Flush+Reload, Prime+Probe)
- Thermal/Photonic Attacks: Exploit heat or light leakages

## SCA classification by Technique
- **Passive Attacks**: Only observe leakage
- **Active Attacks(Fault Injection)**: Intentionally disturb computation using voltage glitches, lasers, or clock manipulations to induce errors and gain information.
- **Simple Analysis**: Directly interpret raw measurements (SPA, simple timing)
- **Differential/Statistical Analysis**: Use large sets of measurements and statistical techniques (DPA, correlation power analysis)
- **Template Attacks**: Build precise models of leakage using profiling and then apply them to targets
- **Machine Learning/Ai-Based attacks**: Use neural networks or classifiers to extract patterns of leakages

| Attack type  | Data Requirement       | Analysis Style          | Strengths                               | Weaknesses               |
| ------------ | ---------------------- | ----------------------- | --------------------------------------- | ------------------------ |
| Simple       | Few traces             | Visual/direct patterns  | Fast, easy                              | Needs very clean leakage |
| Differential | Many traces            | Statistical correlation | Works with noisy data                   | Requires large dataset   |
| Template     | Profiling + few traces | Probabilistic modeling  | Very powerful, few attack traces needed | Needs profiling device   |

# Timing Attack on [[RSA]]
## Background RSA security
To determine how secure an encryption scheme is, you play a hypothetical game between an adversary (a polynomial-time probabilistic Turing machine), and an *all-knowing* oracle:
1. The adversary and oracle both learn of the security parameter $1^{n}$
2. Adversary chooses $m_0$ and $m_1$ such that $\vert m_{0}\vert = \vert m_{1} \vert$
3. Oracle selects $k$ randomly from $\{0,1\}^{n}$, chooses a random $b= 0,1$ and computes $c:= Enc(k,m_{b})$
4. Lastly, the adversary must try and guess $b$ (basically, they should know which message was encrypted)

- Real-life applications require that *multiple messages* be encrypted with the same key -> an adversary can see multiple cypher-plain text pairs 
- Assume the adversary may choose the plain text to encrypt: **Chosen Plaintext Attacker** (CPA)
	- Both parties know security parameter $1^{n}$
	- **Learning phase**: Attacker chooses messages (as many as they like), and oracle encrypts them with random $k \in \{0,1\}^{n}$. 
	- **Challenge phase**: Like before, adversary sends to messages, which they must find which message, the oracle sent back encrypted

> [!definition] Definition encryption security
> We say that $(\text{Enc}, \text{Dec})$ has *indistinguishable encryptions (CPA-secure) under a chosen-plaintext attack*, if every *randomized polynomial time* adversary guesses $b$ with probability at most $0.5 + \varepsilon(n)$, where $\varepsilon$ is negligible.

Usually, we model a system and the attackers, and then determine it's security properties (ex. CPA-secure). Due to *implementation issues* (side channel leakage, fault injection, probing, ...), the assumptions change -> security properties lost.
This is a *fundamental problem*, and these problems must also be modelled, to design countermeasures that determine new properties (ex: secure against side channel attack of type xx).

## Timing analysis of RSA
Breaking RSA requires breaking two hard problems:
- computing the $e$th root of $m \mod n$ to find $m$ from $C = m^{e} \mod n$
- Factoring large numbers into smaller primes

Example implementation:
```
squareAndMultiply(m,d,N):
	x = m
	for j = 1 to length(d)
		x = modSquare(x, N)
		if d_j == 1 then
		   x = modMultiply(x, m, N) 
		end if
	next j
	return x
```

- In software, exponentiation is implemented by squaring and multiplying values
	- Square and multiply are computed modulo n using **Montgomery** multiplication/square

> [!bug] Execution time dependent on key value
> - Due to implementation of square-and-multiplication, code execution branches based on different key values
> - When a bit of key $d$ is 0, a [Montgomery modular multiplication](https://en.wikipedia.org/wiki/Montgomery_modular_multiplication) is skipped, and execution time is faster

- With this we can determine how many 1 bits are in the key ([[Hamming Weight]]) with enough samples of the total execution time
- On average, we know keys will have $n/2$ 1's, (or that multiple messages share the same hamming weight) so knowing the HW won't help much
	- For an attack to work, we need message-dependent timing!
- Montgomery's execution time itself is dependent on 
- For certain values of the plaintext $m$, Montgomery will/won't execute a reduction step
- `modSquare()` execution time is dependent on the reduction step taking place (no reduction leads to faster execution)
	- thus execution time is dependent on values of $m$ (reduction or not) and $d$ (montgomery calculation or not)
- More details in related papers [^4] [^5]
## How to leak the secret
- the attacker will simulate the attack (running the algorithm) on their machine, using random messages, and will also **decrypt the same messages on the victim machine**
- messages are grouped based on the execution speed and the decryption key bit 0 or 1:
	- $M_1$ contains messages where execution was slow and $d_{1} = 1$
	- $M_2$ contains messages where execution was fast and $d_{1} = 1$
	- $M_3$ contains messages where execution was slow and $d_{1} = 0$
	- $M_4$ contains messages where execution was fast and $d_{1} = 0$
- Values in $T_i$ are the execution times for $M_i$ on the victim machine
- Logically, if **a message from one group performs the same on the victim's machine, the secret bit is that from the simulation** otherwise it's the opposite
	- We expect to see that $T_1$ and $T_{2}$ do not have the same execution times (we determine this using distribution graphs), since they are different in the simulation
- Since the iteration stops on the last bit of the key $d$, there is no further square operation; this key must be brute-forced (try both 0 and 1)
## Countermeasures
- Having the algorithm compute in **constant time** 
	- easy to implement for a specific algorithm, but hard to generalize
- Adding noise *would not* help, as attackers would sample more data to filter it out
- Masking each message with a random number $x$ hinders the attacker from guessing if the algorithm is slower/faster
# Power Analysis Side Channel Attacks
- logic gates and memory elements (registers or RAM) are used to implement finite state machines
	- logic gates are composed of [[CMOS]]
	- ==TODO: diagrams==
- Takeaway: Flipping a bit induces a fluctuation in the current which can be measured
	- A bit flip from 0->1 induces a current $i_{sc}+ i_{charge}$
	- A bit flip from 1->0 induces a current $i_{sc}$

> [!summary] Hamming weight and hamming distance
> - Static power is ideally 0
> - 1's don't consume more than 0's
> - Transitions to 1 are on average more expensive than transitions to 0
> - Power proportional to number of bits that at one
> - HammingDistance($Y_{\text{current}}$, $Y_{\text{previous}}$)
> - HammingWeight($Y_{\text{current}}$)

## Lessons learned
- **Data dependency**
	- physical actions create data dependency between logic values and their transitions and the power consumption of the circuit
		- We often tend to confuse 'power' with 'current' because $P=RI^{2}$
	- Current produces electro-magnetic emissions -> can similarly be attacked
- **Measure**
	- We can measure power consumption and observe these phenomena
	- Signals are small, many measurements and statistical analysis are often needed
- **Model**
	- Given some logic data manipulated by the software/hardware, we can 'predict' the corresponding power consumption
	- In this case, the model uses Hamming weights and distance
## Simple Power Analysis on RSA
From [wikipedia](https://en.wikipedia.org/wiki/Power_analysis):
> **Simple power analysis** (**SPA**) is a [side-channel attack](https://en.wikipedia.org/wiki/Side-channel_attack "Side-channel attack") which involves visual examination of graphs of the [current](https://en.wikipedia.org/wiki/Electrical_current "Electrical current") used by a device over time. Variations in power consumption occur as the device performs different operations.

We can observe a single execution to determine the value of $d$
![[Pasted image 20251229005016.png|500]]
## Differential Power Analysis on AES
From [wikipedia](https://en.wikipedia.org/wiki/Power_analysis):
> **Differential power analysis** (**DPA**) is a side-channel attack which involves statistically analyzing power consumption measurements from a [cryptosystem](https://en.wikipedia.org/wiki/Cryptosystem "Cryptosystem"). The attack exploits biases varying power consumption of microprocessors or other hardware while performing operations using secret keys. DPA attacks have _signal processing_ and _error correction_ properties which can extract secrets from measurements which contain too much noise to be analyzed using simple power analysis.

- Assume the following symmetric block cipher:
	- $C = AESEncrypt(P,K)$
	- $P = AESDecrypt(C,K)$
- The goal to breaking AES, is to find the cipher key $K$
- [[AES]] includes a step *SubBytes*: "a non-linear substitution step where each byte is replaced with another according to a lookup table"
	- Attack one byte at a time at the output of the SubBytes operation of the first round
- Attacking one byte:
	- **Plaintext bytes** $(p_{0},p_{2},...,p_{15})$
	- **Key bytes** $(k_{0},k_{2},...,k_{15})$
	- Intermediary bytes $y$ calculated for each byte of plaintext/key : $$y=S_{\text{box}}(p \oplus k)$$

Finding the point of interest (POI):
- We measure the leakage as $l(y)$
- Leakage model: $m(y)=HW[y]$ (we use Hamming weight and hamming distance as our model)
- Take many measurements by varying key byte $k$ and plaintext byte $p$
- Next, *cluster the traces in 9 classes* based on $HW[y]=0 \dots 8$
- Compute the sum of absolute differences:
	- Compute the average trace for each class
	- Graphing the average traces of each class, we see that they diverge at time $t_{POI}$
	- Computing the sum of the absolute difference of average power consumption per each class, we see a maximum at time $t_{POI}$ when the measurements show a data dependency with $y$

Performing differential power analysis for each byte:
- Take many measurements $L$
	- Fixed unknown key byte $k$
	- Vary known plaintext byte $p$
- For $k_{\text{guess}}$ in range 0 to 255:
	- For each measurement $l$:
		- $HW[y]=HW[S_{\text{box}}(p \oplus k_{\text{guess}})]$
		- If $HW[y]<= LowThreshold$ (e.g. 2)
			- Measure power with $p$ as input and put measurement in $M1$
		- else if $HW[y]>=HighThreshold$ (e.g. 6)
			- Measure power with $p$ as input and put measurement in $M2$
	- $Scores[k_{\text{guess}}]=\text{avg\_over\_different\_p}(M2)-\text{avg\_over\_different\_p}(M1)$
- Best guess is most likely key byte: $argmax[Scores]$
==TODO: better explanation using [^1]==

Correlation Power analysis for each byte:
- Take many measurements $L$
	- Fixed unknown key byte $k$
	- Vary known plaintext byte $p$
- For $k_{\text{guess}}$ in range 0 to 255:
	- $Y=[HW[S_{\text{box}} (P \oplus k_{\text{guess}})] \text{for each } p]$
	- $Scores[k_{\text{guess}}] = \text{PearsonCorrelationCoeff}(L,Y)$
- Best guess: $argmax[Scores]$

- Using Pearson Correlation Coefficient shows how well the model is linearly correlated with the measured leakage

## Countermeasures
- Attempt 1: adding noise
	- Adding random noise, or desynchronizing traces
	- Defeated with better signal processing and more measurements
- Attempt 2: balancing hardware
	- Add filters and shielding or design chip to consume the same power for every instruction
- Attempt 3: **nth order masking**
	- Multiply each data byte with a random variable
	- This algorithmically breaks the dependency making it impossible to guess the intermediate value
	- higher order attacks become increasingly harder

> [!example]
> Given $x=x_{1}+x_{2}$ and $y=y_{1}+y_{2}$, compute $z=z_{1}+z_{2}=x+y$ without breaking uniformity, non-completeness or correctness.
> $$z_{1}=x_{1}+y_{1}$$
> $$z_{2}=x_{2}+y_{2}$$
> **Correctness**: $z=z_{1}+z_{2}=x+y$
> **Non-completeness**: $i$ share does not depend on non-$i$ shares
> **Uniformity**: $z_{i}$ is uniform if either $y_{i}$ or $x_{i}$ is uniform

Why is this problem complex?
- Consider 1st order masking scheme
- Mathematical model considers the leakage and prevents 1st order attacks by masking with random values unknown to the attacker
- But...
	- An attacker would need to combine *two leakage points* to cancel the mask. That's much harder, as it requires more traces and computation
	- Some hardware effects might break the countermeasure

This can however still be broken if, for example, two masked values are loaded after each other in a vulnerable/exposed buffer. This allows to compute the hamming distances between these two values. Further countermeasures include:
- [[AES]] encryption, which still is vulnerable to key leaks for every execution of crypto
- Key changes on each iteration prevents many traces of the same key
- Update the key before each usage
	- *Stateful design*: key owner updates key before usage, but symmetric crypto requires synchronization between multiple key owners
	- *Stateless design*: Symmetric applications are easier
- **GGM Construction**
	- Nonce bits decide path
	- Public randomness $R_{i}$
	- One encryption per nonce bit (128 Enc)
	- Final key $K_{\text{nonce}}$ used
	- Great leakage properties: at most two observations per key!
	- Big performance overhead

![[GGM Construction.svg|400]]

- More on leakage detection, profiled correlation attacks and template attacks [^2] [^3]
# TEMPEST
Idea: active signal injection to trigger more emissions using attacker software

1. **Soft-TEMPEST**: Active version of TEMPEST; leakage used to exfiltrate data [^7]
2. **TEMPEST** (Van Eck Phreaking): Passive leakage of plaintext information -> multiple meters distance [^6]
3. **Side Channels**: Use leakage to attack cryptographic implementations, e.g. to recover the key -> only in close proximity

- Video signals travelling over unshielded wires generate *EM waves* (from the current), modulated with the pixel values
	- Tune the radio receiver to $1/T_{\text{pixel}}$, where $T_{\text{pixel}}$ is the length of the signal for data of one pixel
- Soft-TEMPEST allows for exfiltration of data on *air-gapped devices* using EM radiation, created through software executions
	- may also take advantage of other leakage sources (e.g. memory access, noise-[[SDR]]) [^10]
- Todays technology runs with energy at fundamental frequency (100-600-MHz), with better shielding, but still not perfect
- TEMPEST attack vectors: 
	- audio signals (through headphones)
	- reflections in observer with camera [^8]
	- vibrations in nearby objects to monitor audio [^9]
# Tamper Resilience
| Tamper classification                                                                                                | Description                                                                                                                                                          | **Purpose**                                                                   | Example                               |
| -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------- |
| *Tamper resistant* systems take the bank vault approach                                                              | Thick steel or other robust materials are utilized to *slow down an attack* (e.g. ATM machine) -> requires tools and great effort to breach system                   | Prevention of break-in                                                        | ATM machine                           |
| *Tamper responding* systems use the burglar alarm approach                                                           | Intrusion detection followed by response to protect asset; does not depend on robust construction -> primary use in portable systems or where size is a disadvantage | Real-time detection of intrusion (and prevention of access to sensitive data) | Sound alarm -> erasure of secret data |
| *Tamper evident* systems are designed to ensure that if a break-in occurs, *evidence of the break-in is left behind* | Uses chemical or mechanical solutions; sensitive to small penetrations                                                                                               | Detection of intrusion                                                        | Tamper proof tape/stickers            |

> [!example]- Specialized Devices
> - bank cards
> - GSM/UMTS SIM cards
> - electronic tickets for mass transport systems
> - payTV applications
> - access control to buildings
> - electronic ID cards, e-passports

## Commodity Solutions
- PIN + card possession enable user authentication: ATM
- Card holds a key: [[YubiKey]]

---
> [!summary]
> Side Channels
> - **Definition**: Exploiting unintended leakages (timing, power, EM, acoustics, cache, etc.), not breaking cryptographic math
> - **Types of Side Channels**:
> 	- *Timing attacks*: (RSA timing, key-dependent execution)
> 	- *Power analysis*: Simple Power Analysis - SPA, Differential Power Analysis - DPA, Correlation Power Analysis - CPA
> 	- *Electromagnetic & Acoustic*: TEMPEST, van Eck phreaking, glowworm attacks
> 	- *Fault Injection*: Rowhammer, voltage/laser glitches
> - **Analysis methods**: Simple, statistical, template, ML-based
> - **Countermeasures**: constant-time algorithms, masking, noise, balancing hardware
> 
> Security of Cryptographic Algorithms
> - *Models matter* - CPA (Chosen-Plaintext Attack) security as baseline
> - *Implementations break assumptions* -> side channels/faults leak info
> - *RSA timing attack*: Square-and-Multiply leaks key bits due to conditional operations and Montgomery reduction
> - *AES power analysis*: Recover key byte by byte using statistial correlation with measured power traces
> 
> TEMPEST/Emission Security
> - *TEMPEST (van Eck Phreaking)*: leaking video/screen signals via EM radiation
> - *Soft-TEMPEST*: Software-modulated emissions to exfiltrate data
> - Other attack vectors: reflections, vibrations (Lamphone), LEDs (Glowworm)
> - Modern hardware (HDMI, DisplayPort, differential signaling) reduces but does not eliminate leakage
> 
> Tamper Resilience
> - **Tamper classification**
> 	- *Tamper-resistant* (bank vault; strong casing)
> 	- *Tamper-responding* (alarm; detect + erase keys)
> 	- *Tamper-evident* (seals, paint, brittle covers)
> - *Devices*: Smartcards, cryptoprocessors, Hardware Security Modules (HSM)
> - FIPS 140-2 as a certification standard

> [!question] What differences between analysis techniques can you observe? For instance, how many traces does each roughly require to produce a correct result? Which one would you consider more robust?
> The CPA attack is computationally more expensive but requires fewer measurements and tuning of parameters to succeed. DPA requires more measurements, as not all mea- surements are used to compute scores (the ones with a hamming weight outside of the thresholds do not influence the guess score). Furthermore, the hamming weight thresholds must be carefully chosen, making the analysis less robust.
> 
> It might seem that the longer runtime makes CPA the less attractive choice. However, consider that you were supplied with measurements already. The effort of obtaining the measurements required for DPA might well offset the longer runtime of CPA

[^1]: [[Power analysis attacks on the AES-128 S-box using differential power analysis DPA and correlation power analysis CPA .pdf]]

[^2]: François Durvaux and François-Xavier Standaert, “From Improved Leakage Detection to the Detection of Points of Interests in Leakage Traces,” in Advances in Cryptology - EUROCRYPT 2016 - 35th Annual International Conference on the Theory and Applications of Cryptographic Techniques, Vienna, Austria, May 8-12, 2016, Proceedings, Part I, ed. Marc Fischlin and Jean-Sébastien Coron, vol. 9665, Lecture Notes in Computer Science (Springer, 2016), 240–62, https://doi.org/10.1007/978-3-662-49890-3_10.

[^3]: Suresh Chari, Josyula R. Rao, and Pankaj Rohatgi, “Template Attacks,” in Cryptographic Hardware and Embedded Systems - CHES 2002, 4th International Workshop, Redwood Shores, CA, USA, August 13-15, 2002, Revised Papers, ed. Burton S. Kaliski Jr, Çetin Kaya Koç, and Christof Paar, vol. 2523, Lecture Notes in Computer Science (Springer, 2002), 13–28, https://doi.org/10.1007/3-540-36400-5_3.

[^4]: Jean-François Dhem et al., “A Practical Implementation of the Timing Attack,” in Smart Card Research and Applications, ed. Jean-Jacques Quisquater and Bruce Schneier, Lecture Notes in Computer Science (Berlin, Heidelberg: Springer, 2000), 167–82, https://doi.org/10.1007/10721064_15

[^5]: Paul C. Kocher, “Timing Attacks on Implementations of Diffie-Hellman, RSA, DSS, and Other Systems,” in Advances in Cryptology - CRYPTO ’96, 16th Annual International Cryptology Conference, Santa Barbara, California, USA, August 18-22, 1996, Proceedings, ed. Neal Koblitz, vol. 1109, Lecture Notes in Computer Science (Springer, 1996), 104–13, https://doi.org/10.1007/3-540-68697-5_9

[^6]: “TEMPEST: A Signal Problem” (NSA, 1972).  
	W. van Eck, “Electromagnetic Radiation from Video Display Units: An Eavesdropping Risk?,”Comput. Secur. 4, no. 4 (1985).

[^7]: M. G. Kuhn and R. J. Anderson, “Soft Tempest: Hidden Data Transmission Using Electromagnetic Emanations,” in Information Hiding (1998).

[^8]: Michael Backes, Markus Dürmuth, and Dominique Unruh. Compromising Reflections - or - How to Read LCD Monitors Around the Corner. In Proceedings of the IEEE Symposium on Security and Privacy (SSP '08), Oakland, CA, May 2008.

[^9]: Ben Nassi et al., “Lamphone: Real-Time Passive Sound Recovery from Light Bulb Vibrations,” IACR Cryptol. EPrint Arch., 2020

[^10]: G. Camurati and A. Francillon, "Noise-SDR: Arbitrary Modulation of Electromagnetic Noise from Unprivileged Software and Its Impact on Emission Security" to appear at IEEE S&P 2022.
