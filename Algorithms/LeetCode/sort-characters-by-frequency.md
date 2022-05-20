## 451. Sort Characters By Frequency

### Descirption 
Given a string s, sort it in decreasing order based on the frequency of the characters. The frequency of a character is the number of times it appears in the string.

Return the sorted string. If there are multiple answers, return any of them.

#### Constraints
- 1 <= s.length <= 500,000
- s consists of uppercase and lowercase English letters and digits.


### Sol 1: Buitlin Sort

```C++
class Solution {
private:
    static bool cmpFreq(pair<char,int> iti, pair<char,int> itj)
    {
        return (iti.second > itj.second);
    }
    
public:
    string frequencySort(string s) {
        map<char, int> freq;
        for(int i = 0; i < s.length(); i++)
            freq[s[i]]++;
        vector <pair<char, int> > freq2(freq.begin(), freq.end());
        sort(freq2.begin(), freq2.end(), cmpFreq);
        string ret(s);
        int i = 0;
        for(vector <pair<char, int> >::iterator it = freq2.begin(); it != freq2.end(); it++)
        {
            for(int ii = 0; ii < it->second; ii++)
                ret[i++] = it->first;
        }
        return ret;
    }
};
```
Note:
- Runtime: 7 ms
- Memory Usage: 8.2 MB


### Sol 2: Bucket Sort

```C++
class Solution {
public:
    string frequencySort(string s) {
        vector<vector<char>> bucket(s.size()+1); // DONT FORGET +1 to size!!
        unordered_map<char, int> m;
        string ans(s);
        for (const auto& e : s)        
            m[e]++;        
           
		// make the buckets
        // idx represents frequency
        // value at index represents characters with that frequency
        for (const auto& e : m)        
            bucket[e.second].push_back(e.first);
        
        for (int i = bucket.size() - 1, ii  = 0; i > 0; i--)
        {
            for (int j = 0; j < bucket[i].size(); j++)
            {
                for (int count = i; count > 0; count--)
                    ans[ii++] = bucket[i][j];
            }
        }
        return ans;
    }
};
```
Note:
- Runtime: 22 ms
- Memory Usage: 13.7 MB

### Sol 3: Priority Queue

```C++
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char,int> umap;
        for(auto &it:s) umap[it]++; 
		
        priority_queue<pair<int,char>> pq; // Max heap : first entry int : frequncy of a character.
		
        for(auto it=umap.begin();it!=umap.end();it++){ // Push everything from map in pq and let it sort according to the key.
            pq.push({it->second,it->first});
        }
        string t="";
        while(!pq.empty()){ // one by one get all those entries untill pq is empty.
            auto temp=pq.top();
            int k=temp.first;
            char c=temp.second;
            while(k--) t+=c;
            
            pq.pop();
        }
        return t;
    }
};
```
Note:
- Runtime: 22 ms
- Memory Usage: 13.7 MB
