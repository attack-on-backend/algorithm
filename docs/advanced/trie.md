# Attack on Algorithm - 前缀树 🐝 

## 定义

**前缀树 :** 又叫字典树 , 顾名思义 , 就是一个像字典一样的树 , 它是字典的一种存储方式 , 字典中的每个单词在字典树中表现为一条从根节点出发的路径 , 路径相连的边上的字母连起来就形成对应的字符串

![trie-01](https://github.com/attack-on-backend/algorithm/blob/master/assert/trie-01.png?raw=true)

上图中 , 这颗前缀树用边来表示字母 , 从根节点到树上某一节点的路径就代表了一个单词 , 比如`1 -> 2 -> 6 -> 10` 表示的就是单词 `"acc"` 

前缀树的本质就是一个用于字符串检索的多叉树 , 利用空间换时间 , 用字符串的公共前缀来降低查询时间的开销 , 最大限度的减少无谓的字符串比较 , 以达到提高效率的目的

## 实现

```python
class Node:
    def __init__(self):
        # 子节点字典
        self.children = {}
        # 标记是否为单词的结尾
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        # 初始化根节点
        self.root = Node()

    def insert(self, word):
        # 从根节点开始
        node = self.root
        for char in word:
            # 如果字符不在子节点中，创建一个新的子节点
            if char not in node.children:
                node.children[char] = Node()
            # 移动到子节点
            node = node.children[char]
        # 标记单词的结尾
        node.is_end_of_word = True

    def search(self, word):
        # 从根节点开始
        node = self.root
        for char in word:
            # 如果字符不在子节点中，返回 False
            if char not in node.children:
                return False
            # 移动到子节点
            node = node.children[char]
        # 检查是否为单词的结尾
        return node.is_end_of_word

    def starts_with(self, prefix):
        # 从根节点开始
        node = self.root
        for char in prefix:
            # 如果字符不在子节点中，返回 False
            if char not in node.children:
                return False
            # 移动到子节点
            node = node.children[char]
        # 如果能够遍历完前缀，返回 True
        return True

if __name__ == '__main__':
    # 示例用法
    trie = Trie()
    trie.insert("apple")
    trie.insert("app")
    trie.insert("banana")

    print(trie.search("apple"))  # 输出: True
    print(trie.search("app"))  # 输出: True
    print(trie.search("appl"))  # 输出: False
    print(trie.search("banana"))  # 输出: True
    print(trie.search("ban"))  # 输出: False
    print(trie.starts_with("app"))  # 输出: True
    print(trie.starts_with("ba"))  # 输出: True
    print(trie.starts_with("ban"))  # 输出: True
    print(trie.starts_with("appl"))  # 输出: True
    print(trie.starts_with("applz"))  # 输出: False
```

## 例题

- [208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)
- [648. 单词替换](https://leetcode.cn/problems/replace-words/)
- [677. 键值映射](https://leetcode.cn/problems/map-sum-pairs/)



