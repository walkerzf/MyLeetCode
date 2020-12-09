## [820. Short Encoding of Words](https://leetcode-cn.com/problems/short-encoding-of-words/)

> Given a list of words, we may encode it by writing a reference string S and a list of indexes A.
>
> For example, if the list of words is ["time", "me", "bell"], we can write it as S = "time#bell#" and indexes = [0, 2, 5].
>
> Then for each index, we will recover the word by reading from the reference string from that index until we reach a "#" character.
>
> What is the length of the shortest reference string S possible that encodes the given words?
>
> Example:
>
> Input: words = ["time", "me", "bell"]
> Output: 10
> Explanation: S = "time#bell#" and indexes = [0, 2, 5].
>
>
> Note:
>
> 1 <= words.length <= 2000.
> 1 <= words[i].length <= 7.
> Each word has only lowercase letters.

## Solution 暴力用Set储存所有长的后缀

```java
//time 19ms 41.7MB
class Solution {
    public int minimumLengthEncoding(String[] words) {
        Set<String> dic=new HashSet<>(Arrays.asList(words));
        for(String s:words){
            for(int i=1;i<s.length();i++){
                //后缀
                if(dic.contains(s.substring(i))){
                    dic.remove(s.substring(i));
                }
            }
        }
        int ans=0;
        for(String a:dic){
            ans+=a.length()+1;
        }
        return ans;
    }
}
```

## Solution 字典树

* 将单词倒序插入字典树，如果没有子节点即标记
* 或者是将数组排序，将长度更长的字符串先放入字典树，这样就不会出现重复的了

```java
//time 17ms 45.7MB
class Solution {
    class TireNode{
        TireNode []Children;
        boolean isWord;
        public TireNode(){
            Children=new TireNode[26];
            isWord=false;
        }
        public TireNode get(char c){
            if(Children[c-'a']==null){
                this.Children[c-'a']=new TireNode();
                this.isWord=true;
            }
            return Children[c-'a'];
        }

    }
    //care about the reverse-order ,the isWord Variable how to set
    public int minimumLengthEncoding(String[] words) {
        TireNode tire=new TireNode();
        Map<TireNode ,Integer> m=new HashMap<>();
        for(int i=0;i<words.length;i++){
            String s=words[i];
            TireNode cur=tire;
            for(int j=s.length()-1;j>=0;j--){
                cur=cur.get(s.charAt(j));
            }
            m.put(cur,i);
        }
    
    int ans=0;
    for( TireNode n : m.keySet()){
        if(n.isWord==false){
            ans+=words[m.get(n)].length()+1;
        }
    }
    return ans;
}
}
```

## Stream of Characters

> Implement the `StreamChecker` class as follows:
>
> - `StreamChecker(words)`: Constructor, init the data structure with the given words.
> - `query(letter)`: returns true if and only if for some `k >= 1`, the last `k` characters queried (in order from oldest to newest, including this letter just queried) spell one of the words in the given list.
>
>  
>
> **Example:**
>
> ```
> StreamChecker streamChecker = new StreamChecker(["cd","f","kl"]); // init the dictionary.
> streamChecker.query('a');          // return false
> streamChecker.query('b');          // return false
> streamChecker.query('c');          // return false
> streamChecker.query('d');          // return true, because 'cd' is in the wordlist
> streamChecker.query('e');          // return false
> streamChecker.query('f');          // return true, because 'f' is in the wordlist
> streamChecker.query('g');          // return false
> streamChecker.query('h');          // return false
> streamChecker.query('i');          // return false
> streamChecker.query('j');          // return false
> streamChecker.query('k');          // return false
> streamChecker.query('l');          // return true, because 'kl' is in the wordlist
> ```
>
>  
>
> **Note:**
>
> - `1 <= words.length <= 2000`
> - `1 <= words[i].length <= 2000`
> - Words will only consist of lowercase English letters.
> - Queries will only consist of lowercase English letters.
> - The number of queries is at most 40000.

* Intuition: The ```Trie``` Structure come into my mind first .We can implement the Trie ,and maintain many pointers to the``` TrieNode``` ,we especially must care about that when we iterate the List ,we can not add something ,we need to add something to``` tmp``` variable ,and copy the list

## Solution Word-Order Build Trie

```java
class TrieNode {
    TrieNode[] child;
    boolean isWord;
    boolean valid ;
    public TrieNode() {
        child = new TrieNode[26];
        isWord = false;
        valid = false;
    }
}

class StreamChecker {
    List<TrieNode> s = new ArrayList<>();
    TrieNode head = new TrieNode();

    //build the Trie
    public StreamChecker(String[] words) {
        s.add(head);
        head.valid = true ;
        for (String w : words) {
            TrieNode cur = head;
            for (int i = 0; i < w.length(); i++) {
                int index = w.charAt(i) - 'a';
                if (cur.child[index] == null) {
                    cur.child[index] = new TrieNode();
                }
                cur = cur.child[index];
            }
            cur.isWord = true;
        }
    }

    public boolean query(char letter) {
        int index = letter - 'a';
        boolean res = false;
        //Attention!
        List<TrieNode> n =new ArrayList<>();
        for (TrieNode t : s) {
            if (t.child[index]!= null) {
                t.child[index].valid = true;
                n.add(t.child[index]);
                if (t.child[index].isWord) res = true;
            } else {
                if (t != head) t.valid =false;
            }
        }
        //!Attention
        n.add(head);
        s = n;
        return res;
    }
}
```

## Solution Reverse-Word Order

```java
class StreamChecker {
    // Idea: to check word reversely from the back, use Trie
    class Trie {
        // mark the end of the word
        boolean isWord;
        Trie[] next = new Trie[26];
    }
    Trie root = new Trie();
    StringBuilder sb = new StringBuilder();

    // create Trie
    //flexity to set the TireNode ,
    //although we reverse the word order to build the tree ,
    //we actually to treat the word  reverse -order
    public StreamChecker(String[] words) {
        for (String s : words) {
            Trie node = root;
            int len = s.length();
            for (int i = len - 1; i >= 0; i--) {
                char c = s.charAt(i);
                if (node.next[c - 'a'] == null) {
                    node.next[c - 'a'] = new Trie();
                }
                node = node.next[c - 'a'];
            }
            node.isWord = true;
        }
    }
    //elegant
    public boolean query(char letter) {
        sb.append(letter);
        Trie node = root;
        for (int i = sb.length() - 1; i >= 0 && node != null; i--) {
            char c = sb.charAt(i);
            node = node.next[c - 'a'];
            if (node != null && node.isWord) {
                return true;
            }
        }
        return false;
    }
}

```

## [1268. Search Suggestions System](https://leetcode-cn.com/problems/search-suggestions-system/)

Given an array of strings `products` and a string `searchWord`. We want to design a system that suggests at most three product names from `products` after each character of `searchWord` is typed. Suggested products should have common prefix with the searchWord. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return *list of lists* of the suggested `products` after each character of `searchWord` is typed. 

**Example 1:**

```
Input: products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
Output: [
["mobile","moneypot","monitor"],
["mobile","moneypot","monitor"],
["mouse","mousepad"],
["mouse","mousepad"],
["mouse","mousepad"]
]
Explanation: products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"]
After typing m and mo all products match and we show user ["mobile","moneypot","monitor"]
After typing mou, mous and mouse the system suggests ["mouse","mousepad"]
```

**Example 2:**

```
Input: products = ["havana"], searchWord = "havana"
Output: [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]
```

**Example 3:**

```
Input: products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"
Output: [["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]
```

**Example 4:**

```
Input: products = ["havana"], searchWord = "tatiana"
Output: [[],[],[],[],[],[],[]]
```

**Constraints:**

- `1 <= products.length <= 1000`
- There are no repeated elements in `products`.
- `1 <= Σ products[i].length <= 2 * 10^4`
- All characters of `products[i]` are lower-case English letters.
- `1 <= searchWord.length <= 1000`
- All characters of `searchWord` are lower-case English letters.

## Trie

```c++
class Trie{
public:
    Trie * child[26];
    bool valid;
    Trie(){
        for(int i =0;i<26;i++){
            child[i] = nullptr;
        }
        valid =false;
    }
};
string dic[26] ={"a","b","c","d","e","f","g","h","i","j","k","l",
                "m","n","o","p","q","r","s","t","u","v","w","x","y","z"};
class Solution {
    
public:
   
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
         vector<vector<string>> res;
        Trie *root = new Trie();
        for(int i = 0;i<products.size();i++){
            insertword(products[i],root);
        }
        Trie * cur = root;
        string tmp ;
        bool flag = true;
        for(int i = 0;i<searchWord.length();i++){
            if(!flag) {
                res.push_back({});
                continue;
            }
            
            tmp+=searchWord.substr(i,1);
            int index = searchWord[i] - 'a';
            if(cur->child[index]){
                cur = cur->child[index];
                vector<string> v;
                dfs(v,cur,tmp);
                res.push_back(v);
            }else {
                flag =false;
                res.push_back({});
            }
          
        }
        return res;
    }
    void insertword(string & s,Trie * root){
        Trie* cur = root;
        for(int i = 0;i<s.length();i++){
            int index =s[i]-'a';
            if(cur->child[index]==nullptr) {
                cur->child[index] = new Trie();
            }
            cur = cur->child[index];
        }
        cur->valid = true;
    }
    void dfs(vector<string> & v,Trie * cur,string s){
        if(v.size()==3) return;
        if(cur->valid){
            v.push_back(s);
        }
        
        for(int i = 0;i<26;i++){
            if(cur->child[i]){
                dfs(v,cur->child[i],s+dic[i]);
            }
        }
        /*
        for(char c='a';c<='z';c++){
            int index= c-'a';
            if(cur->child[index]){
                dfs(v,cur->child[index],s+c);
            }
        }
        */
        return;
    }
};
```

or optimizated . We store three min string in every node in trie

```c++
struct Trie {
    unordered_map<char, Trie*> child;
    priority_queue<string> words;
};

class Solution {
private:
    void addWord(Trie* root, const string& word) {
        Trie* cur = root;
        for (const char& ch: word) {
            if (!cur->child.count(ch)) {
                cur->child[ch] = new Trie();
            }
            cur = cur->child[ch];
            cur->words.push(word);
            if (cur->words.size() > 3) {
                cur->words.pop();
            }
        }
    }
    
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        Trie* root = new Trie();
        for (const string& word: products) {
            addWord(root, word);
        }
        
        vector<vector<string>> ans;
        Trie* cur = root;
        bool flag = false;
        for (const char& ch: searchWord) {
            if (flag || !cur->child.count(ch)) {
                ans.emplace_back();
                flag = true;
            }
            else {
                cur = cur->child[ch];
                vector<string> selects;
                while (!cur->words.empty()) {
                    selects.push_back(cur->words.top());
                    cur->words.pop();
                }
                reverse(selects.begin(), selects.end());
                ans.push_back(move(selects));
            }
        }
        
        return ans;
    }
};
```



## Binary Search

方法一中的字典树需要使用额外的空间。可以发现，字典树实际上是帮助我们完成了排序的工作。如果我们直接将数组 products 中的所有字符串按照字典序进行排序，那么对于输入单词 searchWord 的某个前缀 prefix，我们只需要在排完序的数组中找到最小的三个字典序大于等于 prefix 的字符串，并依次判断它们是否以 prefix 为前缀即可。由于在排完序的数组中，以 prefix 为前缀的字符串的位置一定是连续的，因此我们可以使用二分查找，找出最小的字典序大于等于 prefix 的字符串 products[i]，并依次判断 products[i]，products[i + 1] 和 products[i + 2] 是否以 prefix 为前缀即可。

此外，在我们输入单词 seachWord 中每个字母的过程中，prefix 的字典序是不断增大的。因此在每次二分查找时，**我们可以将左边界设为上一次找到的位置 i，而不用从以 0 为左边界，这样可以减少每次二分查找中的查找次数（但不会减少时间复杂度的数量级）。**

```c++
class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        sort(products.begin(), products.end());
        string query;
        auto iter_last = products.begin();
        vector<vector<string>> ans;
        for (char ch: searchWord) {
            query += ch;
            auto iter_find = lower_bound(iter_last, products.end(), query);
            vector<string> selects;
            for (int i = 0; i < 3; ++i) {
                if (iter_find + i < products.end() && (*(iter_find + i)).find(query) == 0) {
                    selects.push_back(*(iter_find + i));
                }
            }
            ans.push_back(selects);
            iter_last = iter_find;
        }
        return ans;
    }
};


```

