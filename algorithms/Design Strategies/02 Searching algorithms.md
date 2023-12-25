# Linear Search

```
for(i=0;i<n;i++)
{
    if(A[i]==key)
    {
        printf("Element is found");
        break;
    }
}
if(i==n)
{
    printf("Element is not found");
}
```
If the value of index is the last value of array, then it's said that the element is not found.
# Binary Search
```
key=94
mid=(low+high)/2;
if(low<=high)
{
    if(A[mid]==key)
    {
        return mid;
    }
    else if(A[mid]<key)
    {
        //this is right subproblem
        low=mid+1;
        //if key is greater than middle element, the lowest subproblem will be mid+1
        low=mid+1;
    }
    else
    {
        high=mid-1;
    }
}
```

## Iterative
```
ite_bs(key,low,high)
{
    while(low<=high)
    {  
      mid=(low+high)/2;
    if(A[mid]==key)
    {
        print("Element is found at mid position");
    }
    else if(A[mid]<key)
    {
        print("Element found at %d",mid );
        return;
    }
    else if(A[mid]<key)
    {
        low=mid+1;
    }
    else
    {
        high=mid-1;
    }
    }
}
print("Element is not found");
```
## recursive binary search
```
void rec_bs(int key, int low, int high)
{
   if(low<=high)
    {mid=floor((low+high)/2);
    if(A[mid]==key)
    {
        print("Element is found at %d position",mid);
        return;
    }
    else if(A[mid]<key)
    {
        rec_bs(key,mid+1,high);
    }
    else
    {
        rec_bs(key,low,mid-1);
    }}
}
```
gatewallah algo-02 1:13:17
