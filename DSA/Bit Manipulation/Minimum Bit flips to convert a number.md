# Minimum bit flips to convert a number
- We are given 2 integers, start and goal. We need to calculate how many bits we need to flip to convert the start into goal.

### Solution:
- So, XOR operation works on different kind of bits.
```
int minFlip(int start, int goal)
{
    int ans = start^ goal; // min flips required = number of set bits in the answer
    int count=0;
    while(ans>1)
    {
        if(ans&1==1)
        {
            count++;
        }
        ans = ans >> 1;
    }
    if(ans==1)
    {
        count++;
    }
    return count;
}

```
- Time Complexity: O(log (start^goal)) (base is 2)
- Space Complexity: O(1)