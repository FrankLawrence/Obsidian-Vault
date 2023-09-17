# Exercise 1 `fas:CheckCircle`
`y = [10 20 30 40 50 60 70 80 90 100]`
Can you think of another way to generate this vector?
`y = (10:10:100)`
# Exercise 2 `fas:CheckCircle`
Investigate how the MATLAB function rand works. Then, use it to create a vector filled with 10 random numbers, and plot it.  
Hint: By typing  
`>>help xxx`
you can find out how any MATLAB function (in this example, xxx) works and how it can be used. Another way to get more information about a MATLAB function is to type the command name at the MATLAB prompt, then press the F1 key.
# Exercise 3 `fas:CheckCircle`
We have seen during Lecture 2 that a pure tone can be represented with a sinusoidal signal. First, write a MATLAB function `note_gen (f,fs,Td)` which plays a note of frequency $f$, at a sampling frequency of $fs$, lasting for $Td$ seconds. The function does not exist – you have to create it. You will need to use MATLAB's sound command to play this note. The sound command needs two parameters; a vector with the samples that should be played, and the sampling rate. For example, if note is a vector with a sinusoidal signal sampled at a rate of 8000Hz, then it can be played through the PC loudspeakers using the following MATLAB command:

`>> sound (note, 8000);`

You will need to use headphones to hear the generated sound. Second, use this function to generate and play a 500 Hz sound for a duration of 10 seconds. This should be done with the command: `note=note_gen(500,8000,10)`.
# Exercise 4
Say a certain word three times, and record yourself as you do so. Cut the signal into 3 segments, and plot the resulting signals in a mosaic of 3 windows - one window for each segment, placed one above each other - using the MATLAB function `subplot`.
# Exercise 5
## Exercise 5a
Try to record a sentence with a low sampling rate (undersampling) and play back the result. Observe and describe the effect. Explain why this effect happens.

## Exercise 5b
Try to record a sentence, and then increase or reduce the playback rate and observe and describe the effect. Explain why this effect happens.

## Exercise 5c
Modify your script so that the recording is played in reverse. You can search the Internet for a MATLAB function to reverse the sound vector, if you need to.
# Exercise 6
Change the x-axis so that you can see the time in seconds, then plot the acquired signal against the time axis again. Can you determine the signal frequency from this plot? Compare it with the A note frequency.
# Exercise 7
Write MATLAB code that first generates a square green image, waits 10 seconds, then changes the square colour to blue, waits 10 seconds and changes this blue image to a red one, and finally waits 10 seconds and changes the square to yellow.
