---
title: AC自动机简介
status: public
order: 2
categories:
  - 技术分享
tags:
  - 算法
---

 

# AC自动机详解

AC自动机（Aho-Corasick Automaton）是一种多模式串匹配算法，用于在文本中高效地查找多个模式串的出现位置。它结合了Trie树和KMP算法的思想，能够在O(n)的时间复杂度内完成多模式串匹配。

## 基本概念

AC自动机由以下几个核心部分组成：

1. **Trie树结构**：用于存储所有模式串
2. **失败指针（Fail指针）**：类似于KMP中的next数组，用于匹配失败时的跳转
3. **输出表**：记录每个节点代表的模式串

## 实现步骤

### 1. 构建Trie树

首先将所有模式串插入到Trie树中：

```cpp
struct TrieNode {
    TrieNode* children[26]; // 假设只处理小写字母
    TrieNode* fail;
    bool isEnd; // 是否是某个模式串的结尾
    int length; // 模式串长度（如果是结尾节点）
  
    TrieNode() {
        memset(children, 0, sizeof(children));
        fail = nullptr;
        isEnd = false;
        length = 0;
    }
};

void insert(TrieNode* root, const string& pattern) {
    TrieNode* node = root;
    for (char c : pattern) {
        int index = c - 'a';
        if (!node->children[index]) {
            node->children[index] = new TrieNode();
        }
        node = node->children[index];
    }
    node->isEnd = true;
    node->length = pattern.size();
}
```

### 2. 构建失败指针

使用广度优先搜索（BFS）构建失败指针：

```cpp
void buildFailPointer(TrieNode* root) {
    queue<TrieNode*> q;
    root->fail = nullptr;
    q.push(root);
  
    while (!q.empty()) {
        TrieNode* current = q.front();
        q.pop();
      
        for (int i = 0; i < 26; ++i) {
            if (current->children[i]) {
                TrieNode* child = current->children[i];
              
                if (current == root) {
                    child->fail = root;
                } else {
                    TrieNode* p = current->fail;
                    while (p) {
                        if (p->children[i]) {
                            child->fail = p->children[i];
                            break;
                        }
                        p = p->fail;
                    }
                    if (!p) child->fail = root;
                }
                q.push(child);
            }
        }
    }
}
```

### 3. 匹配过程

```cpp
void search(TrieNode* root, const string& text) {
    TrieNode* current = root;
    for (int i = 0; i < text.size(); ++i) {
        int index = text[i] - 'a';
      
        while (!current->children[index] && current != root) {
            current = current->fail;
        }
      
        if (current->children[index]) {
            current = current->children[index];
        } else {
            current = root;
        }
      
        TrieNode* p = current;
        while (p != root) {
            if (p->isEnd) {
                cout << "Found pattern ending at position " << i 
                     << ", length " << p->length << endl;
            }
            p = p->fail;
        }
    }
}
```

## 时间复杂度分析

- 构建Trie树：O(ΣL)，其中Σ是字符集大小，L是所有模式串的总长度
- 构建失败指针：O(ΣL)
- 匹配过程：O(n)，n是文本长度

## 应用场景

1. 敏感词过滤
2. 病毒特征码检测
3. 生物信息学中的DNA序列匹配
4. 网络入侵检测

## 优化方向

1. **双数组Trie**：减少内存使用
2. **路径压缩**：合并相同后缀的节点
3. **并行化处理**：利用多核CPU加速匹配

AC自动机是处理多模式串匹配问题的高效算法，在实际工程中有广泛应用。