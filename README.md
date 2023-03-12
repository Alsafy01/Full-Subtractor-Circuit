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

###  Convert dicimal to binary
convert both numbers to 8421 code binary numbers with adding one more sign bit on the left as 1 means it's negative number and 0 positve number

|     A    |    5    |  0101 |
| -------- |:-------:| -----:|
|     B    |    -6   |  1110 |

###  Check if numbers are same signs or different signs
using the same FS circuit one the two signs bits we will can get three infos [different, Borrow, XNOR]
usually we in that case we use the (different & borrow) bits to figure out which is positve and which is negative but the no garbge design serves us with the ( XNOR ) bit which negligent the Borrow in bit and results only about the to numbers bits.
that can compress the checked bits in more genral problem with Borrow in

* <ins>Borrow In</ins>: = 0

|  (A,B)(s1,s2)  |different|  Borrow |   XNOR  |                       
| -------------- |:-------:| -------:|:-------:|
|  (0,0) (+,+)   |    0    |    0    |    1    |
|  (0,1) (+,-)   |    1    |    1    |    0    |
|  (1,0) (-,+)   |    1    |    0    |    0    |
|  (1,1) (-,-)   |    0    |    0    |    1    |

* <ins>Borrow In</ins>: = 1

|  (A,B)(s1,s2)  |different|  Borrow |   XNOR  |
| -------------- |:-------:| -------:|:-------:|
|  (0,0) (+,+)   |    1    |    1    |    1    |
|  (0,1) (+,-)   |    0    |    1    |    0    |
|  (1,0) (-,+)   |    0    |    0    |    0    |
|  (1,1) (-,-)   |    1    |    1    |    1    |

let's stick with the Borrow in = 0 table we have 4 possible sign cases both numbers positive, first positive second negative, vice versa and finally both negative in the second and third cases we can immediately find the larger number 

###  Circuit Loading
now we can load the binary digits savely on the our FS circuit by looping throw them and send pair by pair till the last digit

* why it's valid for all kinds of numbers?
it's valide for all kind of numbers (imaginary fractions and imaginery numbers, "at least for now") because there's no limit on the number of digits it get subtracted pair by pair in a for loop, and the negative numbers got handled thanks to the XNOR output in the circuit

every iteration we send the pair with the last itration Borrow out as the next iteration Borrow in 
as our function different_borrow_XNOR(bitA, bitB, Borrow) returns list of three digits [different, borrow, XNOR] as I name it the <ins> DBX </ins> numbers
having the DBX numbers of any iteration we can get all the info needed about that operation and the operation before it and the borrow sent to the next one

###  Last DBX
after the loop successfully finishs we have the last DBX number and as we discussed a DBX number of a specific iteration can gives you info about it's operation it can also tell more about the operation before it <ins> which guarantees convertibility </ins>and having the last borrow out it decides which number is larger.

### References
[1] [Efficient Designs of Quantum Adder/Subtractor Using Universal Reversible Gate on IBM Q](https://www.mdpi.com/2073-8994/13/10/1842#B21-symmetry-13-01842)
[2] [Toffoli, T. Reversible computing. In International Colloquium on Automata, Languages, and Programming](https://scholar.google.com/scholar_lookup?title=Reversible+computing&author=Toffoli,+T.&publication_year=1980&pages=632%E2%80%93644)
[3] [Younes, A. On the universality of n-bit reversible gate libraries](https://scholar.google.com/scholar_lookup?title=On+the+universality+of+n-bit+reversible+gate+libraries&author=Younes,+A.&publication_year=2015&journal=Appl.+Math.+Inf.+Sci.&volume=9&pages=2579)
