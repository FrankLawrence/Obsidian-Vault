---
Tags: 
Created: 2023-09-20 11:57:01
---
(Links:: [[Pervasive Computing]])
- PE1. What is an image histogram? Sketch an example.
	- An image histogram is a plot of the relative frequency of occurence of each of the permitted pixel values in the image against the values themselves
- PE2. What are morphological operations? Give two examples and explain their effect. 
- PE3. What is the frequency of a sinusoidal signal with a period of 5 sec?  
- PE4. Sketch the frequency spectrum of :
	- a. A sinusoidal signal with the frequency of 25 Hz.
	- b. The sum of a sinewave of 25 Hz and a sinewave of 50 Hz of the same amplitude.  
- PE5. What is the FFT? Sketch FFT algorithm as a black box, and identify its inputs and outputs.  
- PE6. We applied the FFT to a sampled signal with 1000 samples and sampling frequency Fs=8000Hz. What does the vector returned by the FFT contain? How long is this vector, and how many elements can we really use? What are the first and last elements? What is the distance between 2 adjacent elements? 
- PE7. What is a spectrogram? How can we plot it? Sketch a spectrogram for a 0,1 s-long 100 Hz periodic signal.  
- PE8. Write in pseudocode a rule-based classifier to distinguish an oil bottle from an orange in an image.
# Exercise 1
Write a MATLAB function that acquires an image, and returns the number of large objects (BLOBs) in it. The function should also plot (in a mosaic of windows) all the intermediate images used to obtain the result (by using the MATLAB function subplot). Your standard solution should count only round objects.

[BONUS-1] Investigate how to count other shapes - for example, matches.  
[BONUS-2] Use the MATLAB function imnoise(I,'salt & pepper',d) to add some noise to your image. Investigate the effect of different types of filters (Median, mean). MATLAB Image Processing Toolbox has some functions for this such as medfilt2 and fspecial('average', n);filter2(h, img);

# Exercise 2
For each signal below, write a MATLAB program that plots the frequency spectrum. Submit the code and the obtained images.

a) a sum of 2 sinusoidal signals (for example, 50Hz and 100Hz).  
```matlab
f1 = 50;              % The signal frequency is 50Hz
f2 = 100;            
N = 1024;             % Total number of samples is 1024
Td = 1/f1;            % The signal period is Td
t = 0:Td/N:1;         % Td/N: Sampling period in seconds and the time axis
s = sin(2*pi*f1*t);
Td = 1/f2;
r = sin(2*pi*f2*t);
sum = s + r;
fft_s = fft(sum,N);  % Apply FFT on vector s with N=1024 samples
m = abs(fft_s);      % fft_s is a complex function so we take only the real part

difference = f2/f1;
% The graph will scale by f1
% We always assume that f2 the the higher frequency
x = (0:difference*2)*f1;
m = m(1:difference*2+1);
subplot(2,1,1)
plot(sum(1:3000))
subplot(2,1,2)
stem(x,m)
```
b) the tuning fork signal recorded with a microphone, as you already did in Lab1.
```matlab
filename = '440 Hz.m4a';
[speech, Fs] = audioread(filename);
length = length(speech)
fft_sound = fft(speech,length)  % Take the entire recording and process through FFT
subplot(2,1,1)
plot(speech(1:length-1))

numOfVals = 1000/(Fs/length)
m = abs(fft_sound); % fft_s is a complex function so we take only the real part
x = (0:numOfVals-1)*Fs/length;
subplot(2,1,2)
stem(x,m(1:numOfVals))
```
![[440Hz.jpg]]

[BONUS] Write a program to plot the spectrogram for the signal from a), b) and the C-major scale that can be found in Canvas.

---
References: