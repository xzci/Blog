# 基本数据结构

    这是因为对可能成为**API调用者**的恐惧而创建的项目
    使用C作为编程的语言，为了全部都由自己完成
    理解数据结构

代码主要使用的这本书：
>Mastering Algoritbms with C

通过反复的练习，达到自己手写的程度（白板写代码）
每个数据结构的实现必须熟悉（最为基本的实现）虽然C++或者其他语言都有这相应的的基本实现，可以直接调用使用，（调用库函数）但是还是应该能够直接手写出代码。以其获得更为具体的优化。（能所不能）
相应的，要理解每个数据结构的优缺点。

## 展开

### 链表系

以后自己的代码也需要这样的设计。建立一个良好的接口，增加程序代码的复用。
比如在上记得代码中，对链表这个数据结构进行了抽象，相同的结构如：

* 插入
* 删除
* 头节点
* 尾结点
* 链表长度
* 链表初始化
* 链表销毁

这些地方这3种数据结构操作相似。可以进行抽象，在其他提供类的语言中，这些可以通过类进行继承，以期实现代码的复用。

对于工作之中，使代码多结构化可以减少需求变更的情况下，尽可能的减少修改代码的行数。
比如，链表实现的代码中，需要添加一个功能，那么可以只需要在list中实现这个功能，其他数据结构就可以通过相应的参数，return给相关的函数。
另外比如需要修改插入函数，那么仅仅只需要在list函数中就行修改就可以保证，剩下的数据结构同时被修改。

这里的数据结构可以替换成其他工作中需要实现的功能等。结果同样。

### C语言编程细节

注意因为C语言没有bool类型。所以在返回值的时候，会出现 **-1， 0， 1**等多种情况。因为在C中True 是任意的非0数字，只有0是false, 所以在写自己的函数的时候，要注意自己的函数应该返回状态码（比如一些函数错误结束后的返回-1），还是用于if判断的（0，1）

## 链表系

### 单链表

最为简单的数据结构，每个节点为一个自己定义的结构（包含数据，指针 **指向下一个节点**）
因为只保存了下一个节点的地址，所以单链表是不可以倒序遍历的，也不能随机访问。必须由头节点逐个依次的访问。
    如果每个下一个节点有重复则称之为**循环链表** <-判断是否成环也是相应的算法题之一，并且我觉得也是一些有用的算法题，需要特别记忆的。
特别的：

* 每个节点的位置是离散的（后一个节点的内存位置并不是在节点内存的下一个） 这种分散的特性，相比数组的连续内存有些时候会更加高效。
* 如果发生删除操作一定要先获取以后节点的地址

如果插入、删除的__位置为NULL__，则默认插入、删除的位置在表头。

```C 节点定义
typedef struct ListElmt_
{
    void *data; // can use point to get the real data
    struct ListElmt_ * next;
} ListElmt;

typedef struct List_
{
    int size;
    int(*match)(const void *key1, const void *key2);
    void(*destroy)(void *data);
    ListElmt *head;
    ListElmt *tail;
} List;
```

然后定义相关的函数： 增删查改

```C 接口函数
void list_init(List *list, void(*destroy)(void *data));
void list_destroy(List *list);

// should use bool but C do not give bool before C99
int list_ins_next(List *list, ListElmt *element, const void * data);
/* when you want to remove a element, you should give the value to the programer
/ because they want to use this data, you can not just delete the element and do nothing
/ also the element can be return as return value, but in this way, you can not judge
/ if it has been remove already */
int list_rem_next(List *list, ListElmt *element, void **data);

// in other language you should use inline function instead of using macro
#define list_size(list) ((list)->size)
#define list_head(list) ((list)->head)
#define list_tail(list) ((list)->tail)
#define list_is_head(list, element) ((element) == (list)->head ? 1 : 0)
#define list_is_tail(element) ((element)->next == NULL ? 1 : 0)
#define list_data(element) ((element)->data)
#define list_next(element) ((element)->next)
```

```C 函数实现
void list_init(List *list, void(*destroy)(void *data))
{
    list->size = 0;
    list->destroy = destroy;
    list->head = NULL;
    list->tail = NULL;
}

void list_destroy(List *list)
{
    void *data;
    while (list_size(list) > 0)
    {
        if (list_rem_next(list, NULL, (void **)&data) == 0 && list->destory != NULL)
        {

            list->destory(data);
        }
    }
    memset(list, 0, sizeof(List));
}

int list_ins_next(List *list, ListElmt *element, const void * data)
{
    ListElmt * new_element;
    if ((new_element = (ListElmt *)malloc(sizeof(ListElmt))) == NULL)
        return -1;
    new_element->data = (void *)data;
    // if element is null insert the data in the head of list
    if (element == NULL)
    {
        if (list_size(list) == 0)
            list->tail = new_element;
        new_element->next = list->head;
        list->head = new_element;
    }
    else
    {
        if (element->next == NULL)
            list->tail = new_element;
        // here you can use new_element->next = NULL
        new_element->next = element->next;
        element->next = new_element;
    }
    list->size++;
    return 0;
}

int list_rem_next(List *list, ListElmt *element, void **data)
{
    ListElmt *old_element;
    if (list_size(list) == 0)
        return -1;
    // if element is null remove the data in the head of list
    if (element == NULL)
    {
        *data = list->head->data;
        old_element = list->head;
        list->head = list->head->next;

        if (list_size(list) == 1)
            list->tail = NULL;
    }
    else
    {
        if (element->next == NULL)
            return -1;
        *data = element->next->data;
        old_element = element->next;
        // you can use element->next = old_element->next;
        element->next = element->next->next;

        if (element->next == NULL)
            list->tail = NULL;
    }
    free(old_element);
    list->size--;
    return 0;
}
```

当然， 我们应该还要编写相应的find函数，destroy函数，来使得这个链表程序更加的完整。

也可以类比其他书籍查看不同，比如
> 数据结构与算法分析 C语言描述

就可以明白在不同的编程思路下，虽然是同一数据结构，但是代码可能会完全不同。具体喜欢哪种风格，那还是需要自己见仁见智了。

### 栈

栈的实现方式很简单，可以通过很多的方式来实现，栈只要保证操作只在头部就可以称之为栈。这里栈的实现是利用先前做出的假设，**如果插入位置为空的话将操作位置设定为表的头部**

```C 节点定义
typedef List Stack;
```

```C 接口函数
#define stack_init list_init
#define stack_destroy list_destroy

#define stack_peek(stack) ((stack)->head == NULL ? NULL : (stack)->head->data)
#define stack_size list_size

int stack_push(Stack *stack, const void *data);
int stack_pop(Stack *stack, void **data);
```

```C 函数实现
int stack_push(Stack *stack, const void *data)
{
    return list_ins_next(stack, NULL, data);
}

int stack_pop(Stack *stack, void **data)
{
    return list_rem_next(stack, NULL, data);
}
```

### 队列

队列也可以看作特殊的链表，操作分别在头尾两端。在头部进行删除，在尾部进行插入。
同栈一样，没有操作内部节点的接口。

```C 节点定义
typedef List Queue;
```

```C 接口函数
#define queue_init list_init
#define queue_destroy list_destroy

#define queue_peek(queue) ((queue)->head == NULL ? NULL : (queue)->head->data)
#define queue_size list_size

int queue_enqueue(Queue *queue, const void *data);
int queue_dequeue(Queue *queue, void **data);
```

```C 函数实现
int queue_enqueue(Queue *queue, const void *data)
{
    return list_ins_next(queue, list_tail(queue), data);
}
int queue_dequeue(Queue *queue, void **data)
{
    return list_rem_next(queue, NULL, data);
}
```

栈和队列可以使用不同的实现方式，比如链表，或者是可以增长的数组。简单的思路如下：
如果对于数据的插入，删除的个数确定，可以简单的使用数组进行实现，（惰性删除即可，或直接在建立的初期为以后的插入位置进行预留等其他的实现方式），或者更进一步，使用其他语言提供的库函数进行封装。
另外，数据量确定和可读性起见，可以直接在结构体中包含所需要的数据。这时，可以建立单独的带有类型的链表，如int类型的链表，char类型的链表等。

上述实现是通过链表系作为接口，修改而成的。如果单纯只使用其中一种数据结构，可以针对这种结构单独写出。

### 集合

集合是一种很特殊的数据结构，（set），它所拥有的性质**元素唯一性**，可以为编程提供很大的便利。比如去重可以直接将数据存入到set就可以了。

```C 节点定义
typedef List Set;
```

```C 接口函数
void set_init(Set *set, int(*match)(const void *key1, const void *key2), void(*destroy)(void *data));
int set_insert(Set *set, const void *data);
int set_remove(Set *set, void **data);
int set_union(Set *setu, const Set *set1, const Set *set2);
int set_intersction(Set *seti, const Set *set1, const Set *set2);
int set_difference(Set *setd, const Set *set1, const Set *set2);
int set_is_member(const Set *set, const void *data);
int set_is_subset(const Set *set1, const Set *set2);
int set_is_equal(const Set *set1, const Set *set2);

#define set_size(set) ((set)->size)
#define set_destroy list_destroy
```

```C 函数实现
void set_init(Set *set, int(*match)(const void *key1, const void *key2), void(*destroy)(void *data))
{
    list_init(set, destroy);
    set->match = match;
}

int set_insert(Set *set, const void *data)
{
    if (set_is_member(set, data))
        return 1;
    return list_ins_next(set, list_tail(set), data);
}

int set_remove(Set *set, void **data)
{
    ListElmt *member;
    ListElmt *prev;

    prev = NULL;

    for (member = list_head(set); member != NULL; member = list_next(member))
    {
        if (set->match(*data, list_data(member)))
            break;
        prev = member;
    }
    if (member == NULL)
        return -1;

    return list_rem_next(set, prev, data);
}

int set_union(Set *setu, const Set *set1, const Set *set2)
{
    ListElmt *member;
    void *data;

    set_init(setu, set1->match, NULL);
    for (member = list_head(set1); member != NULL; member = list_next(member))
    {
        data = list_data(member);
        if (list_ins_next(setu, list_tail(setu), data) != 0)
        {
            set_destroy(setu);
            return -1;
        }
    }
    for (member = list_head(set2); member != NULL; member = list_next(member))
    {
        if (set_is_member(set1, list_data(member)))
        {
            continue;
        }
        else
        {
            data = list_data(member);
            if (list_ins_next(setu, list_tail(setu), data) != 0)
            {
                set_destroy(setu);
                return -1;
            }
        }
        return 0;
    }
}

int set_intersection(Set *seti, const Set *set1, const Set *set2)
{
    ListElmt * member;
    void *data;

    set_init(seti, set1->match, NULL);
    for (member = list_head(set1); member != NULL; member = list_next(member))
    {
        if (set_is_member(set2, list_data(member)))
        {
            data = list_data(member);
            if (list_ins_next(seti, list_tail(seti), data) != 0)
            {
                set_destroy(seti);
                return -1;
            }
        }
    }
    return 0;
}

int set_difference(Set *setd, const Set *set1, const Set *set2)
{
    ListElmt *member;
    void *data;

    set_init(setd, set1->match, NULL);

    for (member = list_head(set1); member != NULL; member = list_next(member))
    {
        if (!set_is_member(set2, list_data(member)))
        {
            data = list_data(member);
            if (list_ins_next(setd, list_tail(setd), data) != 0)
            {
                set_destroy(setd);
                return -1;
            }
        }
    }
    return 0;
}

int set_is_member(const Set *set, const void *data)
{
    ListElmt *member;
    for (member = list_head(set); member != NULL; member = list_next(member))
    {
        if (set->match(data, list_data(member)))
            return 1;
    }
    return 0;
}

int set_is_subset(const Set *set1, const Set *set2)
{
    ListElmt *member;
    if (set_size(set1) > set_size(set2))
        return 0;
    for (member = list_head(set1); member != NULL; list_next(member))
    {
        if (!set_is_member(set2, list_data(member)))
            return 0;
    }
    return 1;
}

int set_is_equal(const Set *set1, const Set *set2)
{
    if (set_size(set1) != set_size(set2))
        return 0;
    return set_is_subset(set1, set2);
}
```

## 树系