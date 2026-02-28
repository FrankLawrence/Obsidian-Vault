---
Tags: 
Created: 2023-03-07 22:29:06
---
(Links:: [[Basic Processing Unit]])
# Add R3, R4, R5
1. Memory address <- [PC], Read memory, IR <- Memory data, PC <- [PC] + 4
2. Decode instruction, RA <- [R4], RB <- [R5]
3. RZ <- [RA] + [RB]
4. RZ <- [RZ]
5. R3 <- [RY]
# Load R5, X(R7)
1. Memory address <- [PC], Read memory, IR <- Memory data, PC <- [PC] + 4
2. Decode instruction, RA <- [R7]
3. RZ <- [RA] + Immediate value X
4. Memory address <- [RZ], Read memory, RY <- Memory data
5. R5 <- [RY]
# Store R6, X(R8)
1. Memory address <- [PC], Read memory, IR <- Memory data, PC <- [PC] + 4
2. Decode instruction, RA <- [R8], RB <- [R6]
3. RZ <- [RA] + Immediate value X, RM <- [RB]
4. Memory address <- [RZ], Memory data <- [RM] , Write data
5. No action
