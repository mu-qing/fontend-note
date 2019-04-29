**栈**

栈【数组】

```
function Stack() {
    this.size = 0;
    this.list = [];
}

// 向后插入一个元素O(1)
Stack.prototype.push = function(ele) {
    this.list[this.size++] = ele;
}

// 向后删除一个元素O(1)
Stack.prototype.pop = function() {
    if (!this.list.length) return;
    var deleteData = this.list[this.size];
    this.size = --this.list.length;
    return deleteData;
}
```





栈【对象】









 





```
function Stack() {
    this._size = 0;
    this._storage = {};
}
 
Stack.prototype.push = function(data) {
    var size = ++this._size;
    this._storage[size] = data;
};
 
Stack.prototype.pop = function() {
    var size = this._size,
        deletedData;
 
    if (size) {
        deletedData = this._storage[size];
 
        delete this._storage[size];
        this._size--;
 
        return deletedData;
    }
};
```





栈【链表】









 





```
class Node {
    constructor(ele) {
        this.element = ele
        this.next = null;
    }
}
class StackBasedOnLinkedList {
    constructor() {
        this.top = null; // 栈顶
    }
    push(ele) {
        const node = new Node(ele);
        if (this.top) { // 栈顶指向最新的元素，所以新元素指向以前的元素
            node.next = this.top
            this.top = node
        } else {
            this.top = node;
        }
    }
    pop() {
        if (this.top === null) return -1;
        const value = this.top.element
        this.top = this.top.next;
        return value
    }
    clear() {
        this.top = null;
    }
    display() {
        if (this.top !== null) {
            let temp = this.top
            while (temp !== null) {
                temp = temp.next
            }
        }
    }
}               
```





基于上面的栈链表，实现的浏览器功能









 





```
class SampleBrowser {
constructor(ele) {
    this.normalStack = new StackBasedOnLinkedList();
    this.backStack = new StackBasedOnLinkedList();
}

// 正常浏览页面
pushNormal(name) {
    this.normalStack.push(name);
    this.backStack.clear();
}

// 后退
goBack() {
    const value = this.normalStack.pop()
    if (value !== -1) {
        this.backStack.push(value);
    } else {
        console.log('无法后退')
    }
    
}

// 前进
goFront() {
    const value = this.backStack.pop()
    if (value !== -1) {
        this.normalStack.push(value);
    } else {
        console.log('无法前进')
    }
    
}

// 打印栈内数据
displayAllStack() {
    console.log('---后退页面---')
    this.backStack.display()
    console.log('---浏览页面---')
    this.normalStack.display()
}
}
let browser = new SampleBrowser();
browser.pushNormal('www.google.com')
browser.pushNormal('www.baidu.com')
browser.pushNormal('www.github.com')
// 后退
browser.goBack()
browser.goBack()
browser.goFront()
console.log(browser);
```













队列









 





```
function Queue() {
    this.size = 0;
    this.list = [];
}

// 向后插入一个元素
Queue.prototype.enqueue = function(ele) {
    this.list[this.size++] = ele;
}

// 从首位删除一个元素
Stack.prototype.dequeue = function(ele) {
    var deleteData = this.list[0];
    for (var i = 0; i< this.size; i++) {
        this.list[i] = this.list[i+1];
    }
    this.size = --this.list.length;
    return deleteData;
}

```































队列【对象】









 





```
// 先进先出
function Queue() {
    this._oldestIndex = 1;
    this._newestIndex = 1;
    this._storage = {};
}
 
Queue.prototype.size = function() {
    return this._newestIndex - this._oldestIndex;
};
 
Queue.prototype.enqueue = function(data) {
    this._storage[this._newestIndex] = data;
    this._newestIndex++;
};
 
Queue.prototype.dequeue = function() {
    var oldestIndex = this._oldestIndex,
        newestIndex = this._newestIndex,
        deletedData;
 
    if (oldestIndex !== newestIndex) {
        deletedData = this._storage[oldestIndex];
        delete this._storage[oldestIndex];
        this._oldestIndex++;
 
        return deletedData;
    }
};
```







队列【链表】









 





```
class Node {
    constructor(ele) {
        this.element = ele
        this.next = null;
    }
}
class QueueBasedOnLinkedList {
    constructor() {
        this.head = null  // 头结点
            this.tail = null  // 尾结点
    }
    // 入队 1 检查队伍是否有元素 若无 就让头结点指向一个新元素
    enqueue(ele) {
        const node = new Node(ele);
        if (this.head) { // 栈顶指向最新的元素，所以新元素指向以前的元素
            this.tail.next = node;
            this.tail = this.tail.next;
        } else {
            this.head = node;
            this.tail = this.head;
        }
    }
    dequeue() {
        if (this.head === null) return -1;
        const value = this.head.element
        this.head = this.head.next;
        return value
    }
    clear() {
        this.tail = null;
        this.head = null;
    }
}

                // Test
const newQueue = new QueueBasedOnLinkedList()
// 插入元素
newQueue.enqueue(1)
newQueue.enqueue(2)
newQueue.enqueue(3)
newQueue.dequeue()
console.log(newQueue);
```





循环队列









 





```
class Node {
    constructor(element) {
        this.element = element
        this.next = null
    }
}

class CircularQueue {
    constructor() {
        this.head = null
        this.tail = null
    }

    enqueue(value) {
        if (this.head === null) {
            this.head = new Node(value)
            this.head.next = this.head
            this.tail = this.head
        } else {
            const flag = this.head === this.tail
            this.tail.next = new Node(value)
            this.tail.next.next = this.head
            this.tail = this.tail.next
            if (flag) {
                this.head.next = this.tail
            }
        }
    }

    dequeue() {
        if (this.head === this.tail) {
            const value = this.head.element
            this.head = null
            return value
        } else if (this.head !== null) {
            const value = this.head.element
            this.head = this.head.next
            this.tail.next = this.head
            return value
        } else {
            return -1
        }
    }

    display() {
        let res = 0
        console.log('-------获取dequeue元素------')
        while (res !== -1) {
            res = this.dequeue()
            // console.log(res)
        }
    }
}
// Test
const newCircularQueue = new CircularQueue()
// 插入元素
newCircularQueue.enqueue(1)
// newCircularQueue.enqueue(2)
// newCircularQueue.enqueue(3)
console.log(newCircularQueue)
// 获取元素
// newCircularQueue.display()
// newCircularQueue.enqueue(1)
// newCircularQueue.display()
```





冒泡排序









 





```
                var arr = [3, 4, 0, 5];
                var n = arr.length;
                for (var i = 0; i < n; i++) {
                    var flag = false; // 
                    for (var j = 0; j < n - i-1; j ++) {
                        if (arr[j] > arr[j +1]) {
                            let temp = arr[j+1];
                            arr[j+1] = arr[j];
                            arr[j] = temp;
                            flag = true;  // 只要有顺序不对的，这里就会执行，如果没执行，说明是已经排好序了
                        }
                    }
                    if (!flag) break;   
                }
                console.log(arr);
```





第一，冒泡排序是原地排序算法吗？

冒泡的过程只涉及相邻数据的交换操作，只需要常量级的临时空间，所以它的空间复杂度为 O(1)， 是一个原地排序算法。



第二，冒泡排序是稳定的排序算法吗？

在冒泡排序中，只有交换才可以改变两个元素的前后顺序。为了保证冒泡排序算法的稳定性，当有

相邻的两个元素大小相等的时候，我们不做交换，相同大小的数据在排序前后不会改变顺序，所以

冒泡排序是稳定的排序算法。

第三，冒泡排序的时间复杂度是多少？

最好情况下，要排序的数据已经是有序的了，我们只需要进行一次冒泡操作，就可以结束了，所以 最好情况时间复杂度是 O(n)。而最坏的情况是，要排序的数据刚好是倒序排列的，我们需要进行 n 次冒泡操作，所以最坏情况时间复杂度为 O(n )。



































![img](file:///C:/Users/刘静怡/Documents/My Knowledge/temp/5b0a02d0-aeb8-43d4-be74-3c3e5b983104/128/index_files/H{IZ@5@C(6Z_NZAMPP4BVGS.png)

![img](file:///C:/Users/刘静怡/Documents/My Knowledge/temp/5b0a02d0-aeb8-43d4-be74-3c3e5b983104/128/index_files/0f3cb698315c25fd0d2d8078d33f194e.png)

![img](file:///C:/Users/刘静怡/Documents/My Knowledge/temp/5b0a02d0-aeb8-43d4-be74-3c3e5b983104/128/index_files/794c502632d9f6e9cd9c54d86d53579e.png)









 





```
                class Node {
                    constructor(ele) {
                        this.element = ele
                        this.next = null;
                    }
                }
                class LinkedList {
                    constructor() {
                        this.head = new Node('head')
                    }
                    // 根据value查找节点 
                    findByElement(ele) {
                        let currentNode = this.head;
                        while (currentNode.element !== ele && currentNode.next !== null) {
                            currentNode = currentNode.next;
                        }
                        if (currentNode.next === null && currentNode.element !==ele) {
                            return -1;
                        } else {
                            return currentNode;
                        }
                    }
                     // 根据index查找节点 

                    findByIndex(index) {
                        let currentNode = this.head;
                        let pos = 0;
                        while (currentNode !== null && pos !== index) {
                            currentNode = currentNode.next
                            pos++
                        }
                        return currentNode === null ? -1 : currentNode
                    }

                    findPrev(ele) {
                        let currentNode = this.head
                        while (currentNode.next !== null && currentNode.next.element !== ele) {
                            currentNode = currentNode.next
                        }
                        if (currentNode.next === null) {
                            return -1
                        }
                        return currentNode
                    }
                    
                    remove(ele) {
                        const currentNode = this.findByElement(ele);
                        const prevNode = this.findPrev(ele);
                        if (currentNode === -1) {
                            console.log('未找到删除元素')
                            return
                        }
                        currentNode 
                    }

                    insert(newElement, element) {
                        const currentNode = this.findByElement(element)
                        if (currentNode === -1) {
                            console.log('未找到插入位置')
                            return
                        }
                        const newNode = new Node(newElement);
                        newNode.next = currentNode.next;
                        currentNode.next = newNode
                    }

                    reverseList() {
                        const root = new Node('head')
                        // console.log(this.head);
                        let currentNode = this.head.next
                        // console.log(currentNode);
                        while (currentNode !== null) {
                            const next = currentNode.next
                            // console.log('111');
                            // console.log(next);
                            currentNode.next = root.next
                            root.next = currentNode
                            // console.log(currentNode);
                            currentNode = next
                        }
                        this.head = root
                    }
                }

                let linkList1 = new LinkedList();
                linkList1.insert('chen1', 'head');
                linkList1.insert('chen2', 'chen1');
                linkList1.insert('chen3', 'chen2');
                let a = linkList1.findByIndex(0);
                console.log(a);
```

