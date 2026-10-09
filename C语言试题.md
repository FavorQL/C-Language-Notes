# C语言基础复习

## 1. 指针数组、数组指针、函数指针、指针函数

### 指针数组
```c
int *arr[5];
```
arr 是一个数组，包含 5 个元素，每个元素都是指向 int 的指针。

### 数组指针
```c
int (*p)[5];
```
p 是一个指针，指向一个包含 5 个 int 的数组。

### 函数指针
```c
int add(int a, int b) { return a + b; }

int (*p)(int, int) = add;
int result = p(2, 3); // 5
```
函数指针指向函数。

### 指针函数
```c
int *get_value(int *p) {
    return p;
}
```
指针函数是返回指针的函数。

| 名称 | 示例 | 含义 |
|---|---|---|
| 指针数组 | `int *arr[5];` | 数组，元素是指针 |
| 数组指针 | `int (*p)[5];` | 指针，指向数组 |
| 函数指针 | `int (*p)(int);` | 指针，指向函数 |
| 指针函数 | `int *fun(void);` | 函数，返回指针 |

## 2. do...while 和 while 的区别

`while` 先判断条件，条件为真才执行，循环体可能执行 0 次。

```c
int i = 5;
while (i < 3) {
    printf("%d\\n", i);
    i++;
}
```

`do...while` 先执行循环体，再判断条件，因此至少执行一次。结尾必须有分号。

```c
int i = 5;
do {
    printf("%d\\n", i);
    i++;
} while (i < 3);
```

## 3. for 循环的执行顺序

```c
for (初始化; 条件; 更新) {
    // 循环体
}
```

执行顺序：
1. 初始化，只执行一次。
2. 判断条件。
3. 条件为真，执行循环体；为假，结束循环。
4. 执行更新表达式。
5. 返回第 2 步。

示例：
```c
for (int i = 0; i < 3; i++) {
    printf("%d ", i);
}
// 输出：0 1 2
```

## 4. 为什么 switch 要加 break

如果某个 `case` 末尾没有 `break`、`return` 等跳转语句，程序可能继续执行后面的分支，这称为贯穿（fall-through）。

```c
switch (n) {
case 1:
    printf("one\\n");
    break;
case 2:
    printf("two\\n");
    break;
default:
    printf("other\\n");
    break;
}
```

通常加 `break` 是为了防止意外执行后续分支。

## 5. 字符串指针代码找错

题目：
```c
char* s = “hello”；
*s=‘H’
```

问题：
1. 使用了中文全角引号和分号，C 语言应使用英文半角符号。
2. 字符串字面量不能被修改，执行 `*s = 'H';` 会导致未定义行为。

正确写法：
```c
char s[] = "hello";
s[0] = 'H';
printf("%s\\n", s); // Hello
```

也可以写 `const char *s = "hello";`，表示不通过 s 修改字符串内容。

## 6. 冒泡排序

冒泡排序就是不断比较相邻的两个数，如果前面的数比后面的大，就交换位置。这样每一趟下来，当前最大的数就会排到后面。

```c
#include <stdio.h>

int main(void)
{
    int a[] = {5, 1, 4, 2, 8};
    int n = sizeof(a) / sizeof(a[0]);

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                int temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
            }
        }
    }

    for (int i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    return 0;
}
```

输出结果是 `1 2 4 5 8`。冒泡排序写起来比较简单，不过平均时间复杂度是 `O(n²)`，数据量大时效率不高。

## 7. #define 和 const 的区别

`#define` 是预处理指令，主要用来做文本替换，例如：

```c
#define MAX 100
```

`const` 用来声明不能通过该标识符修改的对象，例如：

```c
const int max = 100;
```

主要区别是：`#define` 本身没有变量类型，而 `const` 声明的对象有类型。两者用法不完全一样，在 C 语言中，`const int` 也不能直接当成所有场合下的整数常量表达式。

## 8. 返回局部变量地址的问题

下面这段代码有问题：

```c
int fun(int a)
{
    int b = 10;
    return &b;
}
```

首先，函数返回类型是 `int`，但 `&b` 是 `int *`，类型不匹配。其次，`b` 是局部变量，函数返回后它的生命周期就结束了，不能返回它的地址让外面继续使用。

如果只是想返回变量的值，可以写成：

```c
int fun(int a)
{
    int b = 10;
    return b;
}
```

## 9. extern 的作用

在多个源文件组成的程序中，`extern` 可以声明在其他文件中定义的全局变量。

例如，在 `a.c` 中定义：

```c
int count = 10;
```

在 `b.c` 中声明并使用：

```c
#include <stdio.h>
extern int count;

void show(void)
{
    printf("%d\n", count);
}
```

`extern int count;` 通常表示变量在别处定义。文件作用域下的 `int count;` 则是暂定定义，如果本文件里没有其他定义，通常会成为一个定义。所以一般是在一个 `.c` 文件里定义变量，再通过头文件中的 `extern` 声明给其他文件使用，避免重复定义。

## 10. volatile 有什么用

`volatile` 用来告诉编译器，这个对象的值可能会在程序当前代码看不到的情况下发生变化，因此不能像普通对象一样随意省略或合并对它的访问。

它常见于硬件寄存器等场景，也可能用于某些中断相关代码。例如：

```c
volatile int status;
```

不过要注意，`volatile` 不能代替多线程同步，也不能保证操作是原子的。

## 11. #include 两种写法的区别

```c
#include "my_utils.h"
#include <stdio.h>
```

双引号形式通常会先查找当前源文件所在目录，再查找其他配置的目录；尖括号形式通常从编译器配置的头文件目录开始查找。

一般来说，自己写的头文件用双引号，标准库头文件用尖括号。不过实际查找路径也可能受编译器设置影响。

---

