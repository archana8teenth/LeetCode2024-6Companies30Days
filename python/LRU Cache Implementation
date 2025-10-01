import collections

class DNode:
    """Node for the doubly linked list, storing key and value."""
    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    """
    Implements a Least Recently Used (LRU) Cache.
    Uses a dictionary for O(1) lookups and a doubly linked list for O(1)
    ordering/eviction.
    """
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {} 
        
        # Sentinel head and tail nodes to simplify edge cases
        self.head = DNode()
        self.tail = DNode()
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove_node(self, node: DNode):
        """Removes a node from the linked list."""
        p = node.prev
        n = node.next
        p.next = n
        n.prev = p

    def _add_to_head(self, node: DNode):
        """Adds a node right after the head (making it the MRU)."""
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node

    def get(self, key: int) -> int:
        """Retrieves a value and updates its usage status."""
        if key not in self.cache:
            return -1
        
        node = self.cache[key]
        self._remove_node(node)
        self._add_to_head(node)
        
        return node.value

    def put(self, key: int, value: int) -> None:
        """Inserts or updates a value, handling eviction if capacity is met."""
        if key in self.cache:
            node = self.cache[key]
            node.value = value
            self._remove_node(node)
            self._add_to_head(node)
        else:
            new_node = DNode(key, value)
            self.cache[key] = new_node
            self._add_to_head(new_node)
            
            if len(self.cache) > self.capacity:
                lru_node = self.tail.prev
                self._remove_node(lru_node)
                del self.cache[lru_node.key]

# Example Usage:
# lRUCache = LRUCache(2)
# lRUCache.put(1, 1)        # cache is {1: 1}
# lRUCache.put(2, 2)        # cache is {1: 1, 2: 2}
# print(lRUCache.get(1))    # returns 1 (MRU updated to 1)
# lRUCache.put(3, 3)        # LRU key was 2, evicts 2. cache is {1: 1, 3: 3}
# print(lRUCache.get(2))    # returns -1 (not found)
# lRUCache.put(4, 4)        # LRU key was 1, evicts 1. cache is {3: 3, 4: 4}
# print(lRUCache.get(1))    # returns -1 (not found)
# print(lRUCache.get(3))    # returns 3 (MRU updated to 3)
# print(lRUCache.get(4))    # returns 4 (MRU updated to 4)

if __name__ == "__main__":
    cache = LRUCache(2)
    cache.put(1, 1)
    cache.put(2, 2)
    print(f"Get 1: {cache.get(1)}")
    cache.put(3, 3)
    print(f"Get 2 (should be -1, evicted): {cache.get(2)}")
    cache.put(4, 4)
    print(f"Get 1 (should be -1, evicted): {cache.get(1)}")
    print(f"Get 3: {cache.get(3)}")
