![Alt text](trie.png)
![Alt text](trie-search.png)
![Alt text](trie-example.gif)

[208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

```go
type Trie struct {
    children [26]*Trie
    isEnd bool
}


func Constructor() Trie {
    return Trie{}
}


func (this *Trie) Insert(word string)  {
    node := this
    for _, ch := range word {
        ch -= 'a'
        if node.children[ch] == nil {
            node.children[ch] = &Trie{}
        }
        node = node.children[ch]
    }
    node.isEnd = true
}


func (this *Trie) Search(word string) bool {
    node := this
    for _, ch := range word {
        ch -= 'a'
        if node.children[ch] == nil {
            return false
        }
        node = node.children[ch]
    }
    return node.isEnd
}


func (this *Trie) StartsWith(prefix string) bool {
    node := this
    for _, ch := range prefix {
        ch -= 'a'
        if node.children[ch] == nil {
            return false
        }
        node = node.children[ch]
    }
    return true
}


/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */
```
[211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

```go

```


The time and space complexity of operations in a Trie data structure are as follows:

Insertion: The time and space complexity for inserting a word into a Trie is O(k), where k is the length of the word. This is because in the worst case, we need to add k nodes to the Trie, which takes O(k) time and space.

Search: The time complexity for searching a word in a Trie is also O(k), where k is the length of the word. This is because in the worst case, we need to traverse down the Trie until we reach the end of the word. The space complexity for searching is O(1), as we're not using any additional space proportional to the input.

Prefix Search: The time complexity for searching a prefix in a Trie is O(k), where k is the length of the prefix. Similar to the search operation, we need to traverse down the Trie until we reach the end of the prefix. The space complexity is O(1).

The space complexity of a Trie in general is O(n), where n is the total number of characters in all words inserted into the Trie. This is because each node in the Trie represents a character.