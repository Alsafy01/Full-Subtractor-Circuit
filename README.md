# Full-Subtractor-Circuit
Efficient Designs of Quantum Subtractor Using CNOT Gate
This is my attempt to the Assessment Task one (also described below) of [QOSF](https://qosf.org/qc_mentorship/)'s Quantum Computing Mentorship Program Cohort 7.

## Problem Statement
You have two integers, either positive or negative, and the challenge is to generate a quantum algorithm that returns which is the larger number. Consider an appropriate number of qubits and why it's valid for all kinds of numbers

in case C = find_the_largest_number(A,B)
|     A    |    B    |   C   |
| -------- |:-------:| -----:|
|     5    |    -6   |   5   |
|   -25    |    -12  |  -12  |
|   1024   |    256  |  1024 |

## Context
subtraction between any two numbers results in positive sign if the subtracted from it is biger than subtracted and negative when vice versa, so we can excute the subtraction operation using CNOT gate and check only the sign bit

## Idea
 we are able to build a FS that is capable of performing the binary subtraction operation, using two G3 gates.The proposed FS uses G^3(132) and G^3(231) in addition to two NOT gates, as illustrated in Figure (a). 
 
 ![symmetry-13-01842-g009](https://user-images.githubusercontent.com/112229984/224515305-f5dc4e55-ce6e-4618-b502-c7930c27fe2b.png)

 Setting x2=x3=Y, and considering x1 and x4 as X and Bin, respectively, the proposed FS will perform the binary subtraction operation and the y2 bit will output Diff and the y4 bit will output Bout ; in addition, y1 outputs the result of applying the NOT operation on X and y3 performs the XNOR operation X⊙Y
 
 ![Screenshot 2023-03-12 005610](https://user-images.githubusercontent.com/112229984/224515645-4f9d2d92-9c5a-46a5-a941-db6a748964f4.png)
 
 It is clear that the proposed FS has no constant inputs and no garbage as well.

## Full circuit obtained from IbmQ simulator

![Screenshot (76)](https://user-images.githubusercontent.com/112229984/224514989-da6fa7d5-3bfe-4a29-bde9-cdf7e2319682.png)

## Steps

### convert dicimal to binary
convert both numbers to 8421 code binary numbers with adding one more sign bit on the left as 1 means it's negative number and 0 positve number

|     A    |    5    |  0101 |
| -------- |:-------:| -----:|
|     B    |    -6   |  1110 |

### Check if numbers are same signs or different signs
using the same FS circuit one the two signs bits we will can get three infos [different, Borrow, XNOR]
usually we in that case we use the (different & borrow) bits to figure out which is positve and which is negative but the no garbge design serves us with the ( XOR ) bit that compress the info needed in bit insted of two.

|  (A,B)(s1,s2)  |different|  Borrow |   XNOR  |
| -------------- |:-------:| -------:|:-------:|
|  (0,0)(+,+)    |    0    |    0    |    1    |
|  (0,1)(+,-)    |    1    |    1    |    0    |
|  (1,0)(-,+)    |    0    |    1    |    0    |
|  (1,1)(-,-)    |    1    |    0    |    1    |
