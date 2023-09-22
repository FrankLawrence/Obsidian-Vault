---
tags: 
Created: 2023-09-21 23:00:00
Links: "[[Pervasive Computing]]"
---
# Frequency Spectrum
An audio signal is usually not sinusoidal and has not just one frequency but a mixture of more frequencies. We must use the **Fourier analysis** to find the sinusoidal waves that make up the sound wave.
- The component frequencies are multiple of a **fundamental frequency** $$f_{0}=\frac{1}{T}$$
- They are called **harmonics** $$f_{k}=k\times f_{0}\quad k=1\dots N$$

The Fourier series coefficient for $k=0$, $a_0$ is called **average value** of the signal $x(t)$ or **DC component**. We can calculate the amplitudes using **Fast Fourier Transform**. If we have $N$ samples sampled at $Fs$ and we apply FFT, we obtain a vector with $\frac{N}{2+1}$ positive bins. Each bin collects the energy from a small range of frequencies in the original signal.

## Calculating the FFT of a sinusoidal signal in Matlab
```matlab
f=10;  
Td=1/f;  
N=1024;  
Ts=Td/N;  
t=0:Ts:Td;  
s=sin(2*pi*f*t); • >> plot(t,s);
```
```matlab
Fs=1/Ts;  
f=fft(s,N);  
x=(0:(N-1))*Fs/N; >> m=abs(f);  
x=x(1:10);  
m=m(1:10);  
plot(x,m);  
stem(x,m);
```
# Spectrograms
If the frequency spectrum varies in time, a graph is used where the $x$-axis is **time**, the $y$-axis is **frequency** and the color is the **amplitude** of each frequency. 
![[Sound wave Spectrogram.jpg|400]]
# Digital filtering techniques
Some digital signals contain noise that must be removed.
- Moving average filter (time domain): $$y[i]=\frac{1}{M}\sum\limits_{j=0}^{M-1}x[i+j]$$
  e.g. $x[10]=\frac{1}{5}\Big(x[10]+x[11]+x[12]+x[13]+x[14]\Big)$

```matlab
windowSize = 5;
b = (1/windowSize)*ones(1,windowSize);
a = 1;
z = filter(b,a,y);
```
- Filtering in frequency domain: 
	- Low-pass filter: Only low frequencies allowed
	- High-pass filter: Only high frequencies allowed
	- Band-pass filter: Space between low and high frequencies
	- Band-reject filter: Opposite of Band-pass (All but certain frequency range)
- Notch filter principle:
	1. Sample the signal
	2. Compute its spectrum using FFT
	3. Set noise portions to zero
	4. Use inverse FFT to synthesize improved signalA
# Conclusion
- We discussed different techniques to obtain frequency information from a sound signal
- For a frequency analysis, it is easier to represent the signal in frequency domain
- FFT is a tool to convert a signal from time domain in to frequency domain
- The result is called frequency spectrum
- Other useful representation: spectrograms
- Digital Filtering
- There are many ways to visualize an one- dimensional signal such as sound
	1. Plot its waveform in time (but the context information is hidden)
	2. Plot its frequency spectrum (you will see more about its frequency composition)
	3. Plot a spectrogram if the signal is too long and its spectrum varies in time

---
References: