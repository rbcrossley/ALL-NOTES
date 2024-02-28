# Hexadecimal
Hexadecimal means 16 numbers from 0 to 15.
Thus 4 bits as 2^4=16
So, hexadecimal can express in one digit what is done by binary in 4 digits.
Example 15=1111
# NAND gates are preferred over others
- lower fabrication area
https://electronics.stackexchange.com/questions/110649/why-is-nand-gate-preferred-over-nor-gate-in-industry

# Odd parity
can be checked by XOR gate.
Concept used:
```
The sum(disregarding carries) of an even number of 1s is always 0, and the sum of an odd number of 1s is always 1.
```
Parity checkers only detect single bit errors in transmitted codes.
# byte
8 bits
# BCD
![](_resources/Pasted%20image%2020240216201736.png)
![](_resources/Pasted%20image%2020240216201746.png)
# ASCII
- 7 bit
- alphanumeric code(also contains symbols)
# Nibble
4 bits
# Octal to binary
![](_resources/Pasted%20image%2020240217143823.png)
# Binary to octal
![](_resources/Pasted%20image%2020240217144401.png)
![](_resources/Pasted%20image%2020240217144843.png)
# Decimal to binary
![](_resources/Pasted%20image%2020240217145934.png)
![](_resources/Pasted%20image%2020240217151739.png)
![](_resources/Pasted%20image%2020240217151753.png)
# Decimal to octal
Convert to binary, then to octal. Then group of 3 binaries are selected to make combinations.
# Binary to decimal
![](_resources/Pasted%20image%2020240217150539.png)
# 1's complement
![](_resources/Pasted%20image%2020240217155903.png)
## 1s complement of a decimal binary
0.01011
# 2's complement
Find 1's complement and add 1 to it
![](_resources/Pasted%20image%2020240218170743.png)
![](_resources/Pasted%20image%2020240218170754.png)
![](_resources/Pasted%20image%2020240218170801.png)
https://www.rit.edu/academicsuccesscenter/sites/rit.edu.academicsuccesscenter/files/documents/math-handouts/DM3_TwosComplement_BP_9_22_14.pdf
# De-Morgan's Theorem
![](_resources/Pasted%20image%2020240217165137.png)
Example of De-Morgan's Theorem
![](_resources/Pasted%20image%2020240217165411.png)
# Addition of octal numbers
![](_resources/Pasted%20image%2020240217165729.png)
# K-Map example
![](_resources/Pasted%20image%2020240217170623.png)
# Combinational circuits vs sequential circuits
![](_resources/Pasted%20image%2020240217195713.png)
# Half Adder
![](_resources/Pasted%20image%2020240217195730.png)
![](_resources/Pasted%20image%2020240217195749.png)
![](_resources/Pasted%20image%2020240217195758.png)
# Half subtractor
![](_resources/Pasted%20image%2020240217222956.png)
![](_resources/Pasted%20image%2020240217223121.png)
# Full Adder
![](_resources/Pasted%20image%2020240217225119.png)
![](_resources/Pasted%20image%2020240217225130.png)
![](_resources/Pasted%20image%2020240217225140.png)
# Full subtractor
![](_resources/Pasted%20image%2020240218102741.png)![](_resources/Pasted%20image%2020240218102748.png)
# Parallel Adder
![](_resources/Pasted%20image%2020240218104240.png)
![](_resources/Pasted%20image%2020240218104248.png)
![](_resources/Pasted%20image%2020240218104257.png)
# 4 bit parallel subtractor using full adders
![](_resources/Pasted%20image%2020240218105842.png)
![](_resources/Pasted%20image%2020240218105850.png)
# Parallel Adder and Subtractor in same circuit
![](_resources/Pasted%20image%2020240218130759.png)
![](_resources/Pasted%20image%2020240218130813.png)
# Drawbacks of parallel adder
![](_resources/Pasted%20image%2020240218132212.png)
![](_resources/Pasted%20image%2020240218132220.png)
# Signed binary numbers
First bit is sign and rest is magnitude.
Since signed binary numbers require too much hardware, we use 2's complements instead.
# Half and Full Adders required for n bit number
n-1 full adders and 1 half adder.
# How many full adders required to construct an m-bit parallel adder?
![](_resources/Pasted%20image%2020240218170959.png)
# Carry Look Ahead Adder
reduces the propagation delay or cascading ripple delay.
# NAND and NOR as an universal gate
![](_resources/Pasted%20image%2020240219173847.png)
![](_resources/Pasted%20image%2020240219173857.png)
# Bubbled gates(technique to build gates using NAND/NOR gates)
Rules:
Bubbled AND gate acts as a NOR gate.
Bubbled OR gate acts as a NAND gate.
Bubbled NAND gate acts as a OR gate.
Bubbled NOR gate acts as AND gate.
![](_resources/Pasted%20image%2020240219201644.png)

![](_resources/Pasted%20image%2020240219201652.png)
![](_resources/Pasted%20image%2020240219201701.png)
![](_resources/Pasted%20image%2020240219201708.png)
https://socratic.org/questions/how-many-two-input-nand-gates-are-required-to-perform-the-action-of-a-two-input-
# Number of gates required to implement the following Boolean expression after simplification?
![](_resources/Pasted%20image%2020240219202201.png)
# Laws of boolean algebra
## Commutative law
```
A+B=B+A
```
## Idempotent law
```
A+A=A
A.A=A
```
## Distribution law
```
A(B+C)=AB+AC
```

# For which values the term equals to zero?
![](_resources/Pasted%20image%2020240219204155.png)
# Canonical SOP form
![](_resources/Pasted%20image%2020240219203503.png)
# Machine cycle
- fetch, decode execute of an instruction is broken down into several time intervals.
# NOR.XOR.NAND=?
find output of NOR and NAND and XOR it with NOR. You'll get XOR.
# Binary to Gray
![](_resources/Pasted%20image%2020240220093621.png)
![](_resources/Pasted%20image%2020240220093631.png)
![](_resources/Pasted%20image%2020240220093641.png)
![](_resources/Pasted%20image%2020240220093650.png)
# Gray to binary
Use same concept with equations. 
Gray is minimum error code, reflective code etc.
# Demultiplexer
One source to many destinations.
Data distributor.
# Multiplexer
Data selector.
# ASCII vs EBCDIC
Differs in their collating sequences.
Collating sequence is a mapping between the code point and the required position of each character in a sorted sequence. The alphabetic ordering you get when the computer sorts ASCII text. As you might expect, the letters are numbered consecutively, but since the uppercase letters precede the lowercase, "B" comes before "a" in the ASCII collating sequence.
![](_resources/Pasted%20image%2020240220100839.png)

**Good book recommendations**
Kermit: A File Transfer Protocol
By Bozzano G Luisa
# TTL logic family
Acceptable input signal voltages range from 0 volts to 0.8 volts for a low logic state, and 2 volts to 5 volts for a high logic state.
Fastest TTL=Schottky TTL
Uses bipolar transistors.
HTL highest noise immunity.
# Decoder
- Converts BCD to equivalent decimal.
# Multiplexer and Demultiplexer
![](_resources/CamScanner%2002-22-2024%2013.19_1.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_2.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_3.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_4.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_5.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_6.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_7.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_8.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_9.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_10.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_11.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_12.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_13.jpg)
![](_resources/CamScanner%2002-22-2024%2013.19_14.jpg)
# Encoders and Decoders and Sequential Circuits

References:
https://electronics.stackexchange.com/questions/591677/how-does-an-sr-latch-actually-work

![](_resources/CamScanner%2002-23-2024%2015.15_1%202.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_2%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_3%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_4%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_5%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_6%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_7%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_8%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_9%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_10%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_11%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_12%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_13%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_14%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_15%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_16%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_17%201.jpg)
![](_resources/CamScanner%2002-23-2024%2015.15_18%201.jpg)
# References
Digital Systems: Principles and Applications
By Ronald J. Tocci(great book)

Electronic Digital System Fundamentals
By Dale R. Patrick, Stephen W. Fardo, Vigyan Chandra

Foundations of Digital Logic Design
By Gideon Langholz, Abraham Kandel, Joe L. Mott(great book)

# Math books recommendations
![](_resources/Pasted%20image%2020240218125433.png)