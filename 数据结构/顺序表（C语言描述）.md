### 顺序表（C语言描述）

```c
# include <stdio.h>
# define MAXSIZE 20 //这里通过宏定义来给数组分配空间
# define true 1;
# define false 0;
typedef int bool;	//因为在C语言标准（C99）中并没有bool类型，这里只能把int型看作bool型。
typedef int ElemType; 
typedef struct
{
	ElemType data[MAXSIZE];	//数组总大小 
	int length;	//数组实际元素个数 
 } SqList;	//声明一个结构体 

//初始化数组 
void init_array(SqList *array);
 
//判断数组是否为空
bool is_empty(SqList *array);
 
//判断数组是否已满
bool is_full(SqList *array);
 
//给数组从后叠加元素 
bool append(SqList *array, int val);
 
//给数组插入一个元素
bool insert(SqList *array, int pos, int val);

//给数组删除一个元素
bool deleted(SqList *array, int pos, int *val);
 
//显示数组中所有的元素
void show(SqList *array);
 
int main(void)
{
	SqList array;
	init_array(&array);
	append(&array, 1);
	append(&array, 2);
	append(&array, 3);
	append(&array, 4);
	append(&array, 5);
	append(&array, 6);
	append(&array, 7);
	printf("添加数据后的数组：\n");
	show(&array); 
	printf("插入数据后的数组：\n");
	insert(&array, 8, 9);
	show(&array);
	int i;
	deleted(&array, 8, &i);
	printf("删除数据后的数组：删除的数据是：%d\n", i);
	show(&array);
}

void init_array(SqList *array)
{
	array->length = 0;
 } 

bool is_empty(SqList *array)
{
	if (array->length == 0)
	{
		printf("数组为空！\n");
		return true;
	}
	else
		return false;
 }

bool is_full(SqList *array)
{
	if (array->length == MAXSIZE)
	{
		printf("数组已满！\n");
		return true;
	}
	else
		return false;
 } 

bool append(SqList *array, int val)
{
	if(is_full(array))
		return false;
	
	array->data[array->length] = val;
	array->length++;
	return true;
 }

bool insert(SqList *array, int pos, int val)
{
	//在C语言标准中不允许在for循环中定义变量。 
	ElemType i;
	
	//满时直接返回false 
	if(is_full(array))	
		return false;

	//当插入位置不在范围时 
	if(pos < 1 || pos > array->length + 1)	
		return false; 
	 
	//当插入位置不在最后一个元素的后面时。 
	if(pos <= array->length)
	{
		for(i = array->length - 1 ; i >= pos - 1; i--)
		array->data[i + 1] = array->data[i];
	 } 
	 
	array->data[pos - 1] = val;
	(array->length)++;
	return true;
 } 

bool deleted(SqList *array, int pos, ElemType *val)
{
	ElemType i;
	 
	//空时直接返回false 
	if (is_empty(array))	
		return false;
	
	//要删除的元素位置不在范围。	
	if (pos < 1 || pos > array->length)	
		return false;
	
	//备份一波要删除的元素
	*val = array->data[pos - 1];
	
	//如果要删除的不是最后位置。 
	if(pos < array->length)
	{
		for (i = pos; i < array->length; i++)
		array->data[i - 1] = array->data[i];	
	 } 
	array->length--;
	return true;
 } 

void show(SqList *array)
{
	if (is_empty(array))
	;
	
	int i;
	for(i = 0 ; i < array->length; i++)
	{
		printf("%d ", array->data[i]);
	}
	printf("\n");
 } 
```

**要点：**

1. 做插入和删除时要先判断表是否为空，再判断插入或删除的位置的范围是否正确。这里尤其注意判断边界值：如插入的位置是最后一个元素的后面，删除的位置是最后一个元素。这两个位置的元素插入和删除的时间复杂度是O(1)。
2. 在C语言标准（C99）中是没有bool类型的。
3. C语言中通过结构名获取成员变量只需要通过 **结构名.成员名** 。而通过结构体指针则有两种方法：
   1. 结构体指针名->成员名。
   2. （*结构体指针名）.成员名。
4. C语言中，结构体其实就是一个用户自定义的类型，其用法和基本数据类型无区别。