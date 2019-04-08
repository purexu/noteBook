### 链表（C语言描述）

---

#### 单链表：

```c
#include <stdio.h>
#include <malloc.h>
#include <stdlib.h>

#define true 1
#define false 0

typedef int bool; 
typedef int ElemType;
//线性表的单链存储结构
typedef struct Node
{
	ElemType data;
	struct Node *next;
 } Node, *LinkList;

bool GetElem(LinkList L, int i, ElemType *e);

bool insert(LinkList L, int i, ElemType e);

bool deleted(LinkList L, int i, ElemType *e);

LinkList create(LinkList L, int n);

bool clear(LinkList L); 

void show(LinkList L);
 
int main(void)
{
	LinkList list;
	ElemType i;
	list = create(list, 5);
	show(list);
	GetElem(list, 5, &i);
	printf("%d", i);
	insert(list, 3, 99);
	printf("\n插入后的链表："); 
	show(list);
	deleted(list, 3, &i);
	printf("\n删除后的元素：%d 删除后的链表：", i);
	show(list);
	return 0;
}

//返回链表中指定位置的值。 
bool GetElem(LinkList L, int i, ElemType *e)
{
	int j;
	LinkList p;
	//初始化结构体指针结点，把第一个有效结点的地址赋值给结构体指针p 
	p = L->next;
	j = 1;
	//该循环结束后指向的结点位置和j的值相同。 
	while (p && j < i)
	{
		//第一次循环的时候结构体指针p指向第二个有效结点。 
		p = p->next;
		//第一次循环的时候j等于2。 
		++j;
	}
	//当指向目标的结点为空或者移动的位置大于目标位置，则返回false. 
	if (!p || j > i)
		return false;
	*e = p->data;
	return true; 
 } 

//往链表的指定位置插入一个元素。 
bool insert(LinkList L, int i, ElemType e)
{
	int j;
	LinkList p, s, q;
	//初始化p结构体指针结点，把头结点的地址赋值给结构体指针p 
	p = L;
	//初始化移动位置为1 
	j = 1;
	while (p && j < i)
	{
		//这里做个赋值，当下面的while循环判断空时，可以获得最后一个结点。 
		q = p;
		//第一次移动时，结构体指针p指向第一个结点 
		p = p->next;
		//当移动到最后一个结点的后面时，创建一个新的结点，把它塞到最后一个结点后面。 
		while(!p)
		{
			s = (LinkList)malloc(sizeof(Node));
			s->data = e;
			s->next = p->next;
			p->next = s;
			return true; 
		}
		//第一次移动时，移动位置等于2。 
		++j;
	} 
	
	if (!p || j > i)
		return false;
	s = (LinkList)malloc(sizeof(Node));
	s->data = e;
	s->next = p->next;
	p->next = s;
	return true;
}

//将链表的指定位置删除一个元素 
bool deleted(LinkList L, int i, ElemType *e)
{
	int j;
	LinkList p, q, l;
	p = L;
	j = 1;
	//这里和插入有所不同，因为该while循环是将p指针遍历到目标结点的前一个位置，那么如果前前一个位置的下一个结点为空，即要删除的结点，则直接返回false。 
	while(p->next && j < i)
	{
		l = p;
		p = p->next;
		while(!p)
		{
			*e = l->data;
			free(l);
			return true;
		}
		++j;
	 } 
	 if (!(p->next) || j > i)
	 	return false;
	q = p->next;
	p->next = q->next;
	*e = q->data;
	free(q);
	return true; 
 } 
 
//单链表的整表创建
LinkList create(LinkList L, int n)
{
	LinkList p, q;
	int i;
	srand(time(0));
	L = (LinkList)malloc(sizeof(Node));
	L->next = NULL;
	q = L;
	for (i = 0; i < n; i++)
	{
		p = (LinkList)malloc(sizeof(Node));
		p->data = rand()%100 + 1;
		q->next = p;
		q = p;
	}
	q->next = NULL;
	return L;
 } 

//单链表的整表删除
bool clear(LinkList L)
{
	LinkList p, q;
	p = L->next;
	while(!p)
	{
		q = p;
		p = p->next;
		free(q);
	}
	L->next = NULL;
	return true;
 }

//显示链表中所有的元素
void show(LinkList L)
{
	LinkList p;
	p = L->next;
	while(p)
	{
		printf("%d ", p->data);
		p = p->next;
	}
	printf("\n");
 } 
```

**要点：**

1. 线性表由一个个的结点组成，结点的结构体由两个成员组成，即一个数据域和一个指针域。

2. 创建一个线性表分以下几步：

   1. 用malloc函数动态分配一个结点类型的空间，把该空间的作为头结点。
   2. 用for循环自定义创建多个结点，创建的每个结点都要确定指针域指向。可以使用头插法和尾插法。
   3. 将头结点指针返回给函数调用者。

3. 线性表元素的添加和删除：通过while循环遍历找到第 i - 1 个结点。删除要注意判断第 i - 1 个结点后面是否为空。

---

#### 循环链表：

​	循环链表指一个链表的尾节点的指针域不再为空，而是指向头结点。这时候通过rear->next代表头结点，用rear->next->next代表首结点。

---

#### 双向链表：

​	双向链表是在单链表的每个结点中，再设置一个指向其前驱结点的指针域。所以在双向链表中的结点都有两个指针域，一个指向直接后继，另一个指向直接前驱。