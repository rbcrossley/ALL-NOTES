# Divide and Conquer
Divide the problems into subproblems(till the subproblems become small) and combine the solutions to form a final solution.
We use divide and conquer everyday. Example, to eat an apple, we cut the apple till it fits our mouth.
In general a problem is said to be small, if it can be solved in one or two operations.

# Example
![](_resources/Pasted%20image%2020231213195540.png)
# Generalization
![](_resources/Pasted%20image%2020231213195524.png)
![](_resources/Pasted%20image%2020231213200436.png)
![](_resources/Pasted%20image%2020231213202501.png)
![](_resources/Pasted%20image%2020231213202510.png)
![](_resources/Pasted%20image%2020231213202519.png)
- In DandC strategy, Divide is mandatory but combine is optional(depends on applications). eg binary search, quick sort.
# Max-Min Problem
### Procedure to find max and min (simultaneously in an array of size n).
A=8|16|0|-5|18|2|9|60|30|25
A naïve programmer will write at it like this.
```
Algorithm Non_DC(A,n,max,min)
{
    max<-min<-A[1 ];
    for i<-2 to n 
    {
        if(A[i]>max)
        {
            max<-A[i];
        }
        if(A[i]<min)
        {
            min<-A[i];
        }
    }
}
```
Fundamental operation is comparison.
- index comparison
- element comparison
The total number of element comparisons made in Non-DC=2(n-1). It's because the for loop runs for n-1 times and it needs to be multiplied by 2 which is the number of element comparison happening inside. This is for all cases.
To optimize this, we use else so that we don't test it if the first one is true.
```
Algorithm Non_DC(A,n,max,min)
{
    max<-min<-A[1 ];
    for i<-2 to n 
    {
        if(A[i]>max)
        {
            max<-A[i];
        }
        else if (A[i]<min)
        {
            min<-A[i];
        }
    }
}
```
i) Best Case
Array is in increasing order.
Total comparisons=n-1

ii) Worst Case
Array is in decreasing order.
Total comparisons=2(n-1)

ii) Average Case
On an average, first comparison fails for half of given elements.
Average number of comparisons=(n-1)+n/2=3n/2-1

## Divide And Conquer Approach
A=8,12,0,-6,15,20,3,64,9
```
algorithm maxmin(i,j,max,min)
//a[1..n]is a global array. Parameters i and j are integers.
{
    if(i==j)
    {
        max=min=a[i] ;//small(P) 
    }
    else if(i==j-1)//another case of small(P)
    {
        if(a[i]<a[j])
        {
            max=a[j];
            min=a[i];
        }
        else
        {
            max=a[i];
            min=a[j];
        }
    }
    else
    {
        //problem is large
        mid=(i+j)/2;
        maxmin(i,mid,max,min);
        maxmin(mid+1,j,max1,min1);
        if(max<max1)
        {
            max=max1;
        }
        if(min>min1)
        {
            min=min1;
        }

    }
}
```
# Performance of divide and conquer-maxmin
i) number of element comparisons:
Let T(n) represents number of element comparisons for n elements.
T(n)=
0, n=1(small case)
1, n=2(small case)
2T(n/2)+2, n>2

T(n)=3n/2-2
![](_resources/Pasted%20image%2020231214173721.png)
Space Complexity is O(logn) in DNC and O(1) in non-DNC.
Stack space depends on height of the tree.
h=logn in a binary tree.
