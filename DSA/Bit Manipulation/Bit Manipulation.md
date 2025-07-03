# Bit Manipulation

1. Binary Number conversion (1's and 2's complement)
2. Operators (AND,OR, XOR, NOT, shift)
3. Swap 2 numbers
4. Check if the i'th bit is set or not
5. Extract the i'th bit
6. Set the i'th bit
7. Clear the i'th bit
8. Toggle the i'th bit


## Binary and decimal numbers
- When we store "int x=13', it is always stored in form of bits (0 and 1). "int' needs 32 bits. When 13 converted to binary, it is "1101". what are the rest 28 bits? It is saved as (0-28 times + 1101) in the storage/system.  
- "long long" = 64 bits

1. How do we differentiate while storing the positive and negative numbers?: 
   - For 13 decimal number, 1101 is the binary representation. As we know, this is of type "int", which is of 32 bits. (0 is 28 times + 1101). Here, the first bit from the start is for sign (+ve or -ve) of the number. If this bit is 0, it is the positive number, while, if it is 1, then it is a negative number.
   - Here, positive number (+13) is stores as (1101). While storing the negative number (-13), it is first converted to 2's complement and then stored.
   - The 2's complement for 13 is (11-------0011). Its is stores as 2's complement. Here, if we observe 1st bit, it is 1, which denotes that, it is a negative number.
   - So, the largest possible value of integer that can be stores represented in binary form is: (011111----32bits). First bit will be 0, to represent the positive number. So, the largest value is (2^31-1). This value if often represented as INT_MAX.
   - The same can be done to find the smallest number. It will be represented as (10000- 32 bits) in binary form. The corresponding decimal value for it will be -(2^31-1).
   - 


## 1's and 2's complement:
- 1's complement: For any decimal number, convert it to binary, and flip its bits. The final result will the 1's complement of the given decimal number
- 2's complement: For any given decimal number, find the 1's complement and 1 to it. The final result will be the 2's complement of the given decimal number.

## Operators;
1. AND (&):
   - all true: True
   - 1 false: False

2. OR (|):
   - 1 True: True
   - all false: False

3. XOR (^):
   - Number of 1's is odd: True
   - Number of 1's is even: False

4. Shift:
    a. Right shift (>>):
      - Here, (x >> k) basically means, (x / (2^k))

      Examples:
      - (13 >> 2) => (001101) >> 2 => (000011)
      - (13 >> 4) => (001101) >> 4 => (000000)
    
    b. Left Shift (<<):
      - Here, (x << k) basically mean, (x*(2^k))
      - As we can observe, left shift operation increases the value of the number. And the largest value it can have is (2^31 -1). So, during the left shift operation, we have to make sure that it does not go beyond this value, as it might give us the overflow error during running/compiling.

      Examples:
      - (13 << 2) => (001101) << 2 => (00011010)
      - (13 << 4) => (001101) << 4 => (11010000) 

5. NOT (~):
   - If we want to perform NOT operation on x, i.e., ~(x), and x is positive. We follow below steps:
     a. Flip all the bits for binary representation of x.
     b. Check the first bit, to find if the number is negative. If is is negative, get the 2's complement. Or else stop.

    Examples:
    - ~(5) => ~(000101) => (flipped the bits) (1111---010) => (first bit is 1, it is negative) (get 2's complement, flip the bits and add 1) => (0000----010) + (000001) => (1000000110) => ~(6) (System will store this)
    - ~(~6) => (as "-6" is the negative number, it will be stores in 2's complement form) => (2's complement of "6" is 1111----010) => (as per the steps first flip the bits) (0000-----01011) => (as the first is 0, the number of not negative. So, we will stop)=> (0000----01011) => Its decimal equivalent is (+5). It is stored as +5

###
Note: Negative number is always stored in 2's complement form. It is never stores as "-5".
###     


# Swap Two Numbers Without third variable:
- Swap 2 numbers without using the third variable

### Using third variable:
 - We are given 2 variables a and b to swap.
```
{
    temp = a;
    a = b;
    b= temp;
}
```

### Without third variable:
- We will use XOR operator. Here, even number of 1's, give 0, and odd number of 1's give 1.
- We are given 2 variables a and b to swap.
```
{
    a = a ^ b;
    b = a ^ b; // b = (a ^ b) ^ b = a (as b^b=0 and b^0 = b)
    a = a ^ b; // a = (a^b)^a = b (a^a=0 and a^0=a)
}
```

# Check if the i'th bit is set or not
- We are give a number N, and given the index i. We need to find if the i'th bit in number N is set or not.

- Time and Space Complexity: O(1) (for both mentioned below)

### Using Left shift operator:
- Examples:
  - N=13, i=2 => N = 1101, (1 << i) = 0100 => N & (1 << i) = (1101) & (0100) => 0100 (So, the i'th bit is set)
  - N=13, i=1 => N = 1101, (1 << i) = 0010 => N & (1 << i) = (1101) & (0010) => 0000 (So, the i'th bit is set)

### Solution:

```
if((N & (1 << i)) ! =0)
{
    "i'th bit is set";
}
else
{
    "i'th bit is not set;
}
```

### Using right shift operator:
- Examples:
  - N=13, i=2 => N = 1101, (N >> i) = 0011 => (N >> i) & (0001) => (0011) & (0001) => 1 (So, the i'th bit is set)
  - N=13, i=1 => N = 1101, (N >> i) = 0110 => (N >> i) & (0001) => (0110) & (0001) => 0 (So, the i'th bit is set)

```
if(((N >> i) & i)!=0)
{
    "i'th bit is set
}
else
{
    "i'th bit is not set
}
```

# Set i'th bit
- We are given the values of N and i. We need to set the i'th bit for the number N. Here, the rest of the bits will be unchanged.
- Examples:
  - N=9, i=2 => N=1001, (1 << i)=(0100) => N | (1 << i) = (1001) | (0100) => 1101 (i'th bit is set) (Using left shift operation)

```
    N = (N | (1 << i))
```

# Clear the i'th bit
- We are given the values of N and i. We need to clear the i'th bit for the number N.

```
N = N & (~(1 << i))
```

# Toggle the i'th bit
- W e are given the values of N and i. If the i'th bit is 1, we need to set it to 0. While, if the i'th bit is 0, we need to set it to 1.

```
N = N ^ ( 1 << i)
```

# Remove the last/ rightmost set bit
- Here, we are given the values of N and i. We need to clear the first set bit from the right side. If we reduce the value N by 1, the first set bit from the right side will be set to 0.
```
 N = N & (N-1);
```

# Check if the number is a power of 2:
- We are given the value of N. For a number N to be the power of 2, there has to be only one set bit in its binary representation.

```
if ((N& (N-1))==0)
{
    "Number is power of 2"
}
else
{
    "Number is not power of 2"
}
```

# Count the number of set bits
- We are given the value of N. 
- Last bit of an odd number is always 1.
- We can also use the C++ STL function: "__builtin_popcount(n)"
```
int countSetBits(int n)
{
    int count=0;
    while(n>1)
    {
        if(n%2==1) // we can also write (n&1==0)
        {
            count++;
        }
        n = n/2; // can also write (n= n >> 1)
    }
    if(n==1)
    {
        count++;
    }
    return count;
}
```
- Time Complexity: O(logn) (base = 2)

- Here, we iterating the loop, until N becomes zero. And for every iteration, we are removing the rightmost set bit from the number.
```
int countSetBit(int n)
{
    int count=0;
    while(N!=0)
    {
        N = N & (N-1);
        count++;
    }
    return count;
}
```
- Time Complexity: O(Number of set bits) (In worst possible situation, for an integer, it will be O(31))


