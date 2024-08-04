# 146. LRU Cache

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with positive size capacity.

- `int get(int key)` Return the value of the key if the key exists, otherwise return `-1`.

- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

Example:
```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]


Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

### Idea

What do we need to support?
- A **queue** of key-value pairs
- When a key gets used, move it to the front
  - Need to find the corresponding node in queue in `O(1)`
  - Cannot be achieved with only a queue
  - Separate data structure needed
- When the queue gets full, evict the last node
  - Need to keep track of the **back** of the queue

Data structures:
- A node for each key-value pair
- A doubly-linked list (deque) of nodes
- A hashmap that maps keys to node pointers

### C++ Solution

```cpp
struct Node {
    int key;
    int value;
    Node *prev;
    Node *next;
    Node(int key, int value) : key{key}, value{value}, prev{nullptr}, next{nullptr} {}
    Node(int key, int value, Node *prev, Node *next)
        : key{key}, value{value}, prev{prev}, next{next} {}
};

struct Deque {
    Node *front;
    Node *back;
    int size = 0;
    int capacity;

    Deque(int capacity) : front{nullptr}, back{nullptr}, capacity{capacity} {}

    bool isFull() { return size == capacity; };

    void pushFront(Node *node) {
        node->next = front;
        front = node;
        if (node->next) node->next->prev = node;
        if (back == nullptr) back = node;
        size++;
    }

    Node *erase(Node *node) {
        if (node->next) node->next->prev = node->prev;
        if (node->prev) node->prev->next = node->next;
        if (node == front) front = node->next;
        if (node == back) back = node->prev;
        size--;
        return node;
    }

    void moveToFront(Node *node) {
        // Only one node or already at front. No change needed.
        if (size == 1 || node == front) return;

        // Exchange the nodes.
        if (size == 2) {
            Node *oldFront = front;
            Node *oldBack = back;
            front = oldBack;
            back = oldFront;
            front->next = back;
            front->prev = nullptr;
            back->prev = front;
            back->next = nullptr;
            return;
        }

        // All other cases.
        Node *nodeToMove = erase(node);
        pushFront(nodeToMove);
    }
};

class LRUCache {
    Deque *dq;
    unordered_map<int, Node *> ptrs;

    void evict() {
        Node *victim = dq->erase(dq->back);
        ptrs.erase(victim->key);
    }

    void add(int key, int value) {
        Node *newNode = new Node(key, value);
        dq->pushFront(newNode);
        ptrs[key] = newNode;
    }

    void update(Node *node, int value) {
        node->value = value;
        dq->moveToFront(node);
    }

public:
    LRUCache(int capacity) {
        this->dq = new Deque(capacity);
    }

    int get(int key) {
        auto it = ptrs.find(key);
        if (it == ptrs.end()) return -1;
        dq->moveToFront(it->second);
        return it->second->value;
    }

    void put(int key, int value) {
        if (value == 14) cout << dq->size << endl;
        auto it = ptrs.find(key);
        if (it == ptrs.end()) {
            if (dq->isFull())
                evict();
            add(key, value);
            return;
        }
        update(it->second, value);
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```