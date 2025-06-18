# Alien Dictionary
- Given a sorted dictionary of an alien language having N words and k starting alphabets of a standard dictionary. Find the order of characters in the alien language.

- Note: Many orders may be possible for a particular test case, thus you may return any valid order.

### Logic
- Here, ordering is of letters in topological sort is not possible when largest string appears before the shorter one. or when there is cyclic dependency. In such case, we return an empty string.


### BFS
```
string findOrder(vector<string> &words) {
        // code here
        int n = words.size();
        vector<int> adj[26];
        vector<int> indegree(26,0);
        vector<bool> exist(26,false);
        for(auto x: words)
        {
            for(auto a:x)
            {
                exist[a-'a']=true;
            }
        }
        for(int i=0;i<n-1;i++)
        {
            string s1 = words[i];
            string s2 = words[i+1];
            
            int l = min(s1.length(),s2.length());
            
            int j;
            for(j=0;j<l;j++)
            {
                if(s1[j]!=s2[j])
                {
                    adj[s1[j]-'a'].push_back(s2[j]-'a');
                    indegree[s2[j]-'a']++;
                    break;
                }
            }
            if(j<s1.length() && j>=s2.length())
            {
                return "";
            }
        }
        queue<int> q;
        for(int i=0;i<26;i++)
        {
            if(indegree[i]==0 && exist[i]==true)
            {
                q.push(i);
            }
        }
        string res;
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            res = res + (char)('a'+p);
            for(auto it: adj[p])
            {
                indegree[it]--;
                if(indegree[it]==0)
                {
                    q.push(it);
                }
            }
        }
        for(int i=0;i<26;i++)
        {
            if(indegree[i]!=0 && exist[i]==true)
            {
                return "";
            }
        }
        return res;
    }
```
- Time Complexity: O(n*l) (n=number of words, l = max length for each word string) + O(V+E) (BFS time complexity)
- Space Complexity: O(V+E)(for storing adjacency list) + O(V) (queue) + O(2*26) (indegree and exist array)

### DFS
```
```
- Time Complexity:
- Space Complexity: 

