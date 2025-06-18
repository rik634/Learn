# Word Ladder I
- Given are the two distinct words startWord and targetWord, and a list denoting wordList of unique words of equal lengths. Find the length of the shortest transformation sequence from startWord to targetWord.
- In this problem statement, we need to keep the following conditions in mind:
  1. A word can only consist of lowercase characters.
  2. Only one letter can be changed in each transformation.
  3. Each transformed word must exist in the wordList including the targetWord.
  4. startWord may or may not be part of the wordList
- Note:  If there’s no possible way to transform the sequence from startWord to targetWord return 0.

### Logic


### BFS
```
int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        
        unordered_set<string> s(wordList.begin(),wordList.end());
        queue<pair<string,int>> q;
        // string and length of the list
        q.push({beginWord,1});
        s.erase(beginWord);
        while(!q.empty())
        {
            auto p = q.front();
            q.pop();
            string str = p.first;
            int length = p.second;
            if(str==endWord)
            {
                return length;
            }
            for(int j=0;j<str.length();j++)
            {
                char original = str[j];
                for(char i = 'a' ; i<='z';i++)
                {
                    str[j]=i;
                    if(s.find(str)!=s.end())
                    {
                        q.push({str,length+1});
                        s.erase(str);
                    }
                }
                str[j]=original;
            }
        }
        return 0;
    }
```
- Time Complexity: O(N*M*26) (N= number of strings in wordList, M = length of strings in wordList, *26 = to check for all the characters from 'a' to 'z')
- If we use set instead of unordered_set, time complexity will be: O(N*M*26*logN)
- Space Complexity:O(N) (unordered_set)


