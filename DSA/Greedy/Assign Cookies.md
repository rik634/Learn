# Assign Cookies
- Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

- Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

### Logic

### Solution
```
int findContentChildren(vector<int>& g, vector<int>& s) {
        
        int n = g.size();
        int m = s.size();
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int l=0,r=0;
        int count=0;
        while(l<n && r<m)
        {
            if(s[r]>=g[l])
            {
                count++;
                l++;
            }
            r++;
        }
        return count;
    }
```
- Time Complexity: M*logM + N*logN + min(M,N)
- Space Complexity: O(1)
