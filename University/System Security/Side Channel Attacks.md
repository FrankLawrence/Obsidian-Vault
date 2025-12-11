---
Tags: 
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
- On average, we know keys will have n/2 1's, (or that multiple messages share the same hamming weight) so knowing the HW won't help much
	- For an attack to work, we need message-dependent timing!
- Montgomery's execution time itself is dependent on 
- For certain values of the plaintext $m$, Montgomery will/won't execute a reduction step
- `modSquare()` execution time is dependent on the reduction step taking place (no reduction leads to faster execution)
	- thus execution time is dependent on values of $m$ (reduction or not) and $d$ (montgomery calculation or not)
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
- 

---
References: