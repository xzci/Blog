# 基本数据结构

    这是因为对可能成为**API调用者**的恐惧而创建的项目
    使用C作为编程的语言，为了全部都由自己完成
    理解数据结构

代码主要使用的这本书：
>Mastering Algoritbms with C

通过反复的练习，达到自己手写的程度（白板写代码）
每个数据结构的实现必须熟悉（最为基本的实现）虽然C++或者其他语言都有这相应的的基本实现，可以直接调用使用，（调用库函数）但是还是应该能够直接手写出代码。以其获得更为具体的优化。（能所不能）
相应的，要理解每个数据结构的优缺点。

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
    void *data; // 数据可以通过指针传入
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

这里栈的实现是利用

- [ ] adasd