# Wildcard Matching
- Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

- '?' Matches any single character.
- '*' Matches any sequence of characters (including the empty sequence).
- The matching should cover the entire input string (not partial).

### Logic
- f(i,j): returns if the s1(0,i) matches with s2(0,j).


### Recursion
```
int f(i,j)
{le


    if(s1[i]==s2[j] || s2[j]=='?)
    {
        return f(i-1,j-1);
    }
}

```

### Memoization



### Tabulation


### Space Optimized
