---
Tags: 
Created: 2023-09-08 16:31:16
---
%%(Links:: [[Pervasive Computing]])%%
Members: Frank Würthner and Maurice Ibrahim
# Task 1
```matlab
y = (10:10:100)
```
This creates and array starting at 10 and ending at 100 with numbers in between, each increasing by 10.
# Task 2
```matlab
a = rand(10)
``` 
creates a 10x10 array with random values and saves it to the variable `a`. We can plot `a` and get the following graph: ![[Rand(10).jpg|500]]
# Task 3
```matlab
function [s] = note_gen(f,fs,Td)
t = 0:1/fs:Td;

note = sin(2*pi*f*t)
sound(note, 8000)
```
This is the resulting graph when plotting `note` 
![[note_gen.jpg|500]]
# Task 4

```matlab
Fs = 48000;
filename = 'one.m4a';

[speech, Fs] = audioread(filename)
t = [0:length(speech)-1]/Fs

speech1 = speech(20000:57000)      %first word
speech2 = speech(85000:120000)     %second word
speech3 = speech(160000:200000)    %third word

%associated time arrays used for plotting the audio on a time scale
t1 = [0:length(speech1)-1]/Fs
t2 = [0:length(speech2)-1]/Fs
t3 = [0:length(speech3)-1]/Fs

subplot(1,3,1)
plot(t1,speech1)
subplot(1,3,2)
plot(t2,speech2)
subplot(1,3,3)
plot(t3,speech3)
```
This code results in the following graphs being displayed: ![[Speech.jpg]]
# Task 5
## 5a
The speeches become muffled and much quieter. This happens because the sampling points will often not land on the maximum and minimum amplitudes of the frequencies.
## 5b
The speech is either accelerated (higher play back rate) or slowed (lower play back rate) and the voices appear higher pitched or lower pitched. When the recording has a sampling rate of 8000Hz and we play back at 16000Hz, we will be using twice as many samples per second "speeding up" the audio by a factor of 2x.
## 5c
The function to use would be `flip(speech)`, where `speech` is an array of values extracted from the audio recording with 
```matlab
speech = getaudiodata (rec)
```
# Task 6
We measured the sound, and found the length of one period: $2,7909s - 2,789$. The difference is $0,0019s$ which is and equivalent of $1/0,0019s = 536Hz$
![[TimeXwaveImg.jpg]]
# Task 7
```matlab
x = zeros(200,200,3);

x(:,:,2) = 255;
imshow(x);
title('Green Square');
    
pause(10);

x(:,:,2) = 0;
x(:,:,3) = 255;
imshow(x);
title('Blue Square');

pause(10);

x(:,:,3) = 0;
x(:,:,1) = 255;
imshow(x);
title('Red Square');

pause(10);

x(:,:,2) = 255;
imshow(x);
title('Yellow Square');
```