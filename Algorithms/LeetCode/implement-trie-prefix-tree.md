## 208. Implement Trie (Prefix Tree)

### Descirption 
A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the `Trie` class:
- `Trie()` Initializes the trie object.
- `void insert(string word)` Inserts the string word into the trie.
- `boolean search(string word)` Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
- `boolean startsWith(string prefix)` Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

#### Constraints
- 1 <= word.length, prefix.length <= 2000
- word and prefix consist only of lowercase English letters.
- At most 30,000 calls in total will be made to insert, search, and startsWith.

### Sol 1:
This solution adopts the simple approach of giving each TrieNode a 26 element array of each possible child node it may have. The root of Trie is given with an empty character, which is a standard/typical approach for building out a Trie.

For insert, loop through each character in the word being inserted check if the character is a child node of the current TrieNode i.e. check if the array has a populated value in the index of this character. If the current character **ISN'T** a child node of my current node add this character representation to the corresponding index location then set current node equal to the child that was added. However if the current character **IS** a child of the current node only set current node equal to the child. After evaluating the entire string the Node we left off on is marked as a word this allows our Trie to know which words exist in our "dictionary".

For search I simply navigate through the Trie if I discover the current character isn't in the Trie I return false. After checking each Char in the String I check to see if the Node I left off on was marked as a word returning the result.

Starts with is identical to search except it doesn't matter if the Node I left off was marked as a word or not if entire string evaluated i always return true.

```C++
class TrieNode{
public:
    shared_ptr<TrieNode> next[26];
    bool is_word;
    char value;
    TrieNode() = default;
    TrieNode(char ch):is_word(false),value(ch){}
};

class Trie {
private:
//    TrieNode *root;
    shared_ptr<TrieNode> root;
public:
    /** Initialize your data structure here. */
    Trie() {
        root = std::make_shared<TrieNode>();
        root->value = ' ';
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        shared_ptr<TrieNode> ws = root;
        for(int i = 0; i<word.size(); i++)
        {
            char cur = word[i];
            if(ws->next[cur - 'a'] == nullptr){
                ws->next[cur - 'a'] = std::make_shared<TrieNode>(cur);
            }
            ws = ws->next[cur - 'a'];
        }
        ws->is_word = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        shared_ptr<TrieNode> ws = root;
        for(int i = 0; i < word.size(); i++)
        {
            char cur = word[i];
            if(ws->next[cur - 'a'] == nullptr)
                return false;
            ws = ws->next[cur - 'a'];
        }
        return ws->is_word;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        shared_ptr<TrieNode> ws = root;
        for(int i = 0; i < prefix.size(); i++)
        {
            char cur = prefix[i];
            if(ws->next[cur - 'a'] == nullptr)
                return false;
            ws = ws->next[cur - 'a'];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```
Note:
- Runtime: 76 ms
- Memory Usage: 75.4 MB
