---
Tags: 
Created: 2023-09-13 12:00:57
---
(Links:: [[Pervasive Computing]])
## PE1. What is an ultrasound sensor? What does it measure, and how does it work?  
An input device that measures the distance to an object by transmitting frequencies beyond human hearing and measuring the time it takes for the sound to come back (also known as echo location)
## PE2. What is a passive opto‐sensor? What does it measure?
A light sensitive photo diode which measures the intensity of *ambient* light and produces a voltage
## PE3. What is an active opto‐sensor? What does it measure, and how?  
A sensor that emits light, measures the time for the light to come back to the photo diode, and calculates the distance
## PE4. Write (in pseudocode) a controller for a robot that drives forward and stops in front of an obstacle. 
## PE5. Write (in pseudocode) a controller for a robot with two opto‐sensors, that follows a black line.

# Exercise 1. Drive inside a designated area [2p]

Write a MATLAB program to make the vehicle drive inside a circle marked with black tape. Try to make your vehicle intelligent, by choosing every time another trajectory.

```matlab
robot = legoev3('usb')
mA = motor(robot, 'A')
mA.Speed = 50
mC = motor(robot, 'C')
mC.Speed = 50
start(mA)
start(mC)

myColourSensor1 = colorSensor(myenv3,1)
myColourSensor2 = colorSensor(myenv3,2)
while true
	left = readLightIntensity(myColourSensor1, 'reflected')
	right = readLightIntensity(myColourSensor2, 'reflected')
	if(left > 9)
		mA.Speed += 5
		mC.Speed -= 5
	else if(right > 9)
		mC.Speed += 5
		mA.Speed -= 5
	pause(0.2)
end
clear robot
```

# Exercise 2. Dealing with obstacles [3p]

Write a MATLAB program that makes the robot drive forwards, and then stop in front of the first encountered (fixed) obstacle. Adapt your program to make the vehicle to continue driving when the obstacle is taken away. Next, modify the program so that if the obstacle is still visible, the robot follows it at a fixed distance.

```matlab
robot = legoev3('usb')
right = motor(robot, 'A')
right.Speed = 50
left = motor(robot, 'C')
left.Speed = 50
start(right)
start(left)

myUltraSonicSensor = sonicSensor(robot);

while true
    obstacle = readDistance(myUltraSonicSensor);
    if obstacle < 0.07
        stop(right,1); stop(left, 1)
    else
        start(right); start(left)
    end
    pause(0.1)
end

clear("legoev3");
```

# Exercise 3. Follow a line [3p]

Write a program that makes the vehicle start at a black line, follow a curved black line, and then stop at a T-junction. Use the 90 cm x 120 cm boards with a black track, provided for this purpose (see Fig. 1).

```matlab
robot = legoev3('usb')
mA = motor(robot, 'A')
mA.Speed = 100
mC = motor(robot, 'C')
mC.Speed = 100
start(mA)
start(mC)

myColourSensor1 = colorSensor(myenv3,1)
myColourSensor2 = colorSensor(myenv3,2)
while true
	left = readLightIntensity(myColourSensor1, 'reflected')
	right = readLightIntensity(myColourSensor2, 'reflected')
	if(left > 9 && right < 9)
		mC.Speed = 0
		mA.Speed = 100
	else if(right > 9 && left < 9)
		mA.Speed = 0
		mC.Speed = 100
	else if(right > 9 && left > 9)
		mA.Speed = 0
		mC.Speed = 0
		break
	pause(0.2)
end
clear robot
```

Start with a simple algorithm. Use for example a segment without sharp corners and curves. Explain how your algorithm works and evaluate it. Is it smooth or does the robot shake while driving, can it take sharp curves, is it fast, is it slow, just good?

# Exercise 4. Final challenge [2p]

For this exercise you must combine everything you learned in this lab and write a program that makes the vehicle follow the racing track from the start T-junction until the finish T-junction. If obstacles are on the way, the robot should pause. When the obstacle is out of the way, the robot should resume its driving. On the gray lane the robot should slow down.

[OPTIONAL] If things work fine, you still have fun and there is some time left, try to optimize your program for following a line and take part in the race. Explain in the report your key to success.

```matlab
myenv3 = legoev3('usb')
SpeedToUse = 63
mA = motor(myenv3, 'A')
mA.Speed = SpeedToUse
mC = motor(myenv3, 'C')
mC.Speed = SpeedToUse
start(mA)
start(mC)

myColourSensor1 = colorSensor(myenv3,1)
myColourSensor2 = colorSensor(myenv3,2)
UltraSensor = sonicSensor(myenv3)

while 1
    obstacle = readDistance(UltraSensor)
    reflectedLeft = readLightIntensity(myColourSensor1,'reflected')
    reflectedRight = readLightIntensity(myColourSensor2, 'reflected')
    
    if reflectedLeft < 15 && reflectedRight < 15
        pause(0.2)
        if reflectedLeft < 15 && reflectedRight < 15    
            break
        end
    end
    if reflectedLeft > 50 
        mA.Speed = SpeedToUse
    end
    if reflectedRight > 50
        mC.Speed = SpeedToUse/1.5
    end
    if reflectedRight < 30
        mA.Speed = SpeedToUse/2
    end
    if reflectedRight < 31
        mC.Speed = SpeedToUse/2
    end
    if reflectedRight < 15
        mA.Speed = SpeedToUse
        mC.Speed = 1.65 *(-SpeedToUse)
        pause(0.09)
        mA.Speed = SpeedToUse
        mC.Speed = SpeedToUse
        pause(0.02)
    end
    if reflectedLeft < 15
        mC.Speed = SpeedToUse
        mA.Speed = 1.65 * (-SpeedToUse)
        pause(0.114)
        mA.Speed = SpeedToUse
        mC.Speed = SpeedToUse
        pause(0.02)
    end
end
stop(mA)
stop(mC)
clear myenv3
```

---
References: