# Order of magnitude method
The fundamental operation here is
`x=y+z`
i.e add and assignment.
In the previous test algorithm, the time complexity will become= 1+ $n$ + $n^2$
Order of magnitude of a statement of the algorithm refers to frequency or  count of fundamental operation in the statement.
**How many times an operation is being executed=order of magnitude**

## Algorithm do_it(A,x)
```
integer A, A[n];
integer i,sum=0; //no need of time required for declarations
for (i=1;i<=n;i++)
{
	{
		sum=sum+A[i];	
	}
	print(sum);	
}
```
Order of magnitude way.
Fundamental operation:
Addition=n
Print=1
Total=n+1

The basic objective of apriori analysis is to represent the time complexity(running time) by means of a mathematical function with respect to input size(say n).
- Computation time, load time not accounted on running time of algorithm
## Functions are of two types
- Polynomial
- Exponential(variables in power)

Polynomial functions are in the form of $n^x,\     x>=0$ .
Exponential functions are in the form of $a^n\      ,a>1$  

Exponential functions have higher rate of growth(takes more time).
Polynomial functions have lesser rate of growth(takes less time).
So, it's preferred to develop algorithm with polynomial time complexity(efficient).

- Input classes=n inputs can be arranged in different ways.
## Algorithm 
```
integer n, A[n]
{
	int i;
	for i from  to n
		if(x==A[i])
			print(i)
			exit;
	print("element not found")
}
```
This is a linear search.
- Best case: If element found on i=1.
- Worst case: If element found on i=n.
- Whenever we say time complexity, we're referring to worst case time complexity.
### Worst Case
The input class for which algorithm takes maximum time is called as worst case input and its corresponding time is called worst case time.
### Best Case
The input class for which the algorithm takes minimum time is best case input and corresponding time is called best case time.

### Average Case
is derived in 3 steps
- Enumerate all input classes $I_1,I_2,...,I_k$
- Determine time for each input class $t_1,t_2,...,t_k$
- Associate with each input class the probability function.
$A(n)= \sum_{i=1}^{k} P_i * t_i$ 

### Relation between best, worst and average case
1) Best Case=Average Case=Worst Case(example: merge sort, selection sort, heap sort)
2) Best Case<=Average Case=Worst Case(example: linear search, binary search)
3) Best Case = Average Case < Worst Case (example: quicksort )
4) Best Case < Average Case < Worst Case
