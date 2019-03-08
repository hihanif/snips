# Single Linked List

* avoid repetition of the code (getNode is the key API here)
* think of head as headHolder (reduces all NULL check overhead)
* remember the zero index condition (for loop condition)
* check for boundary condition before you traverse (reduces if check overhead)
 
```java
class MyLinkedList {
    class Node {
        int val;
        Node next;
        Node(int val) {
            this.val = val;
            next = null;
        }
    }
    
    Node headHolder;
    int count;
    
    /** Initialize your data structure here. */
    public MyLinkedList() {
        headHolder = new Node(0);
        count = 0;
    }
    
    private Node getNode(int index) {
        Node curr = headHolder;
        for (int i = 0; i <= index && curr != null; i ++, curr = curr.next);
        return curr;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        // Node curr = headHolder.next;
        // for (int i = 0; i < index && curr != null; i ++, curr = curr.next);
        Node node = getNode(index);
        return (node != null) ? node.val : -1;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        Node newNode = new Node(val);
        newNode.next = headHolder.next;
        headHolder.next = newNode;
        count ++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        Node curr = headHolder;
        while (curr.next != null) {
            curr = curr.next;
        }
        curr.next = new Node(val);
        count ++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if (index > count) return; // dont add
        if (index == 0) { addAtHead(val); return; }
        if (index == count)  { addAtTail(val); return; }

        Node curr = getNode(index - 1);
        if (curr == null) return;
        
        Node newNode = new Node(val);
        newNode.next = curr.next;
        curr.next = newNode;
        count ++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if (index >= count || count == 0) return;
        // Node curr = headHolder;
        // for (int i = 0; i < index && curr != null; i++, curr = curr.next);
        Node curr = getNode(index - 1);
        curr.next = curr.next.next;
        count --;
        return;
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
 ```
 
 # Reference code for DLL
 ```java
 class MyLinkedList {
    class Node {
        int val;
        Node next;
        Node prev;
        Node(int val) {this(val, null, null);}
        Node(int val, Node prev, Node next) {
            this.val = val;
            this.prev = prev;
            this.next = next;
        }
    }
    
    private Node head;
    private Node tail;
    private int size;

    /** Initialize your data structure here. */
    public MyLinkedList() {
        head = new Node(-1);
        tail = new Node(-1);
        head.next = tail;
        tail.prev = head;
    }
    
    private Node getNode(int index) {
        assert index < size;
        Node node = head;
        while(index-- >= 0) {
            node = node.next;
        }
        return node;
    }
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        if(index < size) {
            return getNode(index).val;
        }
        return -1;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        Node node = new Node(val, head, head.next);
        head.next = node;
        node.next.prev = node;
        size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        Node node = new Node(val, tail.prev, tail);
        tail.prev = node;
        node.prev.next = node;
        size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if(index < size) {
            Node n = getNode(index);
            Node node = new Node(val, n.prev, n);
            n.prev.next = node;
            node.next.prev = node;
            size++;
        } else if(index == size) {
            addAtTail(val);
        } else {
            return ;
        }
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if(index < size) {
            Node node = getNode(index);
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
 ```
 