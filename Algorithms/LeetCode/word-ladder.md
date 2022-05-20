## 127. Word Ladder

### Descirption 
A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:
- Every adjacent pair of words differs by a single letter.
- Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
- sk == endWord

Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

#### Constraints
- 1 <= beginWord.length <= 10
- endWord.length == beginWord.length
- 1 <= wordList.length <= 5,000
- wordList[i].length == beginWord.length
- beginWord, endWord, and wordList[i] consist of lowercase English letters.
- beginWord != endWord
- All the words in wordList are unique.

### Sol 1: BFS 

```C++
class Solution {
private:
    static bool checkDiffer(string& a, string& b)
    {
        if(a.size() != b.size())
            return false;
        int cnt = 0;
        for(int i = 0; i < a.size(); i++)
        {
            if(a[i]!=b[i])
                cnt++;
        }
        return (cnt==1);
    }
    
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        // BFS search
        unordered_map<string, vector<string>> mapWord;
        unordered_map<string, bool> seen;
        for(int i = 0; i < wordList.size(); i++)
        {
            if(checkDiffer(wordList[i], beginWord))
            {
                mapWord[beginWord].push_back(wordList[i]);
            }
            for(int j = i + 1; j < wordList.size(); j++)
            {
                if(checkDiffer(wordList[i], wordList[j]))
                {
                    mapWord[wordList[i]].push_back(wordList[j]);
                    mapWord[wordList[j]].push_back(wordList[i]);
                }
            }
            seen[wordList[i]] = false;
        }
        
        if(mapWord.find(beginWord) == mapWord.end())
        {
            for(int i = 0; i < wordList.size(); i++)
            {
                if(checkDiffer(wordList[i], beginWord))
                {
                    mapWord[beginWord].push_back(wordList[i]);
                }
            }
            seen[beginWord] = false;
        }
        
        int ret = 0;        
        queue<string> path;        
        path.push(beginWord);
        seen[beginWord] = true;
        while(!path.empty())
        {
            ret++;
            int size = path.size();
            while(size > 0)
            {
                size--;
                string wordNow = path.front();
                for(int i = 0; i < mapWord[wordNow].size(); i++)
                {
                    if(mapWord[wordNow][i] == endWord)
                    {
                        ret++;
                        return ret;
                    }
                    else if(seen[mapWord[wordNow][i]] == false)
                    {
                        path.push(mapWord[wordNow][i]);
                        seen[mapWord[wordNow][i]] = true;
                    }
                }
                path.pop();
            }
        }
        return 0;
    }
};
```
Note:
- Runtime: 1332 ms
- Memory Usage: 28.8 MB

**Update finding the neighbour with hash table.**

```C++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        if(!dict.count(endWord)) return 0;
        
        queue<string> q;
        q.push(beginWord);
        
        int ll = beginWord.length();
        int level = 0;
        
        while(!q.empty()) {
            level++;
            int size = q.size();
            for(int i = 0; i < size; i++) { 
                string w = q.front();
                q.pop();
                for(int j = 0; j < ll; j++){
                    char curr = w[j];
                    for(int ch = 'a'; ch <= 'z'; ch++) {
                        w[j] = ch;
                        if (w == endWord) return level+1;
                        if (!dict.count(w)) continue;
                        dict.erase(w);
                        q.push(w);
                    }
                    w[j] = curr;
                }
            }
            
        }
        return 0;
    }
};
```
Note:
- Runtime: 113 ms
- Memory Usage: 13.7 MB
