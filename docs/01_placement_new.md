## placement new

```cpp
// skiplist.h

template <typename Key, class Comparator>
typename SkipList<Key, Comparator>::Node* SkipList<Key, Comparator>::NewNode(
      const Key& key, int height) {
    char* const node_memory = arena_->AllocateAligned(
        sizeof(Node) + sizeof(std::atomic<Node*>) * (height - 1));
    return new (node_memory) Node(key);   // 此处用了placement new技巧
}

```

new operator是new操作符

A *a = new A()

实际执行：

1. 调用operator new 分配内存， operator new(sizeof(A))

2. 调用构造函数生成类对象，A::A()

3. 返回相应指针

如果class A重载了operator new, 那么将调用A::operator new(size_t), 否则调用c++默认提供的 全局::operator new(size_t)

## operator new

```cpp
void* operator new (std::size_t size) throw (std::bad_alloc);
void* operator new (std::size_t size, const std::nothrow_t& nothrow_constant) throw();
void* operator new (std::size_t size, void* ptr) throw();
```

## placement new

在某些特殊情况下，可能需要在已分配的特定内存创建对象

普通new : A* p = new A 

placement new : A* p = new (ptr)A

注意:

1. 可在stack生成，也可在heap上

2. 定位生成对象时，会自动调用class A的构造函数，但是对象的空间不会自动释放（实际上借用别人的空间），必须显式调用类的析构函数



## reference 

https://zhuanlan.zhihu.com/p/354046948

https://blog.csdn.net/caoshangpa/article/details/78876443