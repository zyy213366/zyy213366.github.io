---
title: kmp字符匹配算法，字典树，以及ac自动机
date: 2025-07-25 13:51:08
tags: acm培训
cover: /img/02.jpg
---
## kmp字符匹配算法，字典树，以及ac自动机
最近一直在写匹配问题，其实也没办法，难的自己也不会，只能写点简单的。但有一说一ac自动机是一个很有意思的算法。
### kmp字符匹配算法
先说kmp的时间复杂度，在给定两个字符串进行匹配的情况下，假设是最坏的情况一位位比较，那么正常两层循环算法的时间复杂度是o（n*m），而kmp算法能讲时间复杂度下降为o（n+m）。
这是怎么做到的呢，kmp的原理是什么？比如给定两个字符串，主串是ababcababcac，要匹配的模式串是ababcac。kmp算法要做的就是适当的跳过，前面几位肯定是能吻合的，当匹配到第七位时候不适配了，这时候模式串第二位和主串第七位开始比较，最后得出结论。
实现就是通过一个next数组来记录模式串的最大前缀和最大后缀相等的个数，比如对于ababcac，他的next数组是[0, 0, 1, 2, 0, 1, 0]，是在第七位（下标6）不匹配，那么就去看next[6-1=5]，发现值是1，接着跳到下标为1的位置，也就是实际第二位开始匹配，循环往复。做个假设，如果跳转后第二位不匹配怎么办？就去找next[1-1=0]，有一种迭代的思想在里面。
洛谷 p3375模板题
```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAXN 1000010
int kmp[MAXN];

int main(){
    string s1, s2;
    cin >> s1 >> s2;
    s1 = ' ' + s1;
    s2 = ' ' + s2;

    int j = 0;
    // 构建 next 数组
    for (int i = 2; i < s2.length(); ++i) {
        while (j && s2[j + 1] != s2[i]) {
            j = kmp[j];
        }
        if (s2[j + 1] == s2[i]) {
            j++;
        }
        kmp[i] = j;
    }

    j = 0;
    for (int i = 1; i < s1.length(); ++i) {
        while (j && s2[j + 1] != s1[i]) {
            j = kmp[j];
        }
        if (s2[j + 1] == s1[i]) {
            j++;
        }
        if (j == s2.length() - 1) {
            cout << i - (s2.length() - 1) + 1 << endl;
            j = kmp[j];
        }
    }
    for(int i=1;i<s2.length();++i){
        cout<<kmp[i]<<" ";
    }
    cout<<endl;
}
```
### 字典树
字典树其实就是把单词每一个字母插入到树当中，如果两个单词拥有相同的前缀，那么他们就共享，就比如ants和apple，a是他们的共享前缀，他有两个子节点（子节点其实就是通过每个节点加一个next指针数组来实现的），也就是度为2，连接着n和p，还有就是在每个单词结尾的那个单词打上一个bool标签表示结束了。这样做的好处就是便于我们去搜索单词库里有没有某个单词，可以试想，有许许多多的单词用着共享前缀，大大节省了空间，又能查询单词，可见这个算法的优越性。这个模板就不写了，和后面ac自动机一起写出来。
### ac自动机
ac自动机其实就是字典树和kmp算法的结合物，树的构建和字典树一样，然后每个结点，其实就是每个单词还加上了一个fail指针，用处就是匹配失败时候回退到指针指向的位置，重新匹配。考虑字典树中当前的结点 u，u 的父结点是 p，p 通过字符 c 的边指向 u，即 trie(p, c)=u。假设深度小于 u 的所有结点的 fail 指针都已求得。
1.如果 trie(fail(p), c) 存在：则让 u 的 fail 指针指向 trie(fail(p), c)。相当于在 p 和 fail(p) 后面加一个字符 c，分别对应 u 和 fail(u)；
2.如果 trie(fail(p), c) 不存在：那么我们继续找到 trie(fail(fail(p)), c)。重复判断过程，一直跳 fail 指针直到根结点；
3.如果依然不存在，就让 fail 指针指向根结点。
下面是一个示例代码
```cpp
#include <bits/stdc++.h>
using namespace std;

const int ALPHABET = 26;  // 小写英文字母

struct Node {
    int next[ALPHABET];  // 子节点指针
    int fail;            // 失败指针
    vector<int> output;  // 匹配到的模式串编号

    Node() {
        memset(next, -1, sizeof(next));
        fail = 0;
    }
};

vector<Node> trie(1); // 根节点下标为 0
vector<string> patterns;

// 插入一个模式串
void insert(const string& pattern, int id) {
    int cur = 0;
    for (char ch : pattern) {
        int c = ch - 'a';
        if (trie[cur].next[c] == -1) {
            trie[cur].next[c] = trie.size();
            trie.emplace_back();
        }
        cur = trie[cur].next[c];
    }
    trie[cur].output.push_back(id);
}

// 构建失败指针
void build_fail() {
    queue<int> q;
    for (int c = 0; c < ALPHABET; c++) {
        int nxt = trie[0].next[c];
        if (nxt != -1) {
            trie[nxt].fail = 0;
            q.push(nxt);
        } else {
            trie[0].next[c] = 0;  // root 自环
        }
    }

    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int c = 0; c < ALPHABET; c++) {
            int v = trie[u].next[c];
            if (v == -1) continue;

            int f = trie[u].fail;
            while (trie[f].next[c] == -1) {
                f = trie[f].fail;
            }
            trie[v].fail = trie[f].next[c];

            // 继承 fail 指针的输出结果
            for (int id : trie[trie[v].fail].output) {
                trie[v].output.push_back(id);
            }

            q.push(v);
        }
    }
}

// 匹配主串中的所有位置
void search(const string& text) {
    int cur = 0;
    for (int i = 0; i < text.size(); ++i) {
        int c = text[i] - 'a';
        while (trie[cur].next[c] == -1) {
            cur = trie[cur].fail;
        }
        cur = trie[cur].next[c];

        for (int id : trie[cur].output) {
            cout << "匹配到模式串 #" << id << " : " << patterns[id]
                 << " at position " << i - patterns[id].size() + 1 << endl;
        }
    }
}

// 示例用法
int main() {
    patterns = {"he", "she", "his", "hers"};
    for (int i = 0; i < patterns.size(); ++i) {
        insert(patterns[i], i);
    }

    build_fail();

    string text = "ushers";
    search(text);

    return 0;
}
```
当然，学会ac自动机之后我们就可以搞一些比较好玩的东西出来了，比如现在各大网站都在用的敏感词匹配机制，或者输入法的提示词之类的，本质都是ac自动机的构建。我们可以把海量的词构建成一棵树，然后把想要查询匹配的句子放到这颗树当中，进行匹配，其实挺有意思的。下面就是一个英文匹配代码。
```cpp
#include <bits/stdc++.h>
using namespace std;

const int ALPHABET_SIZE = 26;

struct TrieNode {
    TrieNode* children[ALPHABET_SIZE];
    TrieNode* fail;
    vector<string> output;

    TrieNode() {
        fail = nullptr;
        memset(children, 0, sizeof(children));
    }
};

class AhoCorasick {
private:
    TrieNode* root;

public:
    AhoCorasick() {
        root = new TrieNode();
    }

    ~AhoCorasick() {
        destroy(root);
    }

    void insert(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            int idx = ch - 'a';
            if (!node->children[idx])
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->output.push_back(word);
    }

    void build() {
        queue<TrieNode*> q;

        // Initialize fail pointers of root's children
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            if (root->children[i]) {
                root->children[i]->fail = root;
                q.push(root->children[i]);
            }
        }

        while (!q.empty()) {
            TrieNode* current = q.front();
            q.pop();

            for (int i = 0; i < ALPHABET_SIZE; i++) {
                TrieNode* child = current->children[i];
                if (!child) continue;

                TrieNode* failNode = current->fail;
                while (failNode && !failNode->children[i]) {
                    failNode = failNode->fail;
                }

                if (failNode)
                    child->fail = failNode->children[i];
                else
                    child->fail = root;

                // Merge output links
                if (child->fail)
                    child->output.insert(child->output.end(),
                                         child->fail->output.begin(),
                                         child->fail->output.end());

                q.push(child);
            }
        }
    }

    vector<string> query(const string& text) {
        TrieNode* node = root;
        vector<string> result;

        for (char ch : text) {
            if (!islower(ch)) continue;  // skip non-lowercase characters

            int idx = ch - 'a';

            while (node != root && !node->children[idx])
                node = node->fail;

            if (node->children[idx])
                node = node->children[idx];

            if (!node->output.empty()) {
                result.insert(result.end(), node->output.begin(), node->output.end());
            }
        }

        return result;
    }

private:
    void destroy(TrieNode* node) {
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            if (node->children[i]) {
                destroy(node->children[i]);
            }
        }
        delete node;
    }
};

int main() {
    AhoCorasick ac;
    int n;
    cout << "Enter number of patterns: ";
    cin >> n;
    cout << "Enter patterns:\n";

    for (int i = 0; i < n; ++i) {
        string pattern;
        cin >> pattern;
        transform(pattern.begin(), pattern.end(), pattern.begin(), ::tolower); // optional
        ac.insert(pattern);
    }

    ac.build();

    string text;
    cout << "Enter text:\n";
    cin.ignore(); // flush newline
    getline(cin, text);
    transform(text.begin(), text.end(), text.begin(), ::tolower); // optional

    vector<string> matched = ac.query(text);

    if (matched.empty()) {
        cout << "No patterns matched.\n";
    } else {
        cout << "Matched patterns:\n";
        for (const string& word : matched) {
            cout << word << endl;
        }
    }

    return 0;
}
```
示例输出输入，我就拿无耻之徒里lan和lip的对话举例子了，没办法，这电视剧太能爆粗口了。
Enter number of patterns: 5
Enter patterns:
fuck
shit
asshole
joke
stupid
Enter text:
You must be joking! You're fucking him?! Him?! He's married. With kids! What else does he buy you, Ian?" "Listen to me, stupid! You think you know everything, and you don’t know shit…" "Fucking kept boy, at best." "You smart asshole! Go back there now. Promise Kash you’ll keep your mouth shut… because he's shitting himself.
Matched patterns:
fuck
stupid
shit
fuck
asshole
shit