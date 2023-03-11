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

## Idea
subtraction between any two numbers results in positive sign if the subtracted from it is biger than subtracted and negative when vice versa, so we can excute the subtraction operation using CNOT gate and check only the sign bit

## Full circuit obtained from ImbQ simulator

![download](https://user-images.githubusercontent.com/112229984/224514951-5232af65-9a7b-4a2c-bc98-a532d396d076.png)
