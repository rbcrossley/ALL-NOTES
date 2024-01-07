- to determine resource consumption(time, space etc)
- performance comparison to find the efficient solution
For mobile computing, power consumption is important.
For distributed algorithms, bandwidth is important.
# Methods to determine time and space
Depends on
- software
	- os
	- language
- hardware
	- cpu architecture
	- memory
	- i/o
## Types
- Aposteriori Analysis (After implementation statistics)
- Apriori Analysis (Before implementation)
## Aposteriori Analysis
Advantages
- Accuracy (exact value in real units)
Drawbacks
- Difficult to carry out on all types of machines.
- Platform dependent.
## Apriori Analysis
Advantages
- platform independence
- No need of actual implementations
Drawbacks
- Based on paper and pencil
- Will not give real or actual value in units.
# Components of analytic framework
- tools for analysis
1) language(pseudo)
for describing an algorithm
2) computational model
that the algorithm executes within it
3) metric
for measuring algorithm running time.
4) approach
or notation to characterize running time.
## RAM Model of Computation
![](_resources/Pasted%20image%2020231110191529.png)
Here in this processor, each basic operation takes exactly 1 unit of time.
## Algorithm
```
x=y+z; 
for(i=1;i<=n;i++)
a=b+c;
for(i=1;i<=n;i++)
{
	for(j=1;j<=n;j++)
	{
		k=k*m;
	}
}
```
`x=y+z`
runs for 2 unit of time. 1 for assignment, 1 for addition.
The first for loop runs for:
i=1 => 1 
i<=n => n+1
i++ => n
a=b+c => 2n
Total time=4n+2

The second for loop runs for:
```
i=1 => 1 
i<=n => n+1
i++ => n
```
j=1 => $n$
j<=n =>n(n+1)
j++ => $n^2$


Just multiplied everything with n in nested loop case.

k=k * m => $n^2$ + $n^2$
Total time for the second loop=4 $n^2$ +4 $n$ +2

Thus final time required for entire algorithm= $2+(4n+1)+4n^2+4n+2=4n^2+8n+6$

This is also called as step count method.