这道题也是链表题里常见的一种，由于咱们之前已经遇到了“套圈”问题，即快慢指针的问题，所以这道题也就基本没啥难度了。

我们先回顾一下这道题里的一个基本知识点：删除链表节点

```cpp
|1|-| --> |2|-| --> |3|-| --> NULL
           ^
       若想删除该节点
// [1]指针的办法 （国内教科书版本）
     /----------\
|1|-|  |2|-| --> |3|-| --> NULL
 ^                ^
 node1->next = node3;
 node1->next = node2->next;
 node1->next = node1->next->next;   // all the same

// [2]指针的指针 （MIT nodes）
|1|-| --> |2|-| --> |3|-| --> NULL
|1|-| --> |3|-| --> NULL
// 如何办到的？
ListNode **del = &node2;
*del = (*del)->next; // 直接替换
```

上述两个版本哪种比较好？第一个版本非常适合教学，因为一目了然，而第二个版本却更加实用。

考虑一下，第二个版本需要参与的指针只有两个：2，3. 而第一个版本呢？ 1，2，3 一个都逃不掉。

-----

回到这道题，如果使用第一个版本，势必需要在 head 前面加一个节点。我也试过这个办法，同样可以AC。但空间复杂度不言而喻。

如果使用第二个版本呢？就无需加什么节点。弄两个指针搞定：

```cpp
ListNode **del = &head, *iter = head; // 第一个指针用于删除， 第二个指针用于迭代。
```

-----

这道题的第二个诀窍在于快慢指针的使用，前面遇到同类型的题，原理是类似的。

咋一想，删除的是倒数第N个节点，擦，链表啊，一次迭代的情况下，怎么知道长度？怎么知道是哪个节点？

不知道大家玩过游标卡尺没有，原理如下：

```cpp
// 假设 n=3, 我们做一个长度为3的游标。你会发现，游标的头指向的节点，就是倒数第n个节点。
 _ _ _ _ _ _ _ _ _
|1|2|3|4|5|6|7|8|9|
             ^   ^
            |7|8|9|
```

有没有顿悟？先让快指针跑，隔n回合后，再让慢指针跑，当然这里快慢指针的频率是一样的（应该叫先后指针）。

当快指针到达终点的时候，慢指针指向的就是我们要删除的节点了。

还可以怎样？

让快指针先跑n回合，然后快指针和慢指针同时跑，当快指针到达终点时，慢指针指向的就是我们要删除的节点了。

我的代码就是基于后一种方式，聪明的你，一定可以同样写出前一种方式的。
