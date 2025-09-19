---
title: 哈希表
date: 2025-08-14 11:27:06
tags: acm培训
cover: /img/02.jpg
---
### hash表
现实生活中我们往往有很多账户，而每一个账户总是需要一个与之匹配的密码，但对于系统来说，它需要存储的账户以及密码就有千千万万，为了提高效率，在实际使用中总是用哈希表来存储密码。
哈希表就是把密码按照一个计算公式转换成一个独一无二的大数，然后用户输入密码，按同样的公式进行计算，算出来与这个大数比对，得出是否是同一密码。这样做的优势是原本比对密码需要一位一位比对，效率低下，而如今的比对方式则快很多，直接两个值比对，会快很多。
但在实际应用中，会有极小极小概率两个密码算出来的哈希值是一样的，这时候我们就需要双哈希，对于每一个密码进行两遍哈希运算，然后把值进行比较，这样哈希碰撞的概率可以近乎为0。下面是示例代码，哈希表在c++中的应用。
```cpp
#include <bits/stdc++.h>
using namespace std;

/*
  ============================
  教学目标
  1) “哈希表哈希公式 + 取模”：把键(用户名)映射到桶
     index_hash(name) = ( ... ) % M
  2) “口令摘要哈希 + 取模”：把 salt|password 映射到 64bit/十六进制字符串
     pw_hash(salt, pwd) = ( ... ) % MOD  (并迭代多轮)
  3) 冲突处理：分离链接法（vector<list<...>>）
  ============================
*/

// ========== 工具函数 ==========
static string to_hex64(uint64_t x) {
    static const char* hex = "0123456789abcdef";
    string s(16, '0');
    for (int i = 15; i >= 0; --i) {
        s[i] = hex[x & 0xF];
        x >>= 4;
    }
    return s;
}

static string gen_salt(size_t len = 16) {
    static const char pool[] =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789./";
    static random_device rd;
    static mt19937_64 rng(rd());
    uniform_int_distribution<int> dist(0, (int)sizeof(pool) - 2);
    string s; s.reserve(len);
    for (size_t i = 0; i < len; ++i) s.push_back(pool[dist(rng)]);
    return s;
}

// ========== 口令哈希（教学版，非生产！） ==========
// 公式：H = (H * BASE + byte) % MOD
// 注意：BASE 必须 < MOD
struct SimplePasswordHasher {
    static constexpr uint64_t MOD  = 1000000007ULL;   // 大素数
    static constexpr uint64_t BASE = 315423904ULL;    // 1315423911 % 1e9+7（取 < MOD 的值）

    static uint64_t hash_once(const string& s) {
        uint64_t h = 0;
        for (unsigned char c : s) {
            h = (h * BASE + c) % MOD;
        }
        return h;
    }

    // 简单的 key stretching：多轮迭代（演示）
    static string hash_password(const string& salt, const string& password, int rounds = 10000) {
        string mix = salt + "#" + password;
        uint64_t h = 0;
        for (int i = 0; i < rounds; ++i) {
            // 每一轮把上轮结果拼回去再哈希（演示思路）
            string t = mix + "|" + to_hex64(h);
            h = hash_once(t);
        }
        return to_hex64(h);
    }
};

// ========== 哈希表：分离链接法 ==========
struct UserEntry {
    string username;
    string salt;
    string pw_hash_hex; // SimplePasswordHasher 产出的十六进制摘要（教学用途）
};

class HashTableUsers {
public:
    // 桶数：素数（减少冲突）；你可以改大些，例如 100003
    explicit HashTableUsers(size_t bucket_count = 10007)
        : M(bucket_count), buckets(bucket_count) {}

    // 插入或更新用户
    void upsert(const string& username, const string& password) {
        size_t idx = index_hash(username);
        auto& lst = buckets[idx];
        for (auto& e : lst) {
            if (e.username == username) {
                // 更新口令
                e.salt = gen_salt();
                e.pw_hash_hex = SimplePasswordHasher::hash_password(e.salt, password);
                return;
            }
        }
        // 新增
        UserEntry e;
        e.username = username;
        e.salt = gen_salt();
        e.pw_hash_hex = SimplePasswordHasher::hash_password(e.salt, password);
        lst.push_front(std::move(e));
    }

    // 校验登录
    bool verify(const string& username, const string& password) const {
        const UserEntry* e = find_user(username);
        if (!e) return false;
        string h = SimplePasswordHasher::hash_password(e->salt, password);
        return h == e->pw_hash_hex;
    }

    // 删除用户
    bool erase(const string& username) {
        size_t idx = index_hash(username);
        auto& lst = buckets[idx];
        for (auto it = lst.begin(); it != lst.end(); ++it) {
            if (it->username == username) {
                lst.erase(it);
                return true;
            }
        }
        return false;
    }

    // 调试/查看：把每个桶里的人数统计一下
    vector<size_t> bucket_sizes_sample(size_t first_k = 20) const {
        first_k = min(first_k, M);
        vector<size_t> a(first_k);
        for (size_t i = 0; i < first_k; ++i) a[i] = buckets[i].size();
        return a;
    }

private:
    size_t M;
    vector<list<UserEntry>> buckets;

    // ====== 哈希表索引的哈希公式（重点：含“取模”）======
    // 经典的多项式滚动：h = (h * B + byte) % M
    // 这里 M 是桶数（素数），B 选个小质数即可，比如 131
    size_t index_hash(const string& key) const {
        const uint64_t B = 131;      // 小质数
        uint64_t h = 0;
        for (unsigned char c : key) {
            h = (h * B + c) % M;     // —— 关键的“取模 M” —— 把哈希值映射到 [0, M)
        }
        return (size_t)h;
    }

    const UserEntry* find_user(const string& username) const {
        size_t idx = index_hash(username);
        const auto& lst = buckets[idx];
        for (const auto& e : lst) {
            if (e.username == username) return &e;
        }
        return nullptr;
    }
};

// ========== 演示 ==========
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    HashTableUsers db(10007); // 1 万级桶，素数

    cout << "简易账户系统（教学 demo）\n";
    cout << "命令：\n"
         << " 1 signup <username> <password>\n"
         << " 2 login  <username> <password>\n"
         << " 3 chpwd  <username> <new_password>\n"
         << " 4 del    <username>\n"
         << " 5 stat   （查看前 20 个桶的尺寸）\n"
         << " 0 exit\n";

    while (true) {
        string cmd;
        if (!(cin >> cmd)) break;

        if (cmd == "0" || cmd == "exit") break;

        if (cmd == "signup" || cmd == "1") {
            string u, p; cin >> u >> p;
            db.upsert(u, p);
            cout << "OK: user '" << u << "' created/updated.\n";
        } else if (cmd == "login" || cmd == "2") {
            string u, p; cin >> u >> p;
            cout << (db.verify(u, p) ? "Login success\n" : "Login failed\n");
        } else if (cmd == "chpwd" || cmd == "3") {
            string u, p; cin >> u >> p;
            db.upsert(u, p);
            cout << "OK: password updated for '" << u << "'.\n";
        } else if (cmd == "del" || cmd == "4") {
            string u; cin >> u;
            cout << (db.erase(u) ? "Deleted\n" : "No such user\n");
        } else if (cmd == "stat" || cmd == "5") {
            auto v = db.bucket_sizes_sample(20);
            cout << "First 20 buckets sizes: ";
            for (size_t i = 0; i < v.size(); ++i) {
                if (i) cout << ", ";
                cout << v[i];
            }
            cout << "\n";
        } else {
            cout << "Unknown cmd\n";
        }
    }

    return 0;
}

```
所谓加盐就是对于每一个密码加一个数让其更加离散代码。哈希公式 h = (h * B + byte) % M。