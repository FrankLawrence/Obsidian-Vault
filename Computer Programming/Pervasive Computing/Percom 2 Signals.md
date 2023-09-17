---
Tags: 
Created: 2023-09-06 22:28:32
---
(Links:: [[Pervasive Computing]])
# Periodic Signals
- A **periodic** signal completes a pattern within a measurable time frame, called a period and repeats that pattern over identical subsequent periods
- **Frequency** is the number of oscillations per second
- A **Sinusoidal** signal is a particular case of a periodic signal which can be described by $$s(x) = s_0 A\times \sin(2\pi ft+\phi)$$
  $A$: Amplitude
  $T$: signal Period
  $f$: signal Frequency
  $s_0$: DC component/offset
  $\phi$: Phase
# Signal examples
- The speech signal is an example of a **one-dimensional continuous time signal**
- Digitization: Real analog signals must first be converted (sampling and quantization) with an **ADC** to digital signals for computers to process them
- Analog to Digital Converter
	- **Vin**: input analog voltage
	- Vref: an analog *reference* voltage against which the analog input is compared
- Digital to Analog Conversion (DAC) needed to control actuators
# Digital Signals
- Sample continuous signal at specific intervals -> $Ts$: **sampling period**
- A discrete signal is represented by a sequence of samples $s[n]$, where $n$ is the integer index in the sequence
- **Sampling frequency**: number of samples per second ($fs=1/Ts$)
- Shannon/Nyquist sampling thoerem
	- A continuous signal $x(t)$ with frequencies no higher than $f_{max}$ can be reconstructed exactly from its samples $x[n]=x(nTs)$, if the samples are taken **at a rate $fs=1/Ts$ that is greater than $2 f_{max}$**
	- If the sample rate is too low, we are faced with **aliasing** (we lose detail and information)

> [!important]
> Minimal sampling rate must be greater than 2x highest frequency contained in the signal
# Conclusion
- Signals can be represented by mathematical functions.
- A particular type of signals, often found in nature and easy to analyze are the **sinusoidal signals**.
- A signal from the environment, in order to be processed by a digital computer, needs to be converted into a digital signal, through digitization = **sampling + quantization**.
- Sampling has to be done with a frequency > 2x signal frequency. Undersampling causes aliasing
# Questions
![[Percom Signal.jpg]]
> [!question] a) Determine its DC component, amplitude, period and frequency. Show how you calculate the frequency. [5p]

> [!question] b) Suggest a reasonable sampling frequency for this signal. Justify your answer. [5p]

> [!question] c) How many samples will be obtained when the signal is digitized with the frequency you suggested in b). [5p]

> [!question] d) Explain what will happen if one uses a sampling frequency that is much higher than the frequency suggested in b). Also, explain what will happen if one uses a sampling frequency that is much lower than the frequency suggested in b). [5p]

---
References: